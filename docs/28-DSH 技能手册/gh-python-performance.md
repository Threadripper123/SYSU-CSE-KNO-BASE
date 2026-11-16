# gh-python-performance

> 主题库：DSH 技能手册　｜　源文件：（纯文本条目）　｜　730 字符　｜　5 个片段　｜　更新于 2026-11-11
---
name: gh-python-performance
description: "Python后端性能优化。优化Python后端CPU、内存、I/O与并发瓶颈。仅在相关任务时使用。"
---

# Python后端性能优化

仅在任务匹配本技能时应用。当前用户要求、系统规则与实际工具权限优先。此为GitHub上游技能的本机兼容适配版，默认中文输出。不要假定上游列出的工具、包、API密钥或Linux命令已可用；先检查环境，优先当前软件已有工具。不要自动调用收费服务、上传本地私密资料或运行上游安装命令。

优化Python后端CPU、内存、I/O与并发瓶颈。先复现、建立timeit或基准指标，使用cProfile定位热点，必要时观察分配与峰值内存。优先算法复杂度、数据结构、批量I/O、生成器和避免重复计算；缓存须定义失效策略。异步适合等待密集任务，CPU任务评估多进程和进程间传输成本。保留异常、顺序、线程安全与资源释放语义。对热点进行小步修改，以同负载多次测量比较延迟、吞吐和内存，注明环境与波动，不虚构加速比例。

上游来源：https://github.com/wshobson/agents/tree/4bde3168b43c0655c4d00ea2726e631719908a6d/plugins/python-development/skills/python-performance-optimization

详细参考与脚本见本技能目录的 UPSTREAM.md、references/、scripts/ 或其他附属文件；只在需要时读取。以上兼容约定替代上游关于预装环境、强制服务和默认数量的假设。
