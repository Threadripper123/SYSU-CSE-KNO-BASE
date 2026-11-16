# conn-github-cli

> 主题库：DSH 技能手册　｜　源文件：（纯文本条目）　｜　1,670 字符　｜　8 个片段　｜　更新于 2026-11-11
---
name: conn-github-cli
description: "GitHub API 与 gh CLI 连接器。通过 gh CLI 或 REST API 查询仓库、PR、Issue、Actions 状态并执行操作。仅在需要访问 GitHub 数据、批量处理 PR/Issue 或查 CI 结果时使用。"
---

# GitHub API 与 gh CLI 连接器

## 适用场景

- 查仓库信息、列 PR/Issue、看 Actions 运行结果、下载 release 产物。
- 批量操作：给多个 Issue 打标签、合并依赖机器人 PR、导出评审记录。
- 不适用：本地 git 操作（直接 git 命令即可，不需要 API）。

## 前置条件

- 优先用 `gh` CLI（已登录状态下 `gh auth status` 通过）；无 gh 时走 `curl` + REST。
- 本机实测：api.github.com 可达（约 170ms）；raw.githubusercontent.com 与 cdn.jsdelivr.net 超时，抓文件内容必须用 contents API 取 base64 再解码。
- 匿名调用有 60 次/小时限额；带 token 为 5000 次/小时。批量任务前先确认配额（`gh api rate_limit`）。

## 步骤

1. 先认证状态：`gh auth status`；未登录时让用户执行 `gh auth login`，不要试图代填凭据。
2. 读数据统一走 `gh api` + `--jq` 过滤，避免拉回巨大 JSON：
   - 仓库：`gh api repos/{owner}/{repo} --jq '.stargazers_count'`
   - PR 列表：`gh api repos/{owner}/{repo}/pulls?state=open --jq '.[].title'`
   - 目录内容：`gh api repos/{owner}/{repo}/contents/{path} --jq '.[].name'`
   - 取文件：`gh api repos/{owner}/{repo}/contents/{path} --jq '.content' | base64 -d`（内容大时用 raw 字段）
3. 批量遍历用分页参数 `--paginate`，并显式设 `per_page=100`。
4. 写操作（评论、打标签、合并）前先 dry-run 打印将受影响的对象列表，得到确认后再执行。
5. Actions 排查：`gh run list --limit 10` → `gh run view <id> --log-failed` 直接定位失败步骤。

## 常见坑

- 匿名限额耗尽后所有调用 403，误判成权限问题；先看 `X-RateLimit-Remaining`。
- `gh api` 默认输出全部字段，巨量输出把上下文撑爆，必须配 `--jq`。
- 用 raw.githubusercontent.com 直接抓文件在本机会超时，必须换 contents API。
- 合并 PR 不等检查完成就执行，导致合入失败分支；合并前用 `gh pr checks` 确认状态。
- 组织私有仓库需要 `repo` scope，`read:org` 不足。

## 验证

- 每条命令给出可复现的完整参数与过滤后的输出。
- 写操作执行后回读确认状态已变更。
- 报告限额剩余情况，避免后续任务被限流打断。

## 参考

- 与 `gh-code-review`（评审 PR）、`gh-devops-delivery`（CI 流水线排查）配合。

## 变更记录

- 2026-11-11: 初始版本，记录 raw.githubusercontent 不可达的绕行方案与 jq 过滤要求。
