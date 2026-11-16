# 10 性能建模与 Profiling：怎么算、怎么看

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　3,136 字符　｜　20 个片段　｜　更新于 2026-11-13
## 一、先把指标定义清楚
| 指标 | 定义 | 谁在意 |
|---|---|---|
| TTFT | 从发出请求到收到第一个 token | 交互体验 |
| ITL / TPOT | 相邻 token 的间隔 / 平均每 token 时间 | 流式阅读体验 |
| 吞吐 | req/s 或 output tokens/s | 成本 |
| Goodput | 满足 SLO 前提下的吞吐 | 生产决策的唯一有效指标 |
| GPU 利用率 | SM 活跃比例 | 参考，不是目标 |
| KV 使用率 | 已分配 block / 总 block | **vLLM 真正的瓶颈信号** |

只报「吞吐」而不报 SLO，几乎所有压测结论都不可用——把 batch 拉到极限总能刷高吞吐，代价是 ITL 不可接受。

## 二、手算性能上限（比压测快得多）
### decode 的理论上限
decode 每步要把权重读一遍：

    单步时间下限 ≈ (权重字节 + batch × KV 字节) / HBM 带宽
    单请求 tok/s 上限 ≈ 1 / 单步时间

例：7B FP16（约 14GB）在 H100（3.35TB/s）上：
    14GB / 3.35TB/s ≈ 4.2 ms → 约 240 tok/s（batch=1）

batch 增大时，权重读取被摊薄，吞吐近似线性上升，直到算力成为瓶颈：

    总吞吐 ≈ batch / 单步时间，而 单步时间 ≈ (W + B·KV) / BW

所以「decode 吞吐 ≈ 带宽 / 每 token 需搬的字节」是个非常好用的心算公式。量化把 W 变小，直接体现在这个式子里。

### prefill 的理论上限
    FLOPs ≈ 2 × 参数量 × token 数（矩阵乘部分）+ 注意力部分
    时间下限 ≈ FLOPs / 有效算力

例：7B 模型处理 1000 token：2 × 7e9 × 1000 = 1.4e13 FLOPs；若有效算力 400 TFLOPS → 约 35ms。
注意实际有效算力常是峰值的 30%–60%，小 batch 时更低。

### 排队论
    并发请求数 = 吞吐（req/s） × 平均时延（s）

这条 Little 定律用来校验：如果你测到 20 req/s、平均时延 5s，系统里就应该有约 100 个在途请求。对不上说明测量口径有问题（比如时延只算了生成阶段）。

## 三、Roofline 快速判断
    算术强度 = FLOPs / 搬运字节
    GPU 拐点 = 峰值算力 / 峰值带宽

- A100 80GB：312 TFLOPS FP16 / 2.0 TB/s → 拐点约 156 FLOP/B
- H100 SXM：约 989 TFLOPS FP16 / 3.35 TB/s → 拐点约 295 FLOP/B

同一个 attention kernel，prefill 时算术强度高（compute-bound），decode 时极低（memory-bound）——所以「为什么 decode 加 batch 有用、prefill 加 batch 用处不大」可以直接从这张图看出来。

## 四、工具链
| 工具 | 看什么 | 用法要点 |
|---|---|---|
| Nsight Systems（nsys） | 时间线：CPU/GPU 谁在等谁、kernel 间隙 | 先看有没有大段空隙（CPU 瓶颈或同步等待） |
| Nsight Compute（ncu） | 单 kernel：DRAM 吞吐、SM 吞吐、occupancy、stall 原因 | 只看 top-N 耗时 kernel，别全量跑 |
| PyTorch Profiler | 算子级耗时、显存 | 与 nsys 互补 |
| vLLM /metrics | 队列长度、KV 使用率、TTFT/ITL 直方图 | 生产环境首选，Prometheus 采集 |
| py-spy dump | Python 侧卡在哪 | 怀疑调度器 CPU 瓶颈时最直接 |

### ncu 的判读顺序
1. **DRAM Throughput > 80%** → 带宽瓶颈，去减少搬运（量化、融合、提高复用）。
2. **SM/Compute Throughput > 80%** → 算力瓶颈，去看 MMA 占比与指令效率。
3. **两者都不高，Occupancy 低** → 并行度不足或寄存器/共享内存限制。
4. **两者都不高，Occupancy 高** → 看 stall 原因（memory dependency、barrier、branch）。

## 五、故障定位对照表
| 现象 | 最可能原因 | 先查什么 | 处理 |
|---|---|---|---|
| GPU 利用率低、batch 上不去 | CPU 调度开销 / tokenize / 同步 | nsys 看 kernel 间空隙；py-spy | 开 CUDA Graph、减少每步 Python 工作、分离 tokenizer 进程 |
| KV 使用率 >95%，出现 preemption | 显存不足 | 日志里的 KV cache usage、preemption 计数 | 降 max_model_len、TP 扩卡、KV 量化、前缀缓存 |
| ITL p99 抖动大 | 长 prefill 插队 / chunk 太大 | 按请求长度分层统计 ITL | 调小 max_num_batched_tokens，开 chunked prefill |
| TTFT 高但负载低 | prefill 慢或排队 | 前缀缓存命中率、prefill 耗时 | 开前缀缓存、减少 prompt 冗余、PD 分离 |
| 吞吐随 batch 饱和 | 已到算力上限 | ncu 看 compute throughput | FP8、换更快 attention 后端、减少 TP 通信 |
| 多卡扩展比差 | all-reduce 开销 | NCCL 时间占比 | TP 限制在单机、改 PP、增大每卡 batch |
| 显存 OOM 但 KV 显示不满 | 激活/CUDA graph/NCCL buffer | 启动前后显存差 | 降 gpu_memory_utilization、减少 graph 捕获数 |

## 六、容量规划模板
1. 定 SLO：TTFT < 1s、ITL < 50ms、可用性 99.9%。
2. 估峰值流量：QPS × 平均输入/输出 token。
3. 算单副本并发（见 02 的 KV 公式）。
4. 算单副本吞吐：并发 / 平均时延。
5. 副本数 = 峰值 QPS / 单副本吞吐 × 冗余系数（1.3–1.5）。
6. 压测验证，按 p99 而不是均值定容量。

## 七、几条经验
- **优化前先量化基线**，否则无法判断改动是否有效；每次只改一个变量。
- **看 p99 不看均值**：平均值会掩盖长尾，而长尾决定用户体验。
- **别用合成流量定容量**：真实长度的双峰分布（短对话 + 长文档）对 KV 的冲击和固定长度完全不同。
- **任何「提升 N 倍」都要注明 batch、序列长度、精度、硬件**，否则无法复现。
