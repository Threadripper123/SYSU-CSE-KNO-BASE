# astrbot-qq-image-recovery

> 主题库：DSH 技能手册　｜　源文件：（纯文本条目）　｜　3,373 字符　｜　8 个片段　｜　更新于 2026-11-11
---
name: astrbot-qq-image-recovery
description: 排查并修复 AstrBot + NapCat 的 QQ 图片 HTTP400、失效鉴权、重复下载及历史图片导致整次 agent 请求失败；适用于本机源码热修与表情收藏取图。
---

# AstrBot QQ 图片恢复

## 适用场景

图片下载HTTP400/403、multimedia.nt.qq.com.cn临时URL失效、引用图片失败后agent再次下载、表情收藏URL存在却取不到。不是模型视觉能力验证或普通网络故障的通用替代。

## 前置条件

定位实际AstrBot安装、数据根及NapCat进程；从日志提取host/status/call site，禁止打印URL查询参数、鉴权、聊天正文。只读源码使用read，内容搜索用grep。使用该安装自带Python并先import astrbot.api再import core，独立验证把ASTRBOT_ROOT指向临时测试目录。

## 步骤

1. 搜错误原文的抛出位置。4.27.4中utils/io.py的HTTP失败异常可由MediaResolver抛出；先区分pipeline/preprocess_stage、astr_main_agent、provider/entities及provider/sources/openai_source路径。
2. 预处理图像成功后使component.file/path/url三者一致。Image.convert_to_file_path优先url；只改file会再下载旧URL。
3. 正常下载失败后，只对aiocqhttp和QQ图片域名尝试通过当前连接bot.call_action('get_image',file=原文件标识)重新取图。NapCat本地get_image返回file可能是客户端缓存绝对路径，url也可能是本地路径；不能只认远程url。仅信任API返回的缓存路径，不读取聊天原始字段指定的本地路径。验证缓存字节大小、图片格式及像素，再用本地副本。
4. 对multimedia.nt.qq.com.cn的失效URL，可调用当前已连接NapCat的get_rkey，按其返回结构匹配private/group。当地源码显示appid1406用private、1407用group。替换rkey时保留fileid和其他参数的原编码；不要parse_qs后整条urlencode。未知appid不猜。不得记录返回rkey，不关TLS验证。
5. 当前或Reply图片最终失败时把Image替换成明确缺图的Plain占位，提示重新发送；不要让模型猜内容。Reply失败要同时阻止astr_main_agent引用fallback重新提取原URL，否则会绕回失败路径。
6. 引用id-only fallback、历史消息仍可能携带远程URL，检查至少三个独立出口：ProviderRequest.assemble_context、OpenAI assemble_context(image_urls)、OpenAI _transform_content_part。逐图catch/None返回text不可用占位，保留正常图和其他文本；不能catch后原样透传失效image_url。历史占位使用payload副本，不删除用户历史数据库。
7. 表情采集器另走原始OneBot消息，不会自动复用preprocess的归一化；它也需要失效后get_image/刷新回退。域名白名单若包含qq.com不代表包含qq.com.cn；需显式正确后缀及点边界，保留下载大小和超时限制。
8. 先写工作区staging与小范围验证，再备份原模块、记录hash、复制部署。重启实际运行的原桌面/后端；不另起替代实例。验证WebUI HTTP200及OneBot连接ESTABLISHED和加载日志。保留备份与回退清单，指出应用升级可能覆盖本地源码补丁。

## 常见坑

- 预处理catch仅日志并保留Image，后续build_main_agent无catch再次下载，整次失败。
- provider的history处理catch后return part仍把失效URL发给上游；应text占位。
- provider修好了但ProviderRequest更早在reset组装图片，仍会抛出；需按实际调用顺序验证。
- NapCat可能只有reverse WS连接；不要假定3001或HTTP API仍监听，也不要擅自开新接口。使用event.bot已有连接。
- PreProcessStage.process在该版本是coroutine，应await，不是async for。
- 原始插件图片下载与框架正常图像输入是不同路径，需分别验证。

## 表情库为空与群相册变体

- 检查SQLite实际count再区分“未采集”和“模型未查询”。QQ默认人格可由resolve_selected_persona返回default/None；若插件以persona必须dict为门槛，队列会静默丢弃且无下载/保存日志。共享图片库应在persona判定前确定范围，仍核验session启用；不要把表情图片共享与聊天记忆共享混为一谈。
- 本机aiocqhttp.Event为dict子类、属于Mapping，不要凭类型名字猜segments不兼容。回归应使用Event.from_payload→真实适配器→正式data.plugins模块注册的CustomFilter→worker→SQLite，检查INSTANCE无双module分离；合成dict不能覆盖默认persona缺口。
- 未带QQ表情标记的图可按用户需求送分类器，只在判定是表情时保存；模型请求始终给真实库存count，空关键词可列最近收藏，关键词未命中不等于库为空。
- NapCat只读相册API为get_qun_album_list和get_group_album_media_list；照片ID可在cell_media.media_items[].image.lloc，不能用feed ID替代。相册列表为attach_info/has_more，实机照片页则为next_attach_info/next_has_more（同时还含prev字段，不能用于向后翻页）；分页需完整保留在会话绑定短cursor里，也保留本页未展示的项。照片地址可在image.default_url.url或image.photo_url[].url.url。
- 相册工具每次从当前event获取群号，核验profile plugin_set和session开关，不接受任意群号或图片URL；图片仅从已核验媒体条目取得。
- aiocqhttp CQHttp.api是UnifiedApi，启动探针不能在它上面检查_api_clients；实际CQHttp._wsr_api_clients才是当前反向连接集合。只在明确只读验证请求时调用，不能把启动未连接误判为相册无权限。

## 验证

- 合成HTTP失败：get_image返回合法缓存则保留原像素；坏缓存拒绝；不读取原消息本地路径。
- 鉴权刷新：保持fileid百分号编码，private/group正确，无可用结果有限尝试后退出。
- 当前和Reply最终失败成为Plain，成功图三引用一致；Reply不会再次fallback。
- 当前图片列表和历史图片逐图异常/None，request仍保留其余文字及有效图片；输入历史不变。
- 模块真实安装导入通过、部署hash一致、人格SHA与配置未改。不得把合成测试说成用户失败原图已恢复；原图若失效且无缓存，需要用户重发。

## 变更记录

- 2026-11-11：来自已验证的QQ HTTP400修复，记录多阶段失败占位、缓存恢复与鉴权刷新边界。
- 2026-11-11：补充真实QQ事件、默认人格漏采集、共享库存和群相册分页/连接就绪边界。
