# conn-web-automation

> 主题库：DSH 技能手册　｜　源文件：（纯文本条目）　｜　1,484 字符　｜　8 个片段　｜　更新于 2026-11-11
---
name: conn-web-automation
description: "浏览器与网页自动化连接器。用 Playwright CLI 驱动真实浏览器完成抓取、登录、提交表单与端到端验收。仅在需要操作网页、抓动态渲染内容或做 UI 自动化测试时使用。"
---

# 浏览器与网页自动化连接器

## 适用场景

- 页面内容靠 JS 渲染，普通 HTTP 抓取拿不到正文。
- 需要登录态下操作后台、批量提交表单、下载报表。
- 端到端验证：点击、输入、断言页面状态。
- 不适用：静态页面（一次 `web_fetch`/curl 更快）；只读文档查询（用 `gh-web-search`）。

## 前置条件

- 本机 Node v24 可用；装 Playwright：`npm i -D playwright` 后 `npx playwright install chromium`。
- 下载浏览器二进制可能较慢，内网受限时配置镜像（`PLAYWRIGHT_DOWNLOAD_HOST`）或复用已装 Chrome（`channel: 'chrome'`）。
- 遵守目标站点条款与 robots；不绕过验证码或风控。

## 步骤

1. 先用无头模式做侦察：`npx playwright codegen <url>` 生成操作脚本骨架，比手写选择器快得多。
2. 选择器策略：优先 `role`/`label`/`text` 等语义选择器，其次稳定 data 属性，最后才用 CSS 路径；绝不用会自动变化的 class 名。
3. 等待策略：用 `waitForSelector`/`waitForResponse` 等条件等待，禁用固定 `sleep`。
4. 登录态复用：`context.storageState({ path })` 保存，后续运行直接加载，避免每次重新登录。
5. 抓取页面数据：优先在页面上下文里执行提取函数一次性返回结构化数组，而不是逐节点跨进程往返。
6. 失败可诊断：出错时截图 + 保存 HTML，附在报告里。
7. 交付脚本时给出运行命令、依赖安装步骤与预期输出样例。

## 常见坑

- 页面在 iframe 内，顶级选择器永远找不到；先定位 frame。
- SPA 路由切换后旧元素句柄失效，需重新查询。
- 请求被反爬拦截：检查 UA、并发过高、缺少必要 Cookie；降速并复用会话比伪装更有效。
- 无头模式下部分站点行为不同（字体、Canvas 指纹），必要时切 `headless: false` 复现。
- 截图尺寸受 DPI 影响，Windows 上需显式设置 `deviceScaleFactor`。
- 长时间运行的浏览器进程残留，验收结束显式 `browser.close()`。

## 验证

- 同一次任务连续运行两遍，结果一致。
- 失败路径给出截图与页面 HTML 片段。
- 明确报告是否使用了登录态、是否修改了站点数据。

## 参考

- 上游：microsoft/playwright-cli（Apache-2.0，skills/playwright-cli）。
- 与 `gh-web-search`（只读检索）分工：前者找资料，本技能操作页面。
- 与 `pyinstaller-tk-exe-verify`（桌面程序外部驱动）思路相通：都靠截图与结构断言验收。

## 变更记录

- 2026-11-11: 初始版本，确立语义选择器与条件等待原则。
