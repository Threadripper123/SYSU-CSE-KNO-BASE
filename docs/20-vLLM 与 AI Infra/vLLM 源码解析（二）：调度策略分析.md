# vLLM 源码解析（二）：调度策略分析

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　50,223 字符　｜　363 个片段　｜　更新于 2026-11-13
来源: https://blog.csdn.net/weixin_42479327/article/details/141614205

# https://blog.csdn.net/weixin_42479327/article/details/141614205

来源: https://blog.csdn.net/weixin_42479327/article/details/141614205
抓取方式: read_page

首页

博客

下载

学习

社区

模型市场

AI搜索

AtomGit

InsCode

技术会议

墨衍
MoGrow

收藏

历史

会员中心

创作中心

vllm源码解析(二)：调度策略分析

原创
2024-09-03 23:26:42
5.8k 阅读

AI写同款

收录于

当前文章被以下社区和专栏收录：

弈秋001

码龄8年

墨衍会员
·
AI 创作全网分发

墨衍智能分发
本文由作者通过墨衍一键同步至各平台

1 分发平台
5.8k 累计阅读
5.8k 平均单平台

已同步平台

CSDN
微信公众号
微博
知乎
掘金
百家号
博客园
抖音
小红书

你的文章也可以这样分发
墨衍会员 ¥399 起/年，写一次发全网

我也要 →

567粉丝 · 42篇原创

关注

深入剖析 vLLM ：大模型计算加速系列之调度器策略探索
从 vLLM 批处理的入口函数开始，介绍了其推理内核LLMEngine的两个重要函数和step ( ) 在LLMEngine开始处理请求前（实例化阶段），它会先做一次模拟实验，来估计gpu上需要预留多少显存给KV Cache block。当LLMEngine开始处理请求时 ( add_request ) ，它会把每个prompt当成一个请求，同时把它包装成一个SequenceGroup对象。

阅读详情

AI摘要

本文详细介绍了vllm的调度逻辑，包括调度系统整体运行规则。每次调度前初始化budget类对象管理tokens和seqs数量，根据阈值决定seq_groups去向。分析了调度判断入口为swapped的原因，还阐述了三个子调度方法，即对waiting、running、swapped队列的调度处理。

五 vllm调度逻辑

前面分享的知识确实遗漏了许多细节，不过不影响我们接下来的讲解，我们在 第一篇文章 中已经详细讲解过三个队列用途，不熟悉的同学可以回看。

本章涉及到的流程图已上传github:
https://github.com/yblir/packages/blob/main/vllm.vsdx

从下图可以看到调度系统整体运行规则：

vllm/core/scheduler.py：def _schedule_default（…），每次执行推理前都要走一遍调度。调度逻辑如下：

我们对调度逻辑做下详细分析：

每次执行调度前都要重新初始化一个budget类对象来 管理本次调度的tokens和seqs数量, 根据数量是否超过阈值，决定将本次seq_groups放入哪个队列(一个seq_groups会包含多个seqs)。budget对象在上图 三个调度方法中都有用到 ，用以判断正在处理的seq_group的去向。

我们来思考这样一个问题：调度的目的是让vllm每次推理都保持最大的吞吐量，而running队列中seq_groups才是送去做推理的，那么 为什么调度逻辑的判断入口是swapped而不是running呢？
让我们从社会学角度来考虑这个问题：
你有没有遇到过这种情况，某天，你老板（ 调度 ）来到你面前，跟你（ running ）说，亲，你的工作饱和吗（ 最大吞吐量 ），要不要给你再来点？我想你肯定没遇到过。真实的情况是，老板会直接把工作甩你脸上，工作不饱和你就干吧，没时间干（ gpu资源不足或处理数量超出阈值 ）就先积压起来（watiing or swapped），有时间再搞。
那么问题来了，你（running）工作的来源有两个（waiting，swapped），要从哪个队列拿数据呢？
稍微思考下就能知道，必定先考虑积压的（swapped），因为这是已经做过一部分的工作，如果不优先处理，它们会始终占据着你的脑容量（ 系统资源 ）; 而另一边，waiting队列则是完全没开始的新工作，当前也不存在任何资源占用问题，一比较就高下立判了吧！同时这也由调度原则决定： 先来的请求先被服务 ，swapped生命历程为：waiting -> running -> swapped, 从整个时间尺度上看，swapped肯定比现在处于waiting队列的seq_group来的更早。

如果swapped有积压的工作（非空），说明你已经在高强度工作，不能向running继续塞新工作了。同时要看看你的工作情况（_schedule_running）,看在接下来工作（ 下一次推理 ）是否有风险( 预期资源不足或tokens,seqs数量超出阈值 )，毕竟压榨打工人是为了创造更多价值，但若直接把打工人累死就啥都没了！（ gpu 爆显存 ）。如果swapped为空，只能从waiting队列拿数据了（ 使用_schedule_prefills方法调度 ）。

从waiting队列取出数据prefill后，先判断它的seq_groups数量，如果为非0，说明有取出seq_group， 再验证下budget阈值（tokens和seqs数量，_schedule_prefills方法内部已经对budget作过校验，此处仅是为了代码健壮性），没问题就能加入running队列，愉快地做推理去，至此，本次调度完结 。若waiting队列为空，没有seq_group可用，那么prefill的seq_group数量必定也为0。此时swapped和waiting队列都没有提供新的seq_group（ 此时swapped队列为空，但waiting队列不一定为空，也可能是当前系统资源紧张，因此跳过取数据的步骤，导致prefill.seq_groups为空 ），唯一还在活跃的队列是running。

_schedule_running处理的是running队列，这个队列里面存储的是 上一次执行推理 的seq_groups, 以前行不代表现在也可以，如经过推理后新生成token的kv-cache会占用部分资源， 导致系统资源不足以支撑running队列中的所有seq_groups再做一次推理 ，所以还要对running队列中的seq_group进行 校验 ，如果确实不能支撑，就要执行抢占操作。

接下来要判断是否存在抢占现象，如果发生抢占，就根据抢占类型转移到不同队列中，对抢占的处理在（一）有讲到。如果没有发生抢占，用_schedule_swapped调度方法 转移seq_group到running队列。 源码在这里没有对swapped队列是否为空做二次判断 ，也可以理解吧，毕竟如果队列为空，那就啥都不取呗！

现在获得seq_groups都是经过筛选的，能确保下一次推理时不会爆显存，还要做最后一次判断，确认没有超过调度器设置的推理时的阈值（tokens和seqs，基本可确定没问题，因为 三个调度方法中都有对阈值做判断 ，觉得这里的判断只是为了增加代码的健壮性）。之后把经过检验的所有seq_groups加入到running队列，这些才是 可用于下一次推理的seq_groups (成功上岸！)

def _schedule_default ( self ) - > SchedulerOutputs :
"""Schedule queued requests.
The current policy is designed to optimize the throughput. First,
it batches as many prefill requests as possible. And it schedules
decodes. If there's a pressure on GPU memory, decode requests can
be swapped or preempted.

当前策调度略旨在优化吞吐量。首先，它会批量处理尽可能多的预填充请求。然后它会安排解码。
如果 GPU 内存有压力，则可以交换或抢占解码请求。因此会优先从swapped进行判断。
"""
# Include running requests to the budget.
# 每次step都要重新初始化一个budget来管理本次调度的的tokens和seqs数量, 根据数量是否超过阈值，决定将本次
# seq_groups放入哪个队列。(一个seq_groups会包含多个seqs)
budget = SchedulingBudget (
token_budget = self. scheduler_config. max_num_batched_tokens,
max_num_seqs = self. scheduler_config. max_num_seqs,
)

# Make sure we include num running seqs before scheduling prefill,
# so that we don't schedule beyond max_num_seqs for prefill.
# 先统计正在执行推理的seq_groups中seq的数量
for seq_group in self. running :
budget. add_num_seqs ( seq_group. request_id, seq_group. get_max_num_running_seqs ( ) )
# lora推理相关，可忽略
curr_loras = set (
seq_group. lora_int_id for seq_group in self. running
if seq_group. lora_int_id > 0 ) if self. lora_enabled else None

# 以下三个变量，类似于C++中的结构体。将多个变量合在一起，通过.属性访问
# 各自保存处于不同活跃状态(wait,run,swap)的seq_groups具有的属性
prefills = SchedulerPrefillOutputs. create_empty ( )
running_scheduled = SchedulerRunningOutputs. create_empty ( )
swapped_in = SchedulerSwappedInOutputs. create_empty ( )

# If any requests are swapped, prioritized swapped requests.
# 为什么要从swap开始判断？
# 调度的任务是优化吞吐量，即保证处于running状态的seqs最多。running从wait和swap队列
# 获得，首先积压的任务可能要比wait的优先级高，因为swap队列中的任务始终占据着系统资源，当
# running可添加时，应该首先处理swap。
if not self. swapped : # 如果swapped队列为空
# 既然不能从swap想running转移，那就只能从wait队列拿任务了。
# wait队列中的都是原始任务，第一步要预填充
# prefills是一个伪结构体：可以.出以下属性
# seq_groups: List[SequenceGroup]
# ignored_seq_groups: List[SequenceGroup]
# num_lookahead_slots: int
prefills = self. _schedule_prefills ( budget, curr_loras, enable_chunking = False )

# Don't schedule decodes if prefills are scheduled.
# NOTE: If `_schedule_prefills` doesn't enable chunking, self.running
# only contains decode requests, not chunked prefills.

# self.waiting空,或 self.swapped非空,都会导致prefills.seq_groups数量为0
# # 这个判断的意思是,prefills.seq_groups==0,说明本次调度没有安排预填充任务,那么就安排解码任务.
# 执行推理任务的seq_group都在running队列，因此需要对这个队列进行调度。
# 调度什么呢？
# 是看running队列中的seq_group是否可以继续做推理任务。因为vllm动态管理，最大限度优化吞吐量，会导致blocks资源紧张
# 上次推理生成的tokens的kv-cache需要GPU blocks去存储，导致资源消耗。那么这次准备推理时blocks数量不一定能够它完成
# 推理，所以要对running队列中每个seq_group进行检查，看是否可以进行做推理。
if len ( prefills. seq_groups ) == 0 :
running_scheduled = self. _schedule_running ( budget, curr_loras, enable_chunking = False )

# If any sequence group is preempted, do not swap in any sequence
# group. because it means there's no slot for new running requests.
# 在对running队列调度后(从self.running队列取seq_group,准备进行推理),如果没有seq_group被
# 抢占(退回wait队列),也没有seq_group被转移到CPU上, 说明blocks资源充足,可以把以前
# self.swapped队列中积压的seq_group转移到gpu blocks做推理.

# 注意这几个队列的判断逻辑. 如果self.swapped原本就非空,会进入上面的if判断分支进行self.running队列
# 调度取值.然后根据这个过程中是否有seq_group被preempted和swapped_out获知blocks资源使用情况.
# 如果没有被preempted和swapped_out.说明工作不饱和，就把self.swapped内容取出来，加入running队列进行推理
# 如果有被preempted和swapped_out，说明资源紧张. self.swapped积压的任务暂时还处理不了
# 如果下面if为False,意思是就不要再从self.swapped转移seq_group会gpu做推理了
if len ( running_scheduled. preempted ) + len ( running_scheduled. swapped_out ) == 0 :
swapped_in = self. _schedule_swapped ( budget, curr_loras )

# 最后一次判断本次推理的tokens和seqs数量是否超过阈值
assert budget. num_batched_tokens <= self. scheduler_config. max_num_batched_tokens
assert budget. num_curr_seqs <= self. scheduler_config. max_num_seqs

# Update waiting requests.
# 这个类型被抢占的seq_group，打回原型，重新加入waiting队列。
# 幸运的是添加到了队列头部，当再次从waiting队列取数据时，会优先处理它
self. waiting. extendleft ( running_scheduled. preempted )
# Update new running requests.
# 将以上通过层层筛选的seq_group加入到running队列(真·running)，这些seq_group才是下一步的推理对象
self. running. extend ( [ s. seq_group for s in prefills. seq_groups ] )
self. running. extend ( [ s. seq_group for s in running_scheduled. decode_seq_groups ] )
self. running. extend ( [ s. seq_group for s in swapped_in. decode_seq_groups ] )
# Update swapped requests.
# 没有足够资源做推理的seq_group会从running转移到swap队列(swap队列是路径之一，另一个是加入到wait队列)
self. swapped. extend ( running_scheduled. swapped_out )
# 统计被抢占的seq_group数量
preempted = len ( running_scheduled. preempted ) + len ( running_scheduled. swapped_out )

# There should be no prefill from running queue because this policy
# doesn't allow chunked prefills.
assert len ( running_scheduled. prefill_seq_groups ) == 0
assert len ( swapped_in. prefill_seq_groups ) == 0

return SchedulerOutputs (
scheduled_seq_groups = ( prefills. seq_groups +
running_scheduled. decode_seq_groups +
swapped_in. decode_seq_groups ),
num_prefill_groups = len ( prefills. seq_groups ),
num_batched_tokens = budget. num_batched_tokens,
blocks_to_swap_in = swapped_in. blocks_to_swap_in,
blocks_to_swap_out = running_scheduled. blocks_to_swap_out,
blocks_to_copy = running_scheduled. blocks_to_copy +
swapped_in. blocks_to_copy,
ignored_seq_groups = prefills. ignored_seq_groups +
swapped_in. infeasible_seq_groups,
num_lookahead_slots = running_scheduled. num_lookahead_slots,
running_queue_size = len ( self. running ),
preempted = preempted,
)

从整体调度逻辑中我们可以看到，核心代码是三处子调度方法，接下来我们来详细分析下这些方法的代码。

5.1 _schedule_prefills调用逻辑

对waiting队列的调度充斥着各种判断，核心只有一条： 当把取出seq_group加入running队列进行推理时，系统资源是否够用 。
基于此，有以下几种处理方式：

准备取出的seq_group 超出 prompt长度阈值 ，标记为finish，以后不再处理
准备取出的seq_group 需求的blocks 数量超出阈值 ，两种处理：①标记为finish，以后不再处理，② 不再取出seq_group，留待以后再处理，跳出调度流程。
准备取出的seq_group tokens或seq数量超出推理能承受的阈值，不再取出seq_group，留待以后再处理，跳出调度流程

def _schedule_prefills (
self,
budget : SchedulingBudget,
curr_loras : Optional [ Set [ int ] ],
enable_chunking : bool = False,
) - > SchedulerPrefillOutputs :
# ignored_seq_groups：记录因太长（所需的blocks和总blocks之间的差值超过阈值了），
# 而无法继续做生成的seq_group，这些seq_group中的seq状态都会被标记为
# FINISHED_IGNORED，表示直接不处理他们
ignored_seq_groups : List [ SequenceGroup ] = [ ]
# 用于装载从wait队列转移出来的seq_group
seq_groups : List [ SequenceGroup ] = [ ]

waiting_queue = self. waiting

leftover_waiting_sequences : Deque [ SequenceGroup ] = deque ( )
# self._passed_delay：通过比较当前请求到达时间来确定是否要从wait队列拿任务
# 因为拿任务也要占用时间，需要平衡调度任务与推理任务的调用时机
while self. _passed_delay ( time. time ( ) ) and waiting_queue :
seq_group = waiting_queue [ 0 ]
# list,从当前seq_group取出准备推理的seq序列. 1个prompt可能有多个seq(多输出)，但wait队列中连预填充
# 都没进行，因此这时的seq(仅是prompt)数量必定==1
waiting_seqs = seq_group. get_seqs ( status = SequenceStatus. WAITING )
assert len ( waiting_seqs ) == 1, "Waiting sequence group should have only one prompt sequence."

# 当前待推理的seq_group需要处理,或者说准备返回的tokens数量,
# 对于WAITING状态，只有1个seq，tokens数量为prompt长度
num_new_tokens = self. _get_num_new_tokens ( seq_group,
SequenceStatus. WAITING,
enable_chunking, budget )
if not enable_chunking :
num_prompt_tokens = waiting_seqs [ 0 ]. get_len ( )
# 从waiting取出的seq_group，连预填充都没做，更不会有output token，
# 若计算出的tokens数量不等与prompt数量，一定有问题，抛出异常吧！
assert num_new_tokens == num_prompt_tokens

# 如果这条seq的长度 > 每次调度能处理的最大序列长度，那么把这条seq的状态置为FINISHED_IGNORED，
# 并将对应seq_group装入ignored_seq_groups中，然后将其从waiting列表中移除，永不再处理，完结撒花~
prompt_limit = self. _get_prompt_limit ( seq_group )
if num_new_tokens > prompt_limit :
logger. warning (
"Input prompt (%d tokens) is too long"
" and exceeds limit of %d", num_new_tokens, prompt_limit )
for seq in waiting_seqs :
seq. status = SequenceStatus. FINISHED_IGNORED
ignored_seq_groups. append ( seq_group ) # 加入失败者联盟
waiting_queue. popleft ( ) # 从 waiting 队列中踢出去
continue # 继续从waiting拿数据处理

# If the sequence group cannot be allocated, stop.
# 比较当前seq需要的物理块,gpu可用物理块之间的数量关系. 决定是否能给当前seq_group分配物理块
# can_allocate返回值可能有三种： NEVER：不分配；OK：可以分配；LATER：延迟分配
can_allocate = self. block_manager. can_allocate ( seq_group )
if can_allocate == AllocStatus. LATER :
break
# 当前seq需要的blocks数量,超过gpu能提供的最大数量.加入失败者联盟,永不再处理，
elif can_allocate == AllocStatus. NEVER :
logger. warning (
f"Input prompt ( { num_new_tokens } tokens) is too long"
" and exceeds the capacity of block_manager" )
for seq in waiting_seqs :
seq. status = SequenceStatus. FINISHED_IGNORED
ignored_seq_groups. append ( seq_group )
waiting_queue. popleft ( )
continue

# lora相关，忽略
lora_int_id = 0
if self. lora_enabled :
lora_int_id = seq_group. lora_int_id
assert curr_loras is not None
assert self. lora_config is not None
if ( self. lora_enabled and lora_int_id > 0
and lora_int_id not in curr_loras
and len ( curr_loras ) >= self. lora_config. max_loras ) :
# We don't have a space for another LoRA, so
# we ignore this request for now.
leftover_waiting_sequences. appendleft ( seq_group )
waiting_queue. popleft ( )
continue

# 当前seq_group中状态为 未执行完 的序列的数量，即seq还没推理完成的数量. 刚从wait中取出时，
# seq数量是1,但推理生成阶段,这个seq_group中会有n个seq在并行.n是外部传入的output数量. 因此这里num_new_seqs==n
num_new_seqs = seq_group. get_max_num_running_seqs ( )
# budget.can_schedule同时判断tokens和seqs数量是否超过阈值，任一个超过单次调度能执行的总数的阈值
# 说明这step可推理的seqs数量已经马上趋于饱和，不能再加入seq到running队列。跳出while, 结束本次waiting向running的调度
if num_new_tokens == 0 or not budget. can_schedule ( num_new_tokens = num_new_tokens,
num_new_seqs = num_new_seqs ) :
break

# Can schedule this request.
# lora相关，忽略
if curr_loras is not None and lora_int_id > 0 :
curr_loras. add ( lora_int_id )

# 走到这一步时，说明当前seq_group已经通过上述种种验证，可以被加入running队列进行推理
# 先将其从waiting队列中移出
waiting_queue. popleft ( )
# 为当前seq_group分配物理块,并将该seq_group中每条seq的status从waiting改为running
self. _allocate_and_set_running ( seq_group )

# ScheduledSequenceGroup类似于C++结构体。仅包含seq_group和token_chunk_size两个变量
# 搞不懂vllm为什么总喜欢这种包裹操作，在各处代码中随处可见。用基本的list,或dict不好吗！
seq_groups. append (
ScheduledSequenceGroup ( seq_group = seq_group,
token_chunk_size = num_new_tokens ) )

# 当前seq_group的tokens和seqs数量增加到预算budget中
budget. add_num_batched_tokens ( seq_group. request_id, num_new_tokens )
budget. add_num_seqs ( seq_group. request_id, num_new_seqs )

# Queue requests that couldn't be scheduled.
# 和lora相关的操作，忽略
waiting_queue. extendleft ( leftover_waiting_sequences )
if len ( seq_groups ) > 0 :
self. prev_prompt = True

return SchedulerPrefillOutputs (
seq_groups = seq_groups,
ignored_seq_groups = ignored_seq_groups,
num_lookahead_slots = self. _get_num_lookahead_slots ( is_prefill = True ) )

5.2 _schedule_running调度方法

running队列存储时上次做推理的seq_group，下次推理前， 要判断下 是否还可以继续做推理 ，如果不能，就要从队列移出来。
会转移到waiting或swapped队列 ，转移规则我们第一篇博客中已经做了详细说明。

def _schedule_running (
self,
budget : SchedulingBudget,
curr_loras : Optional [ Set [ int ] ],
enable_chunking : bool = False,
) - > SchedulerRunningOutputs :
# Blocks that need to be swapped or copied before model execution.
# todo 类型变了
# blocks_to_swap_out：{gpu物理块id: cpu物理块id}
# blocks_to_copy: {旧物理块id：[由旧物理块copy-on-write而来的新物理块id]}
blocks_to_swap_out : List [ Tuple [ int, int ] ] = [ ]
blocks_to_copy : List [ Tuple [ int, int ] ] = [ ]

decode_seq_groups : List [ ScheduledSequenceGroup ] = [ ]
prefill_seq_groups : List [ ScheduledSequenceGroup ] = [ ]
preempted : List [ SequenceGroup ] = [ ]
swapped_out : List [ SequenceGroup ] = [ ]

# NOTE(woosuk): Preemption happens only when there is no available slot
# to keep all the sequence groups in the RUNNING state.

running_queue = self. running

while running_queue :
seq_group = running_queue [ 0 ]
# 当前待推理的seq_group需要处理,或者说准备返回的tokens数量,对于RUNNING状态，每个seq返回1个token
num_running_tokens = self. _get_num_new_tokens (
seq_group, SequenceStatus. RUNNING, enable_chunking, budget )

# todo 觉得这个判断有点多余,因为处于RUNNING状态的seq,必定有tokens返回,prompt总不能为空吧，num_running_tokens
# todo 不可能为0, 再说,如果为0, 方法self._get_num_new_tokens内部就会抛出异常,因为做了assert断言
if num_running_tokens == 0 :
break

# 经过num_running_tokens检验没问题后, 将该seq_group从running_queue中取出来
running_queue. popleft ( )

# 对于这个seq_group，检查对于其中的每一个seq，是否能至少分配一个物理块给它，如果不能的话
# （说明要执行抢占操作了，否则马上会没有资源让这个最早到达的seq_group做完推理）：
# 这里用了while...else，如果while条件正常结束，则进入else内容；如果被break，则不会执行else
while not self. _can_append_slots ( seq_group ) : # 如果不能为当前seq_group的每个seq都分配一个block
# 这个seq_group本来是要送去做推理的,但没有足够的gpu物理blocks分配给它
# 根据vllm的调度原则，这个seq_group要被优先处理，没有足够资源，就把running队列最后位置的
# seq_group踢出去，释放gpu blocks给当前seq_group使用。

# seq_group准备返回的tokens数量已经加到budget属性上,现在不处理它, 要把数量再减回来
# budget会记录每次+-数量的seq_group.request_id,如果以前没被+过，现在就不会被-，就像下面的调用一样
budget. subtract_num_batched_tokens ( seq_group. request_id, num_running_tokens )
# 在外层总调度中，已经在budget汇总了所有正在活跃的seqs数量，现在要减去属于该seq_group的seqs数量
num_running_seqs = seq_group. get_max_num_running_seqs ( )
budget. subtract_num_seqs ( seq_group. request_id, num_running_seqs )

# lora相关,忽略
if ( curr_loras is not None and seq_group. lora_int_id > 0
and seq_group. lora_int_id in curr_loras ) :
curr_loras. remove ( seq_group. lora_int_id )

# ------------------------------------------------------------------------------------------------------
# 经过以下释放gpu blocks工作后,再次进入while循环判断gpu blocks数量是否够用,
# 如果够用,进入到与while对应的else分支,如果不够用,继续释放gpu blocks,直到够用或running_queue全部取完.
# ------------------------------------------------------------------------------------------------------

# 如果此时running_queue队列不为空,把最后一个seq_group踢出去放入swap队列,给
# 上面这个seq_group腾位置(释放最后一个seq_group对应的gpu blocks)
if running_queue :
# Preempt the lowest-priority sequence groups.
victim_seq_group = running_queue. pop ( )

# 有两种swap方式,RECOMPUTE:删除所有,回炉到waiting队列. SWAP:将blocks全部转移到CPU blocks上
preempted_mode = self. _preempt ( victim_seq_group, blocks_to_swap_out )
if preempted_mode == PreemptionMode. RECOMPUTE :
preempted. append ( victim_seq_group )
else :
swapped_out. append ( victim_seq_group )
# 如果running_queue队列已经空了,没有替罪的羊,只能把自己放入swap队列了.
else :
# No other sequence groups can be preempted.
# Preempt the current sequence group.
preempted_mode = self. _preempt ( seq_group, blocks_to_swap_out )
if preempted_mode == PreemptionMode. RECOMPUTE :
preempted. append ( seq_group )
else :
swapped_out. append ( seq_group )
# 此时running_queue队列已空,已经没有seq_group可处理了,使用break中断
# while循环, 不走后面的else分支,直接return,而且本次调度没有指定任何待推理的seq_group
break
else :
# 为当前seq_group分配gpu 物理blocks. 这里只分配了逻辑blocks与物理blocks的映射关系
# blocks_to_copy:[旧物理块id, copy - on - write而来的新物理块id]
self. _append_slots ( seq_group, blocks_to_copy )
is_prefill = seq_group. is_prefill ( )
if is_prefill :
prefill_seq_groups. append ( ScheduledSequenceGroup ( seq_group = seq_group,
token_chunk_size = num_running_tokens ) )
else :
decode_seq_groups. append ( ScheduledSequenceGroup ( seq_group = seq_group,
token_chunk_size = 1 ) )
# todo 这似乎是个bug, 如果_can_append_slots为True，会跳过while直接走当前else分支
# todo seqs在外层_schedule_default已经更新过，所以这里只更新tokens就好
# todo 但是，如果_can_append_slots为False，budget会同时减去seq_group的tokens和seqs数量
# todo 下面把tokens再加回来，逻辑没问题，但没有更新seqs！
budget. add_num_batched_tokens ( seq_group. request_id, num_running_tokens )
# OPTIMIZATION: Note that get_max_num_running_seqs is
# expensive. For the default scheduling chase where
# enable_chunking is False, num_seqs are updated before running
# this method, so we don't have to update it again here.
# 默认情况下, 以下两个if都走不到
if enable_chunking :
num_running_seqs = seq_group. get_max_num_running_seqs ( )
budget. add_num_seqs ( seq_group. request_id, num_running_seqs )
if curr_loras is not None and seq_group. lora_int_id > 0 :
curr_loras. add ( seq_group. lora_int_id )

return SchedulerRunningOutputs (
decode_seq_groups = decode_seq_groups,
prefill_seq_groups = prefill_seq_groups,
preempted = preempted,
swapped_out = swapped_out,
blocks_to_swap_out = blocks_to_swap_out,
blocks_to_copy = blocks_to_copy,
num_lookahead_slots = self. _get_num_lookahead_slots ( is_prefill = False ) )

5.3 _schedule_swapped调度方法

swapped -> running, 这个调度比较简单，原则就一条： 系统是否有足够的资源让它回来做推理 。

def _schedule_swapped (
self,
budget : SchedulingBudget,
curr_loras : Optional [ Set [ int ] ],
enable_chunking : bool = False,
) - > SchedulerSwappedInOutputs :
# Blocks that need to be swapped or copied before model execution.
# [(cpu物理块id, gpu物理块id)]
blocks_to_swap_in : List [ Tuple [ int, int ] ] = [ ]
# [(旧物理块,copy - on - write而来的新物理块id)]
blocks_to_copy : List [ Tuple [ int, int ] ] = [ ]
# 准备解码的seq_group
decode_seq_groups : List [ ScheduledSequenceGroup ] = [ ]
# 准备预填充的seq_group
prefill_seq_groups : List [ ScheduledSequenceGroup ] = [ ]
# 因各种原因，被标记为不再处理的seq_group，如预填充序列太长了...
infeasible_seq_groups : List [ SequenceGroup ] = [ ]

swapped_queue = self. swapped

leftover_swapped : Deque [ SequenceGroup ] = deque ( )
while swapped_queue :
# 取出swap队列中最早被抢占的seq_group
seq_group = swapped_queue [ 0 ]

# ----------------------------------------------------------------------------------------------------------
# If the sequence group cannot be swapped in, stop.
# 对被抢占seq_group有两种处理方式，1. 清空放入waiting队列，这时is_prefill为True
# 2.blocks全部转移到CPU上，这时is_prefill为False
# self._get_num_lookahead_slots(is_prefill)必定为0，否则抛出异常，block_manager_v1不支持非0情况
is_prefill = seq_group. is_prefill ( )
# 根据需要的，与可用的物理blocks数量判断，是否可以把当前seq_group从swap队列转移到running队列
alloc_status = self. block_manager. can_swap_in (
seq_group, self. _get_num_lookahead_slots ( is_prefill ) )
if alloc_status == AllocStatus. LATER : # 稍后，资源多时再处理
break
elif alloc_status == AllocStatus. NEVER : # 不合格，永不再处理
logger. warning (
"Failing the request %s because there's not enough kv "
"cache blocks to run the entire sequence.",
seq_group. request_id )
for seq in seq_group. get_seqs ( ) :
seq. status = SequenceStatus. FINISHED_IGNORED
infeasible_seq_groups. append ( seq_group )
swapped_queue. popleft ( )
continue
# ----------------------------------------------------------------------------------------------------------

# lora相关，忽略
lora_int_id = 0
if self. lora_enabled :
lora_int_id = seq_group. lora_int_id
assert curr_loras is not None
assert self. lora_config is not None
if ( lora_int_id > 0 and ( lora_int_id not in curr_loras )
and len ( curr_loras ) >= self. lora_config. max_loras ) :
# We don't have a space for another LoRA, so
# we ignore this request for now.
leftover_swapped. appendleft ( seq_group )
swapped_queue. popleft ( )
continue

# The total number of sequences in the RUNNING state should not
# exceed the maximum number of sequences.
# 取出这个seq_group在剩余生命周期内将并行运行的最大seq数量
num_new_seqs = seq_group. get_max_num_running_seqs ( )
# 当前准备转移的seq_group,需要处理,或者说准备返回的tokens数量，
# decode模式：每个seq num_token=1,其他模式则遵循各自的状态
num_new_tokens = self. _get_num_new_tokens ( seq_group,
SequenceStatus. SWAPPED,
enable_chunking, budget )
# 感觉num_new_tokens==0的判断有点多余，基本不可能为0
# budget.can_schedule 会判断加上当前seq_group的num_new_tokens和num_new_seqs后
# 总数是否会超标，如果超标，说明不能再添加任何seq_group到running队列，直接结束本次调度
if ( num_new_tokens == 0
or not budget. can_schedule ( num_new_tokens = num_new_tokens, num_new_seqs = num_new_seqs ) ) :
break

if lora_int_id > 0 and curr_loras is not None :
curr_loras. add ( lora_int_id )

# 如果能走到这步，说明可向running队列转移了。先把当前seq_group从swap队列踢出来
# 再把CPU上的blocks转移到GPU block上
swapped_queue. popleft ( )
self. _swap_in ( seq_group, blocks_to_swap_in )
self. _append_slots ( seq_group, blocks_to_copy )
# 判断是不是预填充，将这个seq_group加入不同的分组
is_prefill = seq_group. is_prefill ( )
if is_prefill :
prefill_seq_groups. append ( ScheduledSequenceGroup ( seq_group, token_chunk_size = num_new_tokens ) )
else :
decode_seq_groups. append ( ScheduledSequenceGroup ( seq_group, token_chunk_size = 1 ) )

# 将这个马上上岸的seq_group的tokens和seqs数量更新到budget中
budget. add_num_batched_tokens ( seq_group. request_id, num_new_tokens )
budget. add_num_seqs ( seq_group. request_id, num_new_seqs )

swapped_queue. extendleft ( leftover_swapped )

return SchedulerSwappedInOutputs (
decode_seq_groups = decode_seq_groups,
prefill_seq_groups = prefill_seq_groups,
blocks_to_swap_in = blocks_to_swap_in,
blocks_to_copy = blocks_to_copy,
num_lookahead_slots = self. _get_num_lookahead_slots (
is_prefill = False ),
infeasible_seq_groups = infeasible_seq_groups,
)

标签

#人工智能
#transformer
#自然语言处理
#chatgpt
#语言模型

#gpt-3
#深度学习

版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。

解密 vLLM ：基于nano - vllm 源码 剖析其推理加速之道
vLLM 对于部署过大模型的人来说应该都不算陌生, 它能极大提升LLM的服务吞吐量，显著降低推理成本。 vLLM 成功的核心秘诀在于一项名为的技术及其配套的 调度策略 ，对于不熟悉底层的人来说，这些概念可能非常晦涩难懂。幸好，DeeoSeek一位研究员开源了一个学习项目，，仅使用约1200行Python代码就实现了 vLLM 的核心功能，相比原生的 vLLM 来说更容易学习其核心思想。得益于轻量化的设计，在Qwen 3 的推理速度上比 vLLM 更快。

阅读详情

相关推荐

换一换

第 3 4章：Scheduler 源码 与 调度策略 改造

Scheduler是 vLLM 性能的灵魂。它决定"每轮step中哪些请求参与计算"&mdash;&mdash;这个决策直接影响吞吐、延迟和公平性。理解Scheduler 源码 不仅是为了修改 调度策略 ，更是为了理解 vLLM 如何处理Prefill/Decode两阶段、KV Cache分配的失败路径和Preemption的触发条件。
本章将逐函数阅读Scheduler 源码 ，理解其核心数据结构（等待队列、运行队列、调度输出）、状态转换逻辑、KV Cache分配的回退策略，并实现一个自定义的优先级 调度策略 。

davidwang456的专栏

65

大模型推理框架 vLLM 源码 解析 （一）

原创 marsggbo。

具身机器人和通用AI

4961

vLLM 推理引擎 源码 解读

vLLM 推理引擎核心技术 解析 vLLM 是UC Berkeley开源的LLM推理引擎，通过PagedAttention和连续批处理技术显著提升推理效率，被Anthropic、StabilityAI等公司广泛采用。其核心创新包括： PagedAttention 将KV缓存分块管理，类似操作系统的分页机制，解决显存碎片化问题。 支持动态块分配与共享（如前缀缓存），显存利用率达95%以上。 连续批处理 动态合并预填充与生成阶段请求，最大化GPU利用率，吞吐量提升2 - 24倍。 架构设计 调度器、块管理器与Worker

几十年资深高级架构师，技术专家的技术笔记。专注C、Java、Python、微服务、云原生、AI工程化及高并发系统分布式架构实战，分享源码剖析、性能调优与架构治理经验。

64

vllm 源码 解析 ( 一 ) ：整体架构与推理代码

vlllm官方代码更新频发,每个版本都有极大变动, 很难说哪个版本好用.第一次阅读 vllm 源码 是0.4.0版本,对这版圈复杂度极高的调度代码印象深刻0.4.1对调度逻辑进行重构,完全大变样, 读代码速度快赶不上迭代的速度了。现在已经更新到0.5.4, 经过长时间观察，发现主要的调度逻辑基本也稳定了下来, 应该可以作为一个固话的版本去阅读。本文解读依据 vllm 0.5.4版本. 没有修改任何代码,大家不必担心夹带私货！打算以五篇文章的篇幅剖析 vllm ，希望能对大家有所帮助。

weixin_42479327的博客

1万+

VLLM 0.7. 3 源码 深度 解析 ：PagedAttention与工业级LLM推理优化

大 语言模型 推理引擎的核心挑战在于高效管理KV缓存与动态批处理。PagedAttention作为一种类操作系统内存分页思想的注意力机制，通过块化KV存储和slot映射实现显存利用率与吞吐量的双重提升。其技术价值体现在降低显存碎片、支持长上下文及高并发请求，广泛应用于API服务、大模型网关和私有化部署等生产场景。 VLLM 作为该机制的标杆开源实现，将PagedAttention从论文算法落地为可调试、可定制的C++/CUDA工业级代码，尤其在v0.7. 3 版本中完整暴露调度层、胶水层与kernel层接口，成为 二 次开

weixin_29046035的博客

269

VLLM 0.7. 3 源码 深度 解析 ：PagedAttention与连续批处理实现原理

大 语言模型 推理引擎的核心挑战在于高效管理KV缓存与动态请求调度。PagedAttention通过内存分页机制将KV缓存解耦为可复用的固定大小页块，结合block table实现逻辑序列到物理显存的灵活映射；连续批处理（Continuous Batching）则在此基础上构建动态调度器，使不同长度请求共享GPU资源池，显著提升吞吐并降低延迟。该技术路径已成为 vLLM 等工业级推理框架的性能基石，广泛应用于高并发API网关、实时生成服务及MoE架构适配场景。本文基于 vLLM - 0.7. 3 源码 ，深入剖析其CUDA内核

weixin_29056701的博客

249

vllm 源码 解析 ( 五 ) ：LLM模型推理

self.model_executor.execute_model，调用与 vllm 耦合后的LLM模型进行推理。这是本篇要讲解内容，我们先来看下模型输入长什么样,execute_model_req：

从调度系统中获得，可以用于做推理的seq_groups, 对seq_groups及可用到的各种属性做了封装，暂时不必管都是什么意思，用到时再现场 分析 。8.1中完成了资源调度工作，接下来该送入初始化好的模型进行推理了。不过 vllm 对具体模型的又做了多层封装：8.1中模型调用指向gpu_executor：

weixin_42479327的博客

8261

C# emgu.cv图像识别 从大图中寻找小图,判断存在图的相似度

这是2个问题，我采用emgu.cv来解决这个识别问题，这里需要在nuget中引用emgu。最近写了一个在一张图片中，截取一部分，通过机器判断截取图片是这张图片的。第一、通过小图去找判断是否存在大图中，第 二 、小图存在大图的什么位置。

一个专注于技术研究创新的程序员

2467

图解大模型计算加速系列： vLLM 源码 解析 2，调度器策略 ( Scheduler )

在本文中，我们会详细解读调度器（Scheduler）全部细节；在下一篇文章中，我们会详细解读块管理（blockmanager）的全部细节，并以parallel sampling，beam search和prefix caching为例，将上图左半部分全部串一遍在后续文章中，我们会来解读上图右半部分细节（还没来得及拆逻辑，暂时不知道会写几篇）

python1234567_的博客

2411

Synares病毒实战：如何从U盘感染中恢复你的Excel和EXE文件（附专杀工具推荐）

本文详细介绍了Synares感染型病毒的典型特征与传播机制，特别是其通过U盘感染Excel和EXE文件的行为。文章提供了一套从应急隔离、诊断到使用专杀工具进行深度清除与文件恢复的完整实战流程，并推荐了有效的安全软件和修复方法，旨在帮助用户最大程度救回重要数据并构建长效安全防护习惯。

weixin_29230649的博客

1829

R语言Dunnett&rsquo;s检验实战：寻找差异组之间的效果

总结起来，本文以R语言为工具，演示了如何使用Dunnett&rsquo;s检验来找出单因素方差 分析 中不同组之间的效果差异。在本篇文章中，我们将通过R语言实战演示如何使用Dunnett&rsquo;s检验来寻找不同组之间的效果差异。在 分析 结果时，通常我们会关注具有显著差异的组别，并基于Dunnett&rsquo;s检验结果进行进一步的解释和讨论。首先，我们需要准备一组数据，假设我们有一个实验，涉及到四个不同的处理组和一个对照组。当我们发现方差 分析 结果显示存在显著差异时，我们可以进一步进行Dunnett&rsquo;s检验来确定具体哪些组之间存在差异。

CoderExtra的博客

1104

图解大模型计算加速系列： vLLM 源码 解析 1，整体架构

图中左侧是用户使用界面，罗列了上述所说的两种调用方式（注意，如前文所说，做demo用的api server官方已经不再维护了，openai_api_server才是官方推荐的使用方式，user custom server目前还没有实现）。右侧则是开发者界面，LLMEngine是 vLLM 的核心逻辑。：该方法将每一个请求包装成 vLLM 能处理的数据类型 ( SequenceGroup，后面我们会详细解释 ) ，并将其加入调度器（Scheduler）的waiting队列中。

python1234567_的博客

2730

Stable Diffusion 模型分享：Indigo Furry mix（人类与野兽的混合）

Indigo Furry mix 是一个混合模型，出的图具有雄性人类的体型和野兽的形态。条目内容类型大模型基础模型SD 1.5来源CIVITAI作者indigowing文件名称文件大小1.99GB。

水滴的博客

1831

深入 解析 vLLM 项目：整体架构与 源码 目录结构

vLLM 项目诞生于对高效LLM推理需求的背景下，旨在通过技术创新提升模型推理速度，降低资源消耗，使得大规模 语言模型 能够在更广泛的场景下得到应用。它不局限于特定模型架构，而是提供了一套灵活的框架，支持多种LLMs的快速部署与优化。 vLLM 的核心在于其优化的内存管理和计算 调度策略 ，这些策略共同作用于模型推理的各个环节，实现了性能的显著提升。 vLLM 项目通过其模块化的设计理念和优化的推理策略，为大型 语言模型 的快速部署与高效推理提供了有力支持。

425

eDP V1.4协议介绍

eDP的全称是Embedded DisplayPort嵌入式显示端口，主要应用与短距离系统内应用，例如手机、一体式台式机等。eDP V1.4b是基于DP V1. 3 标准制作完成，但因应用场景的不同，还是有很多区别。电压摆幅不同，eDP相对较低；eDP功耗相对较低；DP有线材和连接器的要求，eDP没有明确要求；AUX通道电路设计不同（相对DP取消了AC耦合电容和100K&Omega;/1M&Omega;的上下拉电阻）：典型应用场景;

qflook的博客

4303

【WRF工具】WRF 模型输出可视化工具 RIP4

【WRF工具】WRF 模型输出可视化工具 RIP4

WW、forever的博客

1594

vLLM 推理底层结构深解：PagedAttention、Token流式与并发调度 解析

vLLM 是当前大模型推理部署领域最受欢迎的框架之一，
它在不改变模型结构的前提下，显著提升了推理吞吐与并发能力，
支持**token 流式输出（Streaming）、长上下文支持、OpenAI API 接口无缝适配**等一系列强特性。
本篇将以系统级视角，深入 解析 ** vLLM 的关键优化模块&mdash;&mdash;PagedAttention** 的设计原理、KV 缓存机制、Token Scheduling 算法与高并发 调度策略

努力分享一些人工智能、计算机视觉、影像等相关的知识干货！

1512

DICOM图像知识：深入详解DICOM的层级关系

解析 DICOM的四个主要层级：Patient（患者）、Study（检查）、Series（序列）和SOP（服务 - 对象对），探讨它们之间的关系、各自的唯一标识符以及在实际应用中的用途

编程技术探索者，分享C/C++、C#、Java、数据库等开发经验，聚焦实战技巧与AI兴趣，助力编程爱好者成长。

3305

vLLM ( 2 ) - 架构总览

上一篇通过Qwen2的 vllm 推理和部署，对 vllm 有了简单直观的了解，但是我们尚未涉及到它的工作原理。接下来我们将以 vllm 源码 为基础，逐步 解析 。本篇主要是对 vllm 架构的总览。本篇通过原理简述、架构图和项目结构展示这几个部分对 vllm 原理和架构做了 解析 ，相信大家对 vllm 有了更深入的认识，也为后续 源码 分析 清楚了一些障碍。

daihaoguang的博客

6828

visio安装包

Microsoft Office Visio 是一款专业的制图软件。利用经过更新的形状、协作工具和数据链接图表，创建专业级图表以简化复杂的信息。

深度学习 工具包 DeepLearningToolbox

深度学习 工具包Deprecation notice. - - - - - This toolbox is outdated and no longer maintained.There are much better tools available for deep learning than this toolbox, e.g. [Theano] ( http : //deeplearning.net/software/theano/ ), [torch] ( http : //torch.ch/ ) or [tensorflow] ( http : //www.tensorflow.org/ ) I would suggest you use one of the tools mentioned above rather than use this toolbox.Best, Rasmus.DeepLearnToolbox================A Matlab toolbox for Deep Learning.Deep Learning is a new subfield of machine learning that focuses on learning deep hierarchical models of data.It is inspired by the human brain&# 3 9;s apparent deep ( layered, hierarchical ) architecture.A good overview of the theory of Deep Learning theory is[Learning Deep Architectures for AI] ( http : //www.iro.umontreal.ca/~bengioy/papers/ftml_book.pdf ) For a more informal introduction, see the following videos by Geoffrey Hinton and Andrew Ng.* [The Next Generation of Neural Networks] ( http : //www.youtube.com/watch?v=AyzOUbkUf 3 M ) ( Hinton, 2007 ) * [Recent Developments in Deep Learning] ( http : //www.youtube.com/watch?v=VdIURAu1 - aU ) ( Hinton, 2010 ) * [Unsupervised Feature Learning and Deep Learning] ( http : //www.youtube.com/watch?v=ZmNOAtZIgIk ) ( Ng, 2011 ) If you use this toolbox in your research please cite [Prediction as a candidate for learning deep hierarchical models of data] ( http : //www2.imm.dtu.dk/pubdb/views/publication_details.php?id=6284 ) ```@MASTERSTHESIS\{IMM2012 - 06284, author = "R. B. Palm", title = "Prediction as a candidate for learning deep hierarchical models of data", year = "2012",}```Contact : rasmusbergpalm at gmail dot comDirectories included in the toolbox - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - `NN/` - A library for Feedforward Backpropagation Neural Networks`CNN/` - A library for Convolutional Neural Networks`DBN/` - A library for Deep Belief Networks`SAE/` - A library for Stacked Auto - Encoders`CAE/` - A library for Convolutional Auto - Encoders`util/` - Utility functions used by the libraries`data/` - Data used by the examples`tests/` - unit tests to verify toolbox is workingFor references on each library check REFS.mdSetup - - - - - 1. Download.2. addpath ( genpath ( &# 3 9;DeepLearnToolbox&# 3 9; ) );Example : Deep Belief Network - - - - - - - - - - - - - - - - - - - - - ```matlabfunction test_example_DBNload mnist_uint8;train_x = double ( train_x ) / 255;test_x = double ( test_x ) / 255;train_y = double ( train_y );test_y = double ( test_y );%% ex1 train a 100 hidden unit RBM and visualize its weightsrand ( &# 3 9;state&# 3 9;,0 ) dbn.sizes = [100];opts.numepochs = 1;opts.batchsize = 100;opts.momentum = 0;opts.alpha = 1;dbn = dbnsetup ( dbn, train_x, opts );dbn = dbntrain ( dbn, train_x, opts );figure; visualize ( dbn.rbm{1}.W&# 3 9; ); % Visualize the RBM weights%% ex2 train a 100 - 100 hidden unit DBN and use its weights to initialize a NNrand ( &# 3 9;state&# 3 9;,0 ) %train dbndbn.sizes = [100 100];opts.numepochs = 1;opts.batchsize = 100;opts.momentum = 0;opts.alpha = 1;dbn = dbnsetup ( dbn, train_x, opts );dbn = dbntrain ( dbn, train_x, opts );%unfold dbn to nnnn = dbnunfoldtonn ( dbn, 10 );nn.activation_function = &# 3 9;sigm&# 3 9;;%train nnopts.numepochs = 1;opts.batchsize = 100;nn = nntrain ( nn, train_x, train_y, opts );[er, bad] = nntest ( nn, test_x, test_y );assert ( er < 0.10, &# 3 9;Too big error&# 3 9; );```Example : Stacked Auto - Encoders - - - - - - - - - - - - - - - - - - - - - ```matlabfunction test_example_SAEload mnist_uint8;train_x = double ( train_x ) /255;test_x = double ( test_x ) /255;train_y = double ( train_y );test_y = double ( test_y );%% ex1 train a 100 hidden unit SDAE and use it to initialize a FFNN% Setup and train a stacked denoising autoencoder ( SDAE ) rand ( &# 3 9;state&# 3 9;,0 ) sae = saesetup ( [784 100] );sae.ae{1}.activation_function = &# 3 9;sigm&# 3 9;;sae.ae{1}.learningRate = 1;sae.ae{1}.inputZeroMaskedFraction = 0.5;opts.numepochs = 1;opts.batchsize = 100;sae = saetrain ( sae, train_x, opts );visualize ( sae.ae{1}.W{1} ( :,2 : end ) &# 3 9; ) % Use the SDAE to initialize a FFNNnn = nnsetup ( [784 100 10] );nn.activation_function = &# 3 9;sigm&# 3 9;;nn.learningRate = 1;nn.W{1} = sae.ae{1}.W{1};% Train the FFNNopts.numepochs = 1;opts.batchsize = 100;nn = nntrain ( nn, train_x, train_y, opts );[er, bad] = nntest ( nn, test_x, test_y );assert ( er < 0.16, &# 3 9;Too big error&# 3 9; );```Example : Convolutional Neural Nets - - - - - - - - - - - - - - - - - - - - - ```matlabfunction test_example_CNNload mnist_uint8;train_x = double ( reshape ( train_x&# 3 9;,28,28,60000 ) ) /255;test_x = double ( reshape ( test_x&# 3 9;,28,28,10000 ) ) /255;train_y = double ( train_y&# 3 9; );test_y = double ( test_y&# 3 9; );%% ex1 Train a 6c - 2s - 12c - 2s Convolutional neural network %will run 1 epoch in about 200 second and get around 11% error. %With 100 epochs you&# 3 9;ll get around 1.2% errorrand ( &# 3 9;state&# 3 9;,0 ) cnn.layers = { struct ( &# 3 9;type&# 3 9;, &# 3 9;i&# 3 9; ) %input layer struct ( &# 3 9;type&# 3 9;, &# 3 9;c&# 3 9;, &# 3 9;outputmaps&# 3 9;, 6, &# 3 9;kernelsize&# 3 9;, 5 ) %convolution layer struct ( &# 3 9;type&# 3 9;, &# 3 9;s&# 3 9;, &# 3 9;scale&# 3 9;, 2 ) %sub sampling layer struct ( &# 3 9;type&# 3 9;, &# 3 9;c&# 3 9;, &# 3 9;outputmaps&# 3 9;, 12, &# 3 9;kernelsize&# 3 9;, 5 ) %convolution layer struct ( &# 3 9;type&# 3 9;, &# 3 9;s&# 3 9;, &# 3 9;scale&# 3 9;, 2 ) %subsampling layer};cnn = cnnsetup ( cnn, train_x, train_y );opts.alpha = 1;opts.batchsize = 50;opts.numepochs = 1;cnn = cnntrain ( cnn, train_x, train_y, opts );[er, bad] = cnntest ( cnn, test_x, test_y );%plot mean squared errorfigure; plot ( cnn.rL );assert ( er<0.12, &# 3 9;Too big error&# 3 9; );```Example : Neural Networks - - - - - - - - - - - - - - - - - - - - - ```matlabfunction test_example_NNload mnist_uint8;train_x = double ( train_x ) / 255;test_x = double ( test_x ) / 255;train_y = double ( train_y );test_y = double ( test_y );% normalize[train_x, mu, sigma] = zscore ( train_x );test_x = normalize ( test_x, mu, sigma );%% ex1 vanilla neural netrand ( &# 3 9;state&# 3 9;,0 ) nn = nnsetup ( [784 100 10] );opts.numepochs = 1; % Number of full sweeps through dataopts.batchsize = 100; % Take a mean gradient step over this many samples[nn, L] = nntrain ( nn, train_x, train_y, opts );[er, bad] = nntest ( nn, test_x, test_y );assert ( er < 0.08, &# 3 9;Too big error&# 3 9; );%% ex2 neural net with L2 weight decayrand ( &# 3 9;state&# 3 9;,0 ) nn = nnsetup ( [784 100 10] );nn.weightPenaltyL2 = 1e - 4; % L2 weight decayopts.numepochs = 1; % Number of full sweeps through dataopts.batchsize = 100; % Take a mean gradient step over this many samplesnn = nntrain ( nn, train_x, train_y, opts );[er, bad] = nntest ( nn, test_x, test_y );assert ( er < 0.1, &# 3 9;Too big error&# 3 9; );%% ex 3 neural net with dropoutrand ( &# 3 9;state&# 3 9;,0 ) nn = nnsetup ( [784 100 10] );nn.dropoutFraction = 0.5; % Dropout fraction opts.numepochs = 1; % Number of full sweeps through dataopts.batchsize = 100; % Take a mean gradient step over this many samplesnn = nntrain ( nn, train_x, train_y, opts );[er, bad] = nntest ( nn, test_x, test_y );assert ( er < 0.1, &# 3 9;Too big error&# 3 9; );%% ex4 neural net with sigmoid activation functionrand ( &# 3 9;state&# 3 9;,0 ) nn = nnsetup ( [784 100 10] );nn.activation_function = &# 3 9;sigm&# 3 9;; % Sigmoid activation functionnn.learningRate = 1; % Sigm require a lower learning rateopts.numepochs = 1; % Number of full sweeps through dataopts.batchsize = 100; % Take a mean gradient step over this many samplesnn = nntrain ( nn, train_x, train_y, opts );[er, bad] = nntest ( nn, test_x, test_y );assert ( er < 0.1, &# 3 9;Too big error&# 3 9; );%% ex5 plotting functionalityrand ( &# 3 9;state&# 3 9;,0 ) nn = nnsetup ( [784 20 10] );opts.numepochs = 5; % Number of full sweeps through datann.output = &# 3 9;softmax&# 3 9;; % use softmax outputopts.batchsize = 1000; % Take a mean gradient step over this many samplesopts.plot = 1; % enable plottingnn = nntrain ( nn, train_x, train_y, opts );[er, bad] = nntest ( nn, test_x, test_y );assert ( er < 0.1, &# 3 9;Too big error&# 3 9; );%% ex6 neural net with sigmoid activation and plotting of validation and training error% split training data into training and validation datavx = train_x ( 1 : 10000, : );tx = train_x ( 10001 : end, : );vy = train_y ( 1 : 10000, : );ty = train_y ( 10001 : end, : );rand ( &# 3 9;state&# 3 9;,0 ) nn = nnsetup ( [784 20 10] ); nn.output = &# 3 9;softmax&# 3 9;; % use softmax outputopts.numepochs = 5; % Number of full sweeps through dataopts.batchsize = 1000; % Take a mean gradient step over this many samplesopts.plot = 1; % enable plottingnn = nntrain ( nn, tx, ty, opts, vx, vy ); % nntrain takes validation set as last two arguments ( optionally ) [er, bad] = nntest ( nn, test_x, test_y );assert ( er < 0.1, &# 3 9;Too big error&# 3 9; );```[![Bitdeli Badge] ( https : //d2weczhvl82 3 v0.cloudfront.net/rasmusbergpalm/deeplearntoolbox/trend.png ) ] ( https : //bitdeli.com/free "Bitdeli Badge" )

vLLM 异步推理实战：从AsyncLLMEngine到流式输出与 源码 解析

异步编程是提升高并发服务吞吐的关键技术，其核心在于通过事件循环和协程实现等待期间的资源复用。大模型推理场景中， vLLM 的连续批处理与 PagedAttention 机制本质上需要一个高频运转的调度循环，这决定了它的服务化形态必然走向异步架构。理解 async/await 和 Task 的协作调度，才能掌握 AsyncLLMEngine 的设计精髓。在实际生产中，基于 AsyncLLMEngine 配合 FastAPI 与 SSE 协议，可以轻松实现打字机式的流式输出，同时支撑多路并发请求。本文从协程原理切

weixin_29042035的博客

297

pscad IEEE标准系统搭建说明

pscad IEEE标准系统搭建说明，包含典型的IEEE 9节点、14节点、 3 0节点、 3 9节点和118节点系统。内含标准测试系统的原始数据和潮流结果。

深入 vLLM ：高性能大模型推理框架 解析

在大模型应用中，推理性能往往成为限制系统规模和用户体验的关键因素。为此， vLLM 应运而生，提供了高吞吐量、低延迟的推理引擎，并支持多模型协作和异构硬件调度。 vLLM 不仅可以独立作为推理服务，还能与 LangChain 等工程框架无缝集成，实现完整的业务流程编排。

和大家一起涨芝士

1136

vLLM - Omni V2前瞻：多模态推理引擎的KV Cache与 调度策略 解析

多模态推理引擎正在从&ldquo;可用&rdquo;走向&ldquo;好用&rdquo;，其核心挑战在于视觉、语音等非文本Token如何高效融入现有的批处理与显存管理体系。传统文本推理框架以文本Token为抽象，而图像Patch、音频特征帧等视觉Token具有数量大、一次性输入、长期占用的特点，直接沿用PagedAttention会导致显存浪费和并发吞吐下降。围绕KV Cache管理、Chunked Prefill状态恢复、动态形状批处理等关键技术，引擎需要在 调度策略 上做出针对性优化，才能支撑高分辨率图片、多图对话和流式语音交互等真实场景。 vLLM - Om

weixin_32597009的博客

262

vLLM 多实例部署 + Nginx 高并发流量控制架构设计实战

本文将围绕企业级落地场景，系统设计并实战构建一套 **&ldquo; vLLM 多实例部署 + Nginx 网关调度 + 流量控制&rdquo;** 架构方案，解决：
>
> - ✅ 多实例部署架构：GPU 负载均衡 &times; 进程隔离 &times; 高可用能力
> - ✅ Nginx 网关限流：流式响应稳定性 &times; API 扩展性 &times; 并发保护
> - ✅ 请求 调度策略 ：Round - Robin / Sticky / 权重分配 / 动态容量追踪
> - ✅ 系统优化方案：Timeout 设计、健康探针、GPU 利用率追踪

努力分享一些人工智能、计算机视觉、影像等相关的知识干货！

2587

解析 nano - vllm

prefill和deocode的特点prefill是计算密集decode是访存密集KV cache的必要性KV cache避免重复的计算提高batch可以降低延迟吗？不一定，batch提高的是吞吐量，但是不一定降低单个用户的延迟。

2401_87151064的博客

183

上一篇：

vllm源码解析(一)：整体架构与推理代码

下一篇：

vllm源码解析(三)：块管理(BlockManager)

第 3 4章：Scheduler 源码 与 调度策略 改造

davidwang456的专栏

06-19

65

Scheduler是 vLLM 性能的灵魂。它决定"每轮step中哪些请求参与计算"&mdash;&mdash;这个决策直接影响吞吐、延迟和公平性。理解Scheduler 源码 不仅是为了修改 调度策略 ，更是为了理解 vLLM 如何处理Prefill/Decode两阶段、KV Cache分配的失败路径和Preemption的触发条件。
本章将逐函数阅读Scheduler 源码 ，理解其核心数据结构（等待队列、运行队列、调度输出）、状态转换逻辑、KV Cache分配的回退策略，并实现一个自定义的优先级 调度策略 。

大模型推理框架 vLLM 源码 解析 （一）

具身机器人和通用AI

02-05

4961

原创 marsggbo。

vLLM 推理引擎 源码 解读

几十年资深高级架构师，技术专家的技术笔记。专注C、Java、Python、微服务、云原生、AI工程化及高并发系统分布式架构实战，分享源码剖析、性能调优与架构治理经验。

06-15

64

vLLM 推理引擎核心技术 解析 vLLM 是UC Berkeley开源的LLM推理引擎，通过PagedAttention和连续批处理技术显著提升推理效率，被Anthropic、StabilityAI等公司广泛采用。其核心创新包括： PagedAttention 将KV缓存分块管理，类似操作系统的分页机制，解决显存碎片化问题。 支持动态块分配与共享（如前缀缓存），显存利用率达95%以上。 连续批处理 动态合并预填充与生成阶段请求，最大化GPU利用率，吞吐量提升2 - 24倍。 架构设计 调度器、块管理器与Worker

vllm 源码 解析 ( 一 ) ：整体架构与推理代码

热门推荐

weixin_42479327的博客

09-03

1万+

vlllm官方代码更新频发,每个版本都有极大变动, 很难说哪个版本好用.第一次阅读 vllm 源码 是0.4.0版本,对这版圈复杂度极高的调度代码印象深刻0.4.1对调度逻辑进行重构,完全大变样, 读代码速度快赶不上迭代的速度了。现在已经更新到0.5.4, 经过长时间观察，发现主要的调度逻辑基本也稳定了下来, 应该可以作为一个固话的版本去阅读。本文解读依据 vllm 0.5.4版本. 没有修改任何代码,大家不必担心夹带私货！打算以五篇文章的篇幅剖析 vllm ，希望能对大家有所帮助。

VLLM 0.7. 3 源码 深度 解析 ：PagedAttention与工业级LLM推理优化

weixin_29046035的博客

10-17

269

大 语言模型 推理引擎的核心挑战在于高效管理KV缓存与动态批处理。PagedAttention作为一种类操作系统内存分页思想的注意力机制，通过块化KV存储和slot映射实现显存利用率与吞吐量的双重提升。其技术价值体现在降低显存碎片、支持长上下文及高并发请求，广泛应用于API服务、大模型网关和私有化部署等生产场景。 VLLM 作为该机制的标杆开源实现，将PagedAttention从论文算法落地为可调试、可定制的C++/CUDA工业级代码，尤其在v0.7. 3 版本中完整暴露调度层、胶水层与kernel层接口，成为 二 次开

VLLM 0.7. 3 源码 深度 解析 ：PagedAttention与连续批处理实现原理

weixin_29056701的博客

10-17

249

大 语言模型 推理引擎的核心挑战在于高效管理KV缓存与动态请求调度。PagedAttention通过内存分页机制将KV缓存解耦为可复用的固定大小页块，结合block table实现逻辑序列到物理显存的灵活映射；连续批处理（Continuous Batching）则在此基础上构建动态调度器，使不同长度请求共享GPU资源池，显著提升吞吐并降低延迟。该技术路径已成为 vLLM 等工业级推理框架的性能基石，广泛应用于高并发API网关、实时生成服务及MoE架构适配场景。本文基于 vLLM - 0.7. 3 源码 ，深入剖析其CUDA内核

vllm 源码 解析 ( 五 ) ：LLM模型推理

weixin_42479327的博客

09-08

8261

self.model_executor.execute_model，调用与 vllm 耦合后的LLM模型进行推理。这是本篇要讲解内容，我们先来看下模型输入长什么样,execute_model_req：

从调度系统中获得，可以用于做推理的seq_groups, 对seq_groups及可用到的各种属性做了封装，暂时不必管都是什么意思，用到时再现场 分析 。8.1中完成了资源调度工作，接下来该送入初始化好的模型进行推理了。不过 vllm 对具体模型的又做了多层封装：8.1中模型调用指向gpu_executor：

C# emgu.cv图像识别 从大图中寻找小图,判断存在图的相似度

一个专注于技术研究创新的程序员

02-09

2467

这是2个问题，我采用emgu.cv来解决这个识别问题，这里需要在nuget中引用emgu。最近写了一个在一张图片中，截取一部分，通过机器判断截取图片是这张图片的。第一、通过小图去找判断是否存在大图中，第 二 、小图存在大图的什么位置。

图解大模型计算加速系列： vLLM 源码 解析 2，调度器策略 ( Scheduler )

python1234567_的博客

10-03

2411

在本文中，我们会详细解读调度器（Scheduler）全部细节；在下一篇文章中，我们会详细解读块管理（blockmanager）的全部细节，并以parallel sampling，beam search和prefix caching为例，将上图左半部分全部串一遍在后续文章中，我们会来解读上图右半部分细节（还没来得及拆逻辑，暂时不知道会写几篇）

Synares病毒实战：如何从U盘感染中恢复你的Excel和EXE文件（附专杀工具推荐）

weixin_29230649的博客

02-23

1829

本文详细介绍了Synares感染型病毒的典型特征与传播机制，特别是其通过U盘感染Excel和EXE文件的行为。文章提供了一套从应急隔离、诊断到使用专杀工具进行深度清除与文件恢复的完整实战流程，并推荐了有效的安全软件和修复方法，旨在帮助用户最大程度救回重要数据并构建长效安全防护习惯。

R语言Dunnett&rsquo;s检验实战：寻找差异组之间的效果

CoderExtra的博客

08-17

1104

总结起来，本文以R语言为工具，演示了如何使用Dunnett&rsquo;s检验来找出单因素方差 分析 中不同组之间的效果差异。在本篇文章中，我们将通过R语言实战演示如何使用Dunnett&rsquo;s检验来寻找不同组之间的效果差异。在 分析 结果时，通常我们会关注具有显著差异的组别，并基于Dunnett&rsquo;s检验结果进行进一步的解释和讨论。首先，我们需要准备一组数据，假设我们有一个实验，涉及到四个不同的处理组和一个对照组。当我们发现方差 分析 结果显示存在显著差异时，我们可以进一步进行Dunnett&rsquo;s检验来确定具体哪些组之间存在差异。

图解大模型计算加速系列： vLLM 源码 解析 1，整体架构

python1234567_的博客

10-02

2730

图中左侧是用户使用界面，罗列了上述所说的两种调用方式（注意，如前文所说，做demo用的api server官方已经不再维护了，openai_api_server才是官方推荐的使用方式，user custom server目前还没有实现）。右侧则是开发者界面，LLMEngine是 vLLM 的核心逻辑。：该方法将每一个请求包装成 vLLM 能处理的数据类型 ( SequenceGroup，后面我们会详细解释 ) ，并将其加入调度器（Scheduler）的waiting队列中。

Stable Diffusion 模型分享：Indigo Furry mix（人类与野兽的混合）

水滴的博客

02-19

1831

Indigo Furry mix 是一个混合模型，出的图具有雄性人类的体型和野兽的形态。条目内容类型大模型基础模型SD 1.5来源CIVITAI作者indigowing文件名称文件大小1.99GB。

深入 解析 vLLM 项目：整体架构与 源码 目录结构

02-25

425

vLLM 项目诞生于对高效LLM推理需求的背景下，旨在通过技术创新提升模型推理速度，降低资源消耗，使得大规模 语言模型 能够在更广泛的场景下得到应用。它不局限于特定模型架构，而是提供了一套灵活的框架，支持多种LLMs的快速部署与优化。 vLLM 的核心在于其优化的内存管理和计算 调���策略 ，这些策略共同作用于模型推理的各个环节，实现了性能的显著提升。 vLLM 项目通过其模块化的设计理念和优化的推理策略，为大型 语言模型 的快速部署与高效推理提供了有力支持。

eDP V1.4协议介绍

qflook的博客

05-30

4303

eDP的全称是Embedded DisplayPort嵌入式显示端口，主要应用与短距离系统内应用，例如手机、一体式台式机等。eDP V1.4b是基于DP V1. 3 标准制作完成，但因应用场景的不同，还是有很多区别。电压摆幅不同，eDP相对较低；eDP功耗相对较低；DP有线材和连接器的要求，eDP没有明确要求；AUX通道电路设计不同（相对DP取消了AC耦合电容和100K&Omega;/1M&Omega;的上下拉电阻）：典型应用场景;

【WRF工具】WRF 模型输出可视化工具 RIP4

WW、forever的博客

11-11

1594

【WRF工具】WRF 模型输出可视化工具 RIP4

vLLM 推理底层结构深解：PagedAttention、Token流式与并发调度 解析

努力分享一些人工智能、计算机视觉、影像等相关的知识干货！

04-14

1512

vLLM 是当前大模型推理部署领域最受欢迎的框架之一，
它在不改变模型结构的前提下，显著提升了推理吞吐与并发能力，
支持**token 流式输出（Streaming）、长上下文支持、OpenAI API 接口无缝适配**等一系列强特性。
本篇将以系统级视角，深入 解析 ** vLLM 的关键优化模块&mdash;&mdash;PagedAttention** 的设计原理、KV 缓存机制、Token Scheduling 算法与高并发 调度策略

DICOM图像知识：深入详解DICOM的层级关系

编程技术探索者，分享C/C++、C#、Java、数据库等开发经验，聚焦实战技巧与AI兴趣，助力编程爱好者成长。

11-09

3305

解析 DICOM的四个主要层级：Patient（患者）、Study（检查）、Series（序列）和SOP（服务 - 对象对），探讨它们之间的关系、各自的唯一标识符以及在实际应用中的用途

vLLM ( 2 ) - 架构总览

daihaoguang的博客

08-17

6828

上一篇通过Qwen2的 vllm 推理和部署，对 vllm 有了简单直观的了解，但是我们尚未涉及到它的工作原理。接下来我们将以 vllm 源码 为基础，逐步 解析 。本篇主要是对 vllm 架构的总览。本篇通过原理简述、架构图和项目结构展示这几个部分对 vllm 原理和架构做了 解析 ，相信大家对 vllm 有了更深入的认识，也为后续 源码 分析 清楚了一些障碍。

visio安装包

Microsoft Office Visio 是一款专业的制图软件。利用经过更新的形状、协作工具和数据链接图表，创建专业级图表以简化复杂的信息。

深度学习 工具包 DeepLearningToolbox

深度学习 工具包Deprecation notice. - - - - - This toolbox is outdated and no longer maintained.There are much better tools available for deep learning than this toolbox, e.g. [Theano] ( http : //deeplearning.net/software/theano/ ), [torch] ( http : //torch.ch/ ) or [tensorflow] ( http : //www.tensorflow.org/ ) I would suggest you use one of the tools mentioned above rather than use this toolbox.Best, Rasmus.DeepLearnToolbox================A Matlab toolbox for Deep Learning.Deep Learning is a new subfield of machine learning that focuses on learning deep hierarchical models of data.It is inspired by the human brain&# 3 9;s apparent deep ( layered, hierarchical ) architecture.A good overview of the theory of Deep Learning theory is[Learning Deep Architectures for AI] ( http : //www.iro.umontreal.ca/~bengioy/papers/ftml_book.pdf ) For a more informal introduction, see the following videos by Geoffrey Hinton and Andrew Ng.* [The Next Generation of Neural Networks] ( http : //www.youtube.com/watch?v=AyzOUbkUf 3 M ) ( Hinton, 2007 ) * [Recent Developments in Deep Learning] ( http : //www.youtube.com/watch?v=VdIURAu1 - aU ) ( Hinton, 2010 ) * [Unsupervised Feature Learning and Deep Learning] ( http : //www.youtube.com/watch?v=ZmNOAtZIgIk ) ( Ng, 2011 ) If you use this toolbox in your research please cite [Prediction as a candidate for learning deep hierarchical models of data] ( http : //www2.imm.dtu.dk/pubdb/views/publication_details.php?id=6284 ) ```@MASTERSTHESIS\{IMM2012 - 06284, author = "R. B. Palm", title = "Prediction as a candidate for learning deep hierarchical models of data", year = "2012",}```Contact : rasmusbergpalm at gmail dot comDirectories included in the toolbox - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - `NN/` - A library for Feedforward Backpropagation Neural Networks`CNN/` - A library for Convolutional Neural Networks`DBN/` - A library for Deep Belief Networks`SAE/` - A library for Stacked Auto - Encoders`CAE/` - A library for Convolutional Auto - Encoders`util/` - Utility functions used by the libraries`data/` - Data used by the examples`tests/` - unit tests to verify toolbox is workingFor references on each library check REFS.mdSetup - - - - - 1. Download.2. addpath ( genpath ( &# 3 9;DeepLearnToolbox&# 3 9; ) );Example : Deep Belief Network - - - - - - - - - - - - - - - - - - - - - ```matlabfunction test_example_DBNload mnist_uint8;train_x = double ( train_x ) / 255;test_x = double ( test_x ) / 255;train_y = double ( train_y );test_y = double ( test_y );%% ex1 train a 100 hidden unit RBM and visualize its weightsrand ( &# 3 9;state&# 3 9;,0 ) dbn.sizes = [100];opts.numepochs = 1;opts.batchsize = 100;opts.momentum = 0;opts.alpha = 1;dbn = dbnsetup ( dbn, train_x, opts );dbn = dbntrain ( dbn, train_x, opts );figure; visualize ( dbn.rbm{1}.W&# 3 9; ); % Visualize the RBM weights%% ex2 train a 100 - 100 hidden unit DBN and use its weights to initialize a NNrand ( &# 3 9;state&# 3 9;,0 ) %train dbndbn.sizes = [100 100];opts.numepochs = 1;opts.batchsize = 100;opts.momentum = 0;opts.alpha = 1;dbn = dbnsetup ( dbn, train_x, opts );dbn = dbntrain ( dbn, train_x, opts );%unfold dbn to nnnn = dbnunfoldtonn ( dbn, 10 );nn.activation_function = &# 3 9;sigm&# 3 9;;%train nnopts.numepochs = 1;opts.batchsize = 100;nn = nntrain ( nn, train_x, train_y, opts );[er, bad] = nntest ( nn, test_x, test_y );assert ( er < 0.10, &# 3 9;Too big error&# 3 9; );```Example : Stacked Auto - Encoders - - - - - - - - - - - - - - - - - - - - - ```matlabfunction test_example_SAEload mnist_uint8;train_x = double ( train_x ) /255;test_x = double ( test_x ) /255;train_y = double ( train_y );test_y = double ( test_y );%% ex1 train a 100 hidden unit SDAE and use it to initialize a FFNN% Setup and train a stacked denoising autoencoder ( SDAE ) rand ( &# 3 9;state&# 3 9;,0 ) sae = saesetup ( [784 100] );sae.ae{1}.activation_function = &# 3 9;sigm&# 3 9;;sae.ae{1}.learningRate = 1;sae.ae{1}.inputZeroMaskedFraction = 0.5;opts.numepochs = 1;opts.batchsize = 100;sae = saetrain ( sae, train_x, opts );visualize ( sae.ae{1}.W{1} ( :,2 : end ) &# 3 9; ) % Use the SDAE to initialize a FFNNnn = nnsetup ( [784 100 10] );nn.activation_function = &# 3 9;sigm&# 3 9;;nn.learningRate = 1;nn.W{1} = sae.ae{1}.W{1};% Train the FFNNopts.numepochs = 1;opts.batchsize = 100;nn = nntrain ( nn, train_x, train_y, opts );[er, bad] = nntest ( nn, test_x, test_y );assert ( er < 0.16, &# 3 9;Too big error&# 3 9; );```Example : Convolutional Neural Nets - - - - - - - - - - - - - - - - - - - - - ```matlabfunction test_example_CNNload mnist_uint8;train_x = double ( reshape ( train_x&# 3 9;,28,28,60000 ) ) /255;test_x = double ( reshape ( test_x&# 3 9;,28,28,10000 ) ) /255;train_y = double ( train_y&# 3 9; );test_y = double ( test_y&# 3 9; );%% ex1 Train a 6c - 2s - 12c - 2s Convolutional neural network %will run 1 epoch in about 200 second and get around 11% error. %With 100 epochs you&# 3 9;ll get around 1.2% errorrand ( &# 3 9;state&# 3 9;,0 ) cnn.layers = { struct ( &# 3 9;type&# 3 9;, &# 3 9;i&# 3 9; ) %input layer struct ( &# 3 9;type&# 3 9;, &# 3 9;c&# 3 9;, &# 3 9;outputmaps&# 3 9;, 6, &# 3 9;kernelsize&# 3 9;, 5 ) %convolution layer struct ( &# 3 9;type&# 3 9;, &# 3 9;s&# 3 9;, &# 3 9;scale&# 3 9;, 2 ) %sub sampling layer struct ( &# 3 9;type&# 3 9;, &# 3 9;c&# 3 9;, &# 3 9;outputmaps&# 3 9;, 12, &# 3 9;kernelsize&# 3 9;, 5 ) %convolution layer struct ( &# 3 9;type&# 3 9;, &# 3 9;s&# 3 9;, &# 3 9;scale&# 3 9;, 2 ) %subsampling layer};cnn = cnnsetup ( cnn, train_x, train_y );opts.alpha = 1;opts.batchsize = 50;opts.numepochs = 1;cnn = cnntrain ( cnn, train_x, train_y, opts );[er, bad] = cnntest ( cnn, test_x, test_y );%plot mean squared errorfigure; plot ( cnn.rL );assert ( er<0.12, &# 3 9;Too big error&# 3 9; );```Example : Neural Networks - - - - - - - - - - - - - - - - - - - - - ```matlabfunction test_example_NNload mnist_uint8;train_x = double ( train_x ) / 255;test_x = double ( test_x ) / 255;train_y = double ( train_y );test_y = double ( test_y );% normalize[train_x, mu, sigma] = zscore ( train_x );test_x = normalize ( test_x, mu, sigma );%% ex1 vanilla neural netrand ( &# 3 9;state&# 3 9;,0 ) nn = nnsetup ( [784 100 10] );opts.numepochs = 1; % Number of full sweeps through dataopts.batchsize = 100; % Take a mean gradient step over this many samples[nn, L] = nntrain ( nn, train_x, train_y, opts );[er, bad] = nntest ( nn, test_x, test_y );assert ( er < 0.08, &# 3 9;Too big error&# 3 9; );%% ex2 neural net with L2 weight decayrand ( &# 3 9;state&# 3 9;,0 ) nn = nnsetup ( [784 100 10] );
