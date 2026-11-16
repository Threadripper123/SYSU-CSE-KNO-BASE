# gh-api-design

> 主题库：DSH 技能手册　｜　源文件：（纯文本条目）　｜　673 字符　｜　5 个片段　｜　更新于 2026-11-11
---
name: gh-api-design
description: "后端API设计。设计或审查REST/GraphQL接口与契约。仅在相关任务时使用。"
---

# 后端API设计

仅在任务匹配本技能时应用。当前用户要求、系统规则与实际工具权限优先。此为GitHub上游技能的本机兼容适配版，默认中文输出。不要假定上游列出的工具、包、API密钥或Linux命令已可用；先检查环境，优先当前软件已有工具。不要自动调用收费服务、上传本地私密资料或运行上游安装命令。

设计或审查REST/GraphQL接口与契约。列明资源、调用者和权限，定义请求响应、状态码、错误结构、字段约束、分页、排序、版本兼容和幂等语义。REST使用资源名及正确方法；GraphQL限制查询复杂度并避免N+1。明确鉴权授权、输入校验、速率限制、超时、日志脱敏和观测指标。提供OpenAPI或清晰示例，以及正常、边界、无权限、重复提交与失败恢复的验证用例。

上游来源：https://github.com/wshobson/agents/tree/4bde3168b43c0655c4d00ea2726e631719908a6d/plugins/backend-development/skills/api-design-principles

详细参考与脚本见本技能目录的 UPSTREAM.md、references/、scripts/ 或其他附属文件；只在需要时读取。以上兼容约定替代上游关于预装环境、强制服务和默认数量的假设。
