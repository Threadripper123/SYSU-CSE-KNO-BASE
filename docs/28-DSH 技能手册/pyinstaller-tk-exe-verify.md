# pyinstaller-tk-exe-verify

> 主题库：DSH 技能手册　｜　源文件：（纯文本条目）　｜　4,916 字符　｜　20 个片段　｜　更新于 2026-11-11
---
name: pyinstaller-tk-exe-verify
description: 验证与排查 Windows 上 PyInstaller 单文件 + tkinter 桌面程序的打包产物：确认新代码真的进了 exe（PYZ marshal 解包查模块/字符串/方法名）、跨进程驱动登录框、给窗口截图时避开系统菜单与 DPI 偏移、单实例互斥体探测，以及「改了源码但 exe 没变」的定位顺序。当任务是「重新打包并验收桌面 exe」「确认打包产物带上了本轮改动」「从外部自动化一个 Tk 程序」时使用。
---

# PyInstaller + tkinter 单文件 exe 的打包验收

## 适用场景

- 源码里改完了，需要证明**打包产物**也带上了（经典失败：只测源码，exe 是旧的）。
- 需要从进程外驱动一个 tkinter 程序（登录框、按键、截图）。
- 需要自动化给窗口截图并放进文档。
- 不适用：Electron/Qt/WPF、多文件 `--onedir` 打包、非 Windows。

## 前置条件

- Python 环境里装了同版本 PyInstaller（`from PyInstaller.archive.readers import CArchiveReader, ZlibArchiveReader`）。
- 有 GUI 会话（Tk 需要真实桌面；不要试图在无头会话里跑）。
- 目标进程可启动（单实例互斥体没被占）。

## 步骤

### 1. 先杀干净旧进程，再打包

```powershell
taskkill /IM 你的程序.exe /F
& python desktop\build_exe.py     # 用后台 job，几十秒到几分钟
```

打包完成先看 `Get-Item dist\程序.exe | Select Length, LastWriteTime`：**时间戳必须是刚才**。
时间戳没变说明 PyInstaller 用了缓存或者写文件失败（见「常见坑」第 3 条）。

### 2. 解包 exe，证明新代码在里面

```python
from PyInstaller.archive.readers import CArchiveReader, ZlibArchiveReader
archive = CArchiveReader(str(EXE))
names = [str(n) for n in archive.toc]
pyz = ZlibArchiveReader(str(extracted_pyz_path))   # archive.extract('PYZ.pyz') 写到临时文件
modules = sorted(pyz.toc)                          # 模块名清单
```

要点：

- **脚本入口（如 `app`）不在 PYZ 里**，它在 PKG 根；`pyz.extract('app')` 会抛 `KeyError`，
  要改用 `archive.extract('app')`。
- PYZ 里的内容是 **marshal 过的 code object**，不是源码字节。想断言「这段中文文案/这个方法名
  在不在」必须递归 `co_consts`（字符串常量）和 `co_names`（方法/全局名），不能拿 bytes 直接
  `decode` 再 `in`（一定会 MISS，浪费半小时）。

```python
def walk(code, consts, names):
    for item in getattr(code, 'co_consts', ()):
        if isinstance(item, str): consts.append(item)
        elif hasattr(item, 'co_consts'): walk(item, consts, names)
    names.extend(getattr(code, 'co_names', ()))

raw = pyz.extract('模块名')
code = raw if hasattr(raw, 'co_consts') else marshal.loads(raw if not isinstance(raw, tuple) else raw[0])
```

`complete_text`、`_init_xxx` 这类**方法名在 `co_names`**，中文提示词/文案在 `co_consts`。

### 3. 单实例互斥体：别被假进程骗了

```powershell
# C# P/Invoke OpenMutexW(0x00100000, 0, 'Local\程序名.1') → 非 0 表示已存在
```

`tasklist` 里有残留 PID 不代表互斥体还在（僵死进程 `Get-Process -Id` 甚至可能查不到）。
用 `OpenMutexW` 判定，返回 False 就可以放心启动新实例。

### 4. 跨进程驱动登录框

- Tk 的普通控件**没有原生 HWND**，`EnumChildWindows` 只能看到顶层窗口，无法点按钮/填输入框；
  唯一可靠的是把 `WM_CHAR` / `WM_KEYDOWN(VK_RETURN)` **PostMessage 到顶层窗口**——Tk 会把
  键盘消息转给当前焦点控件。登录框的输入框通常已经 `focus_force`，直接打字符即可。
- 登录成功后轮询 `EnumWindows` 找主窗口标题，再 `PostMessage(WM_CLOSE)` 优雅关闭。
- 结论：**登录类**流程可以自动化；任意业务控件（某个按钮、某个输入框）不要硬点坐标，
  改用「源码模式 + 注入假客户端」的方式测行为，打包版只验证「启动/登录/退出 + 内容在包里」。

### 5. 窗口截图（给文档用）

```python
image = ImageGrab.grab()                       # 整屏（物理像素）
scale = image.width / root.winfo_screenwidth() # 125% 缩放时 = 1.25
crop = image.crop((x*scale, y*scale, (x+w)*scale, (y+h)*scale))
```

- 不要用 `ImageGrab.grab(bbox=...)`：在高 DPI 下 bbox 被当作逻辑坐标，截出来偏。
- 截图前把窗口 `lift()` + `attributes('-topmost', True)`，并**清掉标题栏残留的系统菜单**：
  `PostMessageW(int(root.frame(), 16), 0x001F, 0, 0)`（WM_CANCELMODE）。
- **不要**用全局 `keybd_event(VK_ESCAPE)` 关菜单：那会切换前台窗口，截到别的程序。

### 6. 收尾

- 打包版跑过就会往 `dist/runtime/audit/*.jsonl` 写日志，交付前删掉。
- 重新跑一遍「源码模式行为验证 + 打包版启动/登录验证 + 包内容断言」，三者缺一不可。

## 常见坑

1. **改了源码，exe 还是旧的**：先看 `LastWriteTime`；没变就 `taskkill` 后重跑打包，
   再核对时间戳。别用「运行了没报错」当证据。
2. **PYZ 字符串断言全 MISS**：见步骤 2，marshal + `co_consts`/`co_names`。
3. **`ReplaceFileW EIO (Win32 32)`**：某个同步盘/杀毒在锁文件（百度网盘目录写代码时高发）。
   `edit`/`write` 会直接失败；用 Python 补丁脚本，`io.open(..., 'w', encoding='utf-8', newline='')`
   外面套 30–40 次 `except PermissionError: time.sleep(1)` 重试。
4. **截图里混进「大小 / 最小化 / 关闭」系统菜单**：用 WM_CANCELMODE 而不是全局 Esc。
5. **给窗口截图后内容被切**：窗口高度超过屏幕会被系统夹紧，`;-fullscreen` 之类要用整屏 grab；
   普通窗口先 `geometry()` 设成小于屏幕高度再截。
6. **DPAPI 存的密钥换目录后失效**：Windows DPAPI 是「用户+机器」绑定，但**路径**是代码自己
   决定的。凡是「设置可搬到别的目录」的程序，密钥文件必须跟随**配置目录**（并提供旧路径回退读），
   否则用户搬完目录就变成「已启用但没有密钥」，接口只报 401，很容易误判成网络问题。
7. **别把跨进程 exe 验收和进程内 Tk 用例并发跑**：exe 验收靠 `EnumWindows` 按标题找窗口、
   靠 `PostMessage`/前台焦点敲键，进程内用例一开 Tk 窗口就会互相干扰，结果是**两边各出现
   一条神秘失败**（本机踩过：exe 侧「登录界面已关闭」失败、进程内「错误后清空输入框」失败，
   各自单独跑全绿）。GUI 类验收一律串行：跑完一个再跑下一个。
8. **功能可以被开关关掉时，`source`/`client` 这类对象要能是 None 并被界面吃掉**：本轮加了
   「关掉数据库接口」的开关后，`_load_archives()` 直接 `self.source.archives()` 就抛
   `'NoneType' object has no attribute 'archives'`，用户看到的是一句技术栈原文。
   加开关的同时要检查**所有直接使用该对象的方法**，把「不可用原因」显示成人话，并写一条审计。

## 验证

- `LastWriteTime` = 本次构建时间；`Length` 与上一版对比有变化。
- 包内容断言脚本全绿（模块名、中文文案、方法名各有命中）。
- 打包版启动 → 登录 → 进主窗口 → 关闭 → 无残留进程（`tasklist` 前后差集为空）。
- 交付目录里没有 `audit.jsonl` 残留、`config` 里没有明文密钥。

## 参考

- PyInstaller `archive.readers`（`CArchiveReader.toc` / `ZlibArchiveReader.toc`）。
- Win32：`WM_CHAR=0x0102`、`WM_KEYDOWN=0x0100`、`VK_RETURN=0x0D`、`WM_CLOSE=0x0010`、
  `WM_CANCELMODE=0x001F`、`GWL_EXSTYLE=-20`、`WS_EX_TOPMOST=0x00000008`。

## 变更记录

- 2026-11-08: 初始版本，来自「干部人事档案助手」桌面 exe 多轮重建验收（语音大模型改字 +
  软件功能问答）；收录 PYZ marshal 断言、WM_CANCELMODE 截图、DPAPI 密钥路径缺陷三个真实坑。
- 2026-11-08（第二次）: 补两条——GUI 类验收不要并发跑（跨进程找窗口 + 进程内 Tk 用例互相
  污染，会各出一条假失败）；新增「功能可被开关关掉」时的 None 对象处理。
