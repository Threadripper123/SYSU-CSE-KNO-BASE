# vLLM 架构分析（博客园）

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　11,230 字符　｜　113 个片段　｜　更新于 2026-11-13
来源: https://cnblogs.com/wildkid1024/p/17682483.html

会员

周边

新闻

博问

闪存

赞助商

Chat2DB

所有博客

当前博客

我的博客
我的园子
账号设置
会员中心

简洁模式...

退出登录

注册
登录

wildkid1024

平平无奇！

博客园

首页

新随笔

联系

订阅

管理

[vllm]vllm架构分析

vllm架构分析

文件目录结构

benchmark: 测试延迟和吞吐的脚本

csrc: torch下的cuda扩展，一些关键kernels的cpp源码，包含了attention、激活函数、cache等核函数

vllm/core: 关键调度算法，调度策略以及维护cpu和gpu映射的关系表

vllm/engine: llm的engine，包含模型配置，启动模型，请求的前后处理等

vllm/entrypoints: 纯模型生成部分，只包含模型的prompt到token的这部分

vllm/model_executor: 模型op到layer到model组成部分以及包含了各模型的配置

vllm/transformers_utils: tokenizer的一些配置

vllm/worker: 负责分布式调度以及cache的分配

bloclk: 逻辑块和物理块的定义以及基本操作

剩下的是一些配置和工具函数

关键源码分析

不难看出，vllm/core，vllm/engine，vllm/worker是vllm架构中的不可缺少的部分，下面来逐一分析。

vllm/core

vllm/core中存放的是block调度算法，分为block_manager、Policy、Scheduler等三部分，其中block_manager包含了逻辑块和物理块的相互映射，Policy目前还是一个简单的FCFS策略，Scheduler中则包含了根据调度策略进行block调度的具体实现。

这里需要注意的是，在vllm/core中的所有分配的block是block mapping，在模型实际执行时才会调用cache engine进行内存搬运和运行。

block管理

block管理的主要功能是建立对应seq_id所对应cpu_block和gpu_block的映射关系，并处理block的分配释放等操作。

首先是block_manager，其中包含了BlockAllocator类，主要作用是分配和释放block，使用一个list管理blcok的上限长度，使用引用计数判断block是否为空。

BlockSpaceManager类是block管理的重要类，管理blcok的swap_in、swap_out、free、append_slot、allocate等操作。

首先是allocate函数，其生成与logic_token_block相对应数量的gpu_blocks,并将block的引用计数增加到group_num，更新block_table对应的是seq_id对应的物理块。

对于append_slot函数，则为新添加的token分配新的blcok，这里有个copy on write的优化，这里没太看明白，似乎是将原有的block保留，并返回新的block。

free操作则是将seq_id对应的所有block清空，引用计数-1,减为0则标记为空闲。

对于swap_in函数，则是将cpu_block从对应的seq中取出，并将cpu_block改为gpu_block，相同的block引用计数+1,释放cpu_block内存，然后更新block_table,swap_out操作与此类似，只不过使用了cpu_block和gpu_block的位置相反。

def swap_in(self, seq_group: SequenceGroup) -> Dict[int, int]:
# CPU block -> GPU block.
mapping: Dict[PhysicalTokenBlock, PhysicalTokenBlock] = {}
for seq in seq_group.get_seqs():
if seq.is_finished():
continue
new_block_table: BlockTable = []
block_table = self.block_tables[seq.seq_id]

for cpu_block in block_table:
if cpu_block in mapping:
gpu_block = mapping[cpu_block]
gpu_block.ref_count += 1
else:
gpu_block = self.gpu_allocator.allocate()
mapping[cpu_block] = gpu_block
new_block_table.append(gpu_block)
# Free the CPU block swapped in to GPU.
self.cpu_allocator.free(cpu_block)
self.block_tables[seq.seq_id] = new_block_table

block_number_mapping = {
cpu_block.block_number: gpu_block.block_number
for cpu_block, gpu_block in mapping.items()
}
return block_number_mapping

Scheduler

Scheduler是block调度的具体核心实现，包含了waitting、running、swapped等3种状态列表，3种状态列表可以相互转换，新放进来的seq会放进waitting队列，每一次_schedule，如果swapped为空，则直接将waiting队列添加到running队列，如果swapped队列不为空，则从running队列找到优先级较低的seq，_preempt较低优先级的seq，_preempt有两种模式，一种是recompute会将当前seq对应的block置换出，等用到时再重新计算，另外一种是swapped则是先放在swapped队列中，也就是cpu block中，等有需要再调度到gpu。函数上主要包含了add_seq_group添加请求、abort_seq_group中断请求、schedule调度、update根据已输出token更新调度器、free_seq释放请求资源等操作。

重点看_schedule函数，其主要功能是从waiting队列中找到符合token限制的请求，添加到running队列中，然后在scheduled中添加对应的seq_group。如果swapped队列不为空，则从running队列中找到最低优先级的seq,然后添加到preempted标记。swapped队列类似，只不过只有当swapped_out队列为空时才开始调度，最后将调度好的seq放入到running队列中进行后续调度。

def _schedule(self) -> SchedulerOutputs:
# Blocks that need to be swaped or copied before model execution.
blocks_to_swap_in: Dict[int, int] = {}
blocks_to_swap_out: Dict[int, int] = {}
blocks_to_copy: Dict[int, List[int]] = {}

# Fix the current time.
now = time.time()

# Join waiting sequences if possible.
if not self.swapped:
ignored_seq_groups: List[SequenceGroup] = []
scheduled: List[SequenceGroup] = []
num_batched_tokens = 0
# Optimization: We do not sort the waiting queue since the preempted
# sequence groups are added to the front and the new sequence groups
# are added to the back.
while self.waiting:
seq_group = self.waiting[0]

num_prompt_tokens = seq_group.get_seqs()[0].get_len()
prompt_limit = min(
self.scheduler_config.max_model_len,
self.scheduler_config.max_num_batched_tokens)
if num_prompt_tokens > prompt_limit:
logger.warning(
f"Input prompt ({num_prompt_tokens} tokens) is too long"
f" and exceeds limit of {prompt_limit}")
for seq in seq_group.get_seqs():
seq.status = SequenceStatus.FINISHED_IGNORED
ignored_seq_groups.append(seq_group)
self.waiting.pop(0)
break

# If the sequence group cannot be allocated, stop.
if not self.block_manager.can_allocate(seq_group):
break

# If the number of batched tokens exceeds the limit, stop.
if (num_batched_tokens + num_prompt_tokens >
self.scheduler_config.max_num_batched_tokens):
break

# The total number of sequences in the RUNNING state should not
# exceed the maximum number of sequences.
num_new_seqs = seq_group.num_seqs(
status=SequenceStatus.WAITING)
num_curr_seqs = sum(
seq_group.num_seqs(status=SequenceStatus.RUNNING)
for seq_group in self.running)
if (num_curr_seqs + num_new_seqs >
self.scheduler_config.max_num_seqs):
break

seq_group = self.waiting.pop(0)
self._allocate(seq_group)
self.running.append(seq_group)
num_batched_tokens += num_prompt_tokens
scheduled.append(seq_group)

if scheduled:
scheduler_outputs = SchedulerOutputs(
scheduled_seq_groups=scheduled,
prompt_run=True,
num_batched_tokens=num_batched_tokens,
blocks_to_swap_in=blocks_to_swap_in,
blocks_to_swap_out=blocks_to_swap_out,
blocks_to_copy=blocks_to_copy,
ignored_seq_groups=ignored_seq_groups,
)
return scheduler_outputs

# NOTE(woosuk): Preemption happens only when there is no available slot
# to keep all the sequence groups in the RUNNING state.
# In this case, the policy is responsible for deciding which sequence
# groups to preempt.
self.running = self.policy.sort_by_priority(now, self.running)

# Reserve new token slots for the running sequence groups.
running: List[SequenceGroup] = []
preempted: List[SequenceGroup] = []
while self.running:
seq_group = self.running.pop(0)
while not self.block_manager.can_append_slot(seq_group):
if self.running:
# Preempt the lowest-priority sequence groups.
victim_seq_group = self.running.pop(-1)
self._preempt(victim_seq_group, blocks_to_swap_out)
preempted.append(victim_seq_group)
else:
# No other sequence groups can be preempted.
# Preempt the current sequence group.
self._preempt(seq_group, blocks_to_swap_out)
preempted.append(seq_group)
break
else:
# Append new slots to the sequence group.
self._append_slot(seq_group, blocks_to_copy)
running.append(seq_group)
self.running = running

# Swap in the sequence groups in the SWAPPED state if possible.
self.swapped = self.policy.sort_by_priority(now, self.swapped)
while self.swapped and not blocks_to_swap_out:
seq_group = self.swapped[0]
# If the sequence group has been preempted in this step, stop.
if seq_group in preempted:
break
# If the sequence group cannot be swapped in, stop.
if not self.block_manager.can_swap_in(seq_group):
break

# The total number of sequences in the RUNNING state should not
# exceed the maximum number of sequences.
num_new_seqs = seq_group.num_seqs(status=SequenceStatus.SWAPPED)
num_curr_seqs = sum(
seq_group.num_seqs(status=SequenceStatus.RUNNING)
for seq_group in self.running)
if (num_curr_seqs + num_new_seqs >
self.scheduler_config.max_num_seqs):
break

seq_group = self.swapped.pop(0)
self._swap_in(seq_group, blocks_to_swap_in)
self._append_slot(seq_group, blocks_to_copy)
self.running.append(seq_group)

num_batched_tokens = sum(
seq_group.num_seqs(status=SequenceStatus.RUNNING)
for seq_group in self.running)

scheduler_outputs = SchedulerOutputs(
scheduled_seq_groups=self.running,
prompt_run=False,
num_batched_tokens=num_batched_tokens,
blocks_to_swap_in=blocks_to_swap_in,
blocks_to_swap_out=blocks_to_swap_out,
blocks_to_copy=blocks_to_copy,
ignored_seq_groups=[],
)
return scheduler_outputs

update函数则是根据当前的找到已完成的block，将其空间释放，如果碰到beam search中的子seq，则将对应的空间释放，并将父seq fork为子seq。

def update(
self,
seq_outputs: Dict[int, SequenceOutputs],
) -> List[SequenceGroup]:
scheduled: List[SequenceGroup] = []
for seq_group in self.running:
for seq in seq_group.get_seqs(status=SequenceStatus.RUNNING):
if seq.seq_id in seq_outputs:
scheduled.append(seq_group)
break

# Update the scheduled sequences and free blocks.
for seq_group in scheduled:
# Process beam search results before processing the new tokens.
for seq in seq_group.get_seqs(status=SequenceStatus.RUNNING):
output = seq_outputs[seq.seq_id]
if seq.seq_id!= output.parent_seq_id:
# The sequence is a fork of the parent sequence (beam
# search). Free the current sequence.
self.block_manager.free(seq)
# Fork the parent sequence.
parent_seq = seq_group.find(output.parent_seq_id)
parent_seq.fork(seq)
self.block_manager.fork(parent_seq, seq)

# Process the new tokens.
for seq in seq_group.get_seqs(status=SequenceStatus.RUNNING):
# Append a new token to the sequence.
output = seq_outputs[seq.seq_id]
seq.append_token_id(output.output_token, output.logprobs)
return scheduled

add_seq_group、abort_seq_group、free_seq操作比较简单，如字面意思，不再做详细解读。

vllm/worker

worker负责对所有model和engine的封装，驱动模型进行输入输出以及block的管理。

CacheEngine负责管理分配管理KV Cache,调用自定义的cache算子在实际运行时调用copy、swap_in、swap_out等操作，是对cuda算子的封装。

vllm/engine

engine是scheduler、worker、cache engine的组合大类，是更进一步的封装，其中包含add_request对数据预处理之后放入scheduler队列，在step函数中则调用scheduler得到的swap_in、swap_out，通过驱动worker得到output，然后在调用scheduler的update函数对output值进行更新。

下面是step部分的源码：

def step(self) -> List[RequestOutput]:
"""Performs one decoding iteration and returns newly generated results.

This function performs one decoding iteration of the engine. It first
schedules the sequences to be executed in the next iteration and the
token blocks to be swapped in/out/copy. Then, it executes the model
and updates the scheduler with the model outputs. Finally, it decodes
the sequences and returns the newly generated results.
"""
seq_group_metadata_list, scheduler_outputs = self.scheduler.schedule()
if scheduler_outputs.is_empty():
if not scheduler_outputs.ignored_seq_groups:
# Nothing to do.
return []
# If there are ignored seq groups, we need to return them as the
# request outputs.
return [
RequestOutput.from_seq_group(seq_group)
for seq_group in scheduler_outputs.ignored_seq_groups
]

# Execute the model.
output = self._run_workers(
"execute_model",
seq_group_metadata_list=seq_group_metadata_list,
blocks_to_swap_in=scheduler_outputs.blocks_to_swap_in,
blocks_to_swap_out=scheduler_outputs.blocks_to_swap_out,
blocks_to_copy=scheduler_outputs.blocks_to_copy,
)
# Update the scheduler with the model outputs.
seq_groups = self.scheduler.update(output)

# Decode the sequences.
self._decode_sequences(seq_groups)
# Stop the sequences that meet the stopping criteria.
self._stop_sequences(seq_groups)
# Free the finished sequence groups.
self.scheduler.free_finished_seq_groups()

# Create the outputs.
request_outputs: List[RequestOutput] = []
for seq_group in seq_groups + scheduler_outputs.ignored_seq_groups:
request_output = RequestOutput.from_seq_group(seq_group)
request_outputs.append(request_output)

if self.log_stats:
# Log the system stats.
self._log_system_stats(scheduler_outputs.prompt_run,
scheduler_outputs.num_batched_tokens)
return request_outputs

逻辑还是很清晰的，首先调用scheduler.schedule()得到scheduler_outputs，scheduler_outputs中包含了swap_in、swap_out等映射，通过worker的execute_model得到逻辑输出，使用scheduler.update对输出进行更新，将使用完的block置换出。然后对已经完成的seq解码，更新seq状态，返回输出。

posted @
2023-09-06 15:42
wildkid1024
阅读( 2474 )
评论( 0 )

收藏
举报

刷新页面 返回顶部

公告

博客园
&copy; 2004-2026

浙公网安备 33010602011771号
浙ICP备2021040463号-3
