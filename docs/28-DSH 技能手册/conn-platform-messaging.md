# conn-platform-messaging

> 主题库：DSH 技能手册　｜　源文件：（纯文本条目）　｜　1,507 字符　｜　8 个片段　｜　更新于 2026-11-11
---
name: conn-platform-messaging
description: "消息与邮箱平台连接器。通过 IMAP/SMTP 读写邮件、调用 IM 群机器人、同步云文档与云盘接口。仅在需要收发邮件、群内通知或对接办公平台 API 时使用。"
---

# 消息与邮箱平台连接器

## 适用场景

- 读取邮箱（收件箱、附件下载、按主题/发件人筛选）。
- 发送带附件的邮件或批量通知。
- 对接办公平台 API（云文档、云盘、日程）。
- 不适用：站内对话；简单的告警推送（用 `conn-webhook-receiver` 更轻）。

## 前置条件

- IMAP/SMTP 主机、端口、账号；多数邮箱需要"授权码"而不是登录密码。
- 服务商文档中的限流规则先查清（SMTP 每小时发信上限、IMAP 连接数上限）。
- 密钥存环境变量或系统凭据管理，不落盘明文。

## 步骤

1. **读邮件**：Python `imaplib` + `email` 库。流程：`connect → login → select('INBOX') → search(criteria) → fetch(RFC822) → 解码`。
   - 搜索条件用服务器端语法 `(SINCE 01-Nov-2026 UNSEEN)`，不要在本地拉全量再过滤。
   - 正文多段编码（base64/quoted-printable）；用 `msg.walk()` 找 `text/plain`，没有再用 `text/html` 转文本。
   - 附件用 `get_payload(decode=True)` 写文件，文件名先做路径清洗（防 `..` 穿越）。
2. **发邮件**：`smtplib` + `EmailMessage`。中文主题要 `Header(subject, 'utf-8')`；附件用 `add_attachment` 并显式给 maintype/subtype。
3. **IM 群机器人**：功能受限（多为单向推送），需要"接收用户消息"的场景走官方 API + 回调而不是机器人。
4. **云文档/云盘**：优先用平台提供的官方技能或 SDK；自写时注意 token 有效期与刷新，分页参数必填。
5. 批量操作节制：每批之间加间隔，失败按指数退避，单次任务记录总调用次数。

## 常见坑

- IMAP 连接长期不关闭导致服务端连接数耗尽；用完必须 `close()` + `logout()`。
- 编码错乱：主题/发件人名字段需 `decode_header` 逐个解码，中文最易踩。
- 附件名在 Windows 上含非法字符（`:` `?` `*`）导致写盘失败，需过滤。
- 群机器人被禁言或密钥轮换后静默失败，需回读响应体判断 `errcode`。
- 一次拉取整年邮件把内存打满，用 UID 分页与时间窗口。
- 把邮箱密码写进脚本，提交后泄露；用授权码 + 环境变量。

## 验证

- 读取一封已知邮件，正文与附件内容与预期一致。
- 发一封测试邮件，收件方实际收到且中文正常。
- 密钥缺失时给出明确配置提示，而不是抛连接异常。

## 参考

- 待补上游（ClawHub imap-smtp-email），当前为自建。
- 与 `conn-webhook-receiver`（推送）、`gh-office-pdf-extra`（附件处理）配合。

## 变更记录

- 2026-11-11: 初始版本，记录 IMAP 编码与连接释放的典型坑。
