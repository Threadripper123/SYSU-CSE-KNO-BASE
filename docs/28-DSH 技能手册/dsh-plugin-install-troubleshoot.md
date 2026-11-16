# dsh-plugin-install-troubleshoot

> 主题库：DSH 技能手册　｜　源文件：（纯文本条目）　｜　2,977 字符　｜　10 个片段　｜　更新于 2026-11-11
---
name: dsh-plugin-install-troubleshoot
description: 在 Windows 上给 DeepSeek Harness（DSH）desktop profile 安装/升级插件时使用：处理 pnpm 大包下载假死（换腾讯镜像）、allowBuilds 原生构建许可、包已进 node_modules 却没进 dsh.profile.bundles、DSH 自动升级后插件版本闸门失效，以及长安装超过 run_code 120s 上限。适用于「给 DSH 加知识库/技能库等插件」「dsh plugin add 卡住」「插件装了但界面没出现」这类任务。
---

# DSH 插件安装与排障（Windows desktop profile）

## 适用场景

- 用户要求给 DSH 安装/升级插件（如 dsh-knowledge 知识库、dsh-skills-manager 技能库）。
- `dsh plugin add` 长时间无输出、看起来卡死。
- 安装命令跑完，UI 里却没有新插件；或启动时报插件与 DSH 版本不兼容。

## 前置条件

- CLI：`C:\Users\CCW\AppData\Local\Programs\DeepSeek Harness\resources\runtime\cli\bin\dsh.cmd`
- Profile：`%USERPROFILE%\.dsh\profiles\desktop`（Electron 独占管理，不能用 `--dump-config` 校验，只能重启应用验证）
- 关键文件：`package.json`（`dsh.profile.bundles` 是插件层栈）、`pnpm-workspace.yaml`（allowBuilds 等 pnpm 设置）、`cordis.patch.yml`（用户补丁层）

## 步骤

1. `dsh --version` 记录当前 DSH 版本；读 profile 的 `package.json` 与 `pnpm-workspace.yaml`。
2. 备份：`Copy-Item package.json package.json.bak-<原因>`、`pnpm-workspace.yaml` 同样处理。
3. 若目标插件含原生构建（dsh-knowledge 需要），在 `pnpm-workspace.yaml` **合并**写入（不要出现第二个 `allowBuilds:` 键）：
   ```yaml
   allowBuilds:
     esbuild: true
     onnxruntime-node: true
     protobufjs: true
     sharp: true
     tesseract.js: false
   ```
4. 安装（大包一定要带腾讯镜像）：
   ```
   dsh plugin --profile desktop add dsh-knowledge --registry https://mirrors.cloud.tencent.com/npm/
   ```
5. 用后台任务跑，别用前台：`run_in_background: true` + `job_output(wait)`；`run_code` 默认 120s 会杀掉长安装（除非给 run_code 传 `timeoutMs`）。
6. 判断真实进度：看 `%LOCALAPPDATA%\pnpm\store\v11\files` 最新 LastWriteTime、或 `node_modules/<pkg>` 是否出现；stdout 因 `Out-String` 缓冲往往一直为空。
7. 结束后核对 `package.json` 的 `dsh.profile.bundles` 是否出现在列表里；没有则执行第 8 步修复。
8. 包已在 `node_modules` 但不在 bundles：
   ```
   dsh plugin --profile desktop remove dsh-knowledge
   dsh plugin --profile desktop add dsh-knowledge --registry https://mirrors.cloud.tencent.com/npm/
   ```
   第二次 add 会跑 protobufjs / onnxruntime-node 的 postinstall（约 21s），成功后 bundles 末尾出现包名。
9. 重启 DSH（关掉应用重开）后验证新插件工具可用。

## 常见坑

- **pnpm 假死其实在下载**：onnxruntime-node 的 tarball 有 96.8MB。npmjs ≈0.22MB/s、npmmirror ≈0.065MB/s（要 20 多分钟），腾讯镜像 ≈2.2MB/s。输出停在 `added 111/118` 不代表失败。
- **bundle 登记只在 pnpm 真正新增包时发生**：命令被中断后依赖已写进 package.json/lockfile，再 add 只会打印 `Packages: -78`、`Already up to date` 而不登记。必须 remove + add。
- **DSH 自动升级会废掉版本豁免**：0.2.1-rc.5 → 0.2.2-rc.1 后 billion-context-dsh@0.2.27 被闸门拒绝。修复：
  `dsh plugin --profile desktop allow-version billion-context-dsh@0.2.27 --dsh-version 0.2.2-rc.1 --accept-risk`
- **每次 add 都打印 `Packages: -78`**：pnpm 反复清理 78 个树外遗留包，属既有现象，不影响使用。
- 安装期间 DSH 可能自动升级并重启（GUI 端口会变），后台 job 会随进程消失，需重新核对状态而不是等 job。

## 验证

- `node_modules/<pkg>/package.json` 的 version 正确。
- `package.json` 的 `dsh.profile.bundles` 含包名。
- 重启后调用插件工具成功，例如 dsh-knowledge：`knowledge_list_bases` 返回 `{"bases":[]}`、`knowledge_stats` 返回 0 计数。
- 原插件未被误删：逐项 `Test-Path node_modules/<原有包>`。

## 变更记录

- 2026-11-11: 初始版本，来自给 desktop profile 装 dsh-knowledge 0.5.3、升级 dsh-skills-manager 1.4.1 的实际排障（慢镜像、bundle 未登记、版本闸门）。
