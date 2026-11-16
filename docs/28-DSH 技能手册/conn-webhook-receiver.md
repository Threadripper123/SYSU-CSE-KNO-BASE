# conn-webhook-receiver

> 主题库：DSH 技能手册　｜　源文件：（纯文本条目）　｜　1,507 字符　｜　8 个片段　｜　更新于 2026-11-11
---
name: conn-webhook-receiver
description: "Webhook 与消息推送连接器。本机收 Webhook、调机器人/邮件/IM 接口推送通知并做签名校验与去重。仅在需要让外部系统触发本机任务或把结果推送到群/邮箱时使用。"
---

# Webhook 与消息推送连接器

## 适用场景

- 外部系统（GitHub、n8n、监控告警、表单）需要触发本机脚本。
- 任务完成后推送到企业微信/钉钉/飞书/Telegram 群或邮件。
- 不适用：纯站内通知；只读数据查询。

## 前置条件

- 本机对外可达性：内网直连、内网穿透（frp/ngrok）或轮询替代。
- 目标平台机器人 webhook URL 或 SMTP 账号；密钥一律走环境变量。
- Windows 防火墙需放行监听端口（`New-NetFirewallRule`）。

## 步骤

1. **接收端**：写最小 HTTP 服务（Python `http.server` 或 Flask/FastAPI），只暴露一条路径，POST 进来先 dump 原始 body 到日志再解析。
2. **安全校验三件套**：签名/HMAC 校验（GitHub 用 `X-Hub-Signature-256`）、时间戳窗口防重放、来源 IP 白名单（可选）。校验失败直接 401，不解析业务。
3. **幂等去重**：用事件 id 或 body hash 记录已处理（sqlite/文件，带过期清理）；重复投递不重复执行。
4. **快速响应**：处理耗时超过几秒就先回 202，任务丢后台队列执行；平台通常有 5–10 秒超时，超时会被判失败并重投。
5. **推送端**：
   - 企业微信/钉钉/飞书机器人：POST JSON 到 webhook，注意各平台字段名不同（`markdown.content` / `msgtype`），先用最小 body 验证再拼格式。
   - Telegram：`https://api.telegram.org/bot<TOKEN>/sendMessage`，注意消息长度上限 4096。
   - 邮件走 SMTP，密码用授权码而非登录密码。
6. 推送内容包含：发生了什么、影响范围、关键 id、下一步动作；失败重试 3 次后落 critical 日志。

## 常见坑

- 平台重投导致同一条告警重复发：必须按事件 id 去重。
- 在内网环境里让外部直接访问本机失败：优先改成"本机主动轮询"或经穿透，不要假设公网可达。
- 监听 0.0.0.0 且无校验 = 任何人都能触发你的脚本。
- 大 payload 卡住主线程：限制请求体大小（如 1MB）再拒绝。
- 中文乱码：明确 `Content-Type: application/json; charset=utf-8`。
- 机器人 webhook URL 泄露等于泄漏发送权限，不进 git、不贴日志。

## 验证

- 用 curl 发一条真实 payload，确认 200 且任务实际执行。
- 重发同一条 payload，确认被去重（不产生第二次执行）。
- 签名错误时返回 401 且无副作用。
- 推送目标群实际收到消息，格式正确。

## 参考

- 与 `conn-n8n-workflow`（流程侧）、`gh-automation-workflow`（幂等与状态外置）配合。

## 变更记录

- 2026-11-11: 初始版本，明确签名校验、去重与快速响应的硬要求。
