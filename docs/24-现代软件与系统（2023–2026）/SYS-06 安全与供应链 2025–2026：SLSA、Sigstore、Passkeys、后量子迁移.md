# SYS-06 安全与供应链 2025–2026：SLSA、Sigstore、Passkeys、后量子迁移

> 主题库：现代软件与系统（2023–2026）　｜　源文件：（纯文本条目）　｜　7,436 字符　｜　34 个片段　｜　更新于 2026-11-15
> 资料截至 2026-11-15；覆盖 2023-11 至 2026-11，重点 2024–2026。

**一句话结论**：2025–2026 年的安全工程被两条线同时拉动——**"证明"**（用签名元数据证明软件从哪来、由谁构建，SLSA + Sigstore）和**"迁移"**（在量子计算机到来之前把密码算法换掉，PQC + passkeys）。两条线有一个共同点：信任正从"看起来可信"变成"可验证"，而可验证的代价是元数据、体积和运维复杂度全面上升。

## 一、关键时间线

| 时间 | 事件 | 意义 | 证据等级 |
|---|---|---|---|
| 2024-08 | NIST 发布 `FIPS 203`（ML-KEM）、`FIPS 204`（ML-DSA）、`FIPS 205`（SLH-DSA）最终版 | 后量子算法从"候选"变成"标准" | 一手（NIST CSRC 页面标题） |
| 2024-11 | NIST `IR 8547`《Transition to Post-Quantum Cryptography Standards》初始公开草案 | 给出"何时迁、先迁什么"的官方时间表 | 一手（CSRC 页面标题） |
| 2026-08-25 | `WebAuthn Level 3` 成为 W3C Recommendation | passkeys 的浏览器 API 基线定稿 | 一手（W3C REC 页） |
| 2026-10-02 | Sigstore 发布《Post-Quantum Cryptography for Sigstore》，调整路线 | Google/Cloudflare 把迁移提前到 **2029**，多国政府提前到 **2030** | 一手（Sigstore 官方博客） |
| 2026-11 | `SLSA v1.2` 状态 Approved，新增 **Source Track** | 供应链保证从"构建"扩展到"源码修改历史" | 一手（slsa.dev） |
| 在研 | IEEE 802.11 新设 `TGbt`（Post-Quantum Cryptography） | Wi-Fi 认证与管理帧保护也要 PQC 化 | 一手（IEEE 802.11 工作组页） |

## 二、SLSA v1.2：从"怎么构建"扩展到"源码怎么被改"

SLSA 解决的是一个很具体的问题：**你拿到的二进制，和仓库里的那份源码，是不是同一回事？** 它用"等级"表达保证强度，用"provenance（来源证明）"承载证据——一段由构建平台签名、记录 commit、构建参数、builder 身份的元数据。

2026 年 11 月的状态是 `SLSA v1.2`，状态 **Approved**。按官方 "What's new" 页面的说法，v1.2 相对 v1.1 的变化集中在三件事：

1. **新增 Source Track**：v1.1 及以前只有 Build Track（L1–L3），管的是"构建过程"；Source Track 管的是"源码在合并前经历了什么"。
2. **更新威胁模型**，把 Source Track 能缓解的威胁纳入。
3. **重构规范结构**，让"多 track"成为组织方式，而不是特例。

Source Track 的分级逻辑值得记住：**L2 关注历史与来源**（提交历史是否可追溯、变更是谁批准的），**L3 关注技术控制的持续强制执行**（不是"有制度"，而是"分支保护、双人评审、签名提交被机器强制"）。这个区分很关键——大量组织声称自己有代码评审流程，但没有任何机制强制它。

Build Track 仍然是 L1–L3：L1 只要求"构建过程有文档、provenance 存在"，L2 要求"由托管构建服务生成、provenance 可防篡改"，L3 要求"构建环境强隔离、密钥不可被构建过程窃取"。落到实践：

```bash
# 验证产物是否由指定仓库、指定 builder 构建
slsa-verifier verify-artifact ./app-linux-amd64 \
  --provenance-path ./app.intoto.jsonl \
  --source-uri github.com/example/app

# 验证 attestation 的签发身份（keyless 场景）
cosign verify-attestation --type slsaprovenance \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com \
  --certificate-identity-regexp '^https://github.com/example/app/' \
  ghcr.io/example/app@sha256:<digest>
```

要注意 SLSA 等级**不是安全评级**：L3 说明"构建链路可信"，完全不说明"代码里没有漏洞"。它是"来源可信度"的度量，不是"质量"的度量。这是最常见的误读。

## 三、Sigstore：把签名变成基础设施，而不是仪式

Sigstore 的贡献不是"又一个签名工具"，而是把签名从"管理长期密钥"变成"用短期身份换签名"：

- **Fulcio** 签发短寿命证书（通常几分钟），证书里的身份来自 OIDC（GitHub Actions、Google 账号等）。
- **Rekor** 是透明日志（Merkle 树），把"某身份在某时刻签了某产物"变成可公开审计的记录。
- **cosign** 是面向 OCI 镜像与 blob 的客户端。

这套"keyless"模型的真正价值在于：**没有长期私钥可偷**。攻击者拿到 CI 的临时 token，能签的东西也只在那个时间窗口内有效，且会留下公开日志痕迹。

2026 年 10 月 2 日，Sigstore 官方博客发了一篇路线调整文章，标题就是《Post-Quantum Cryptography for Sigstore》。文中给出的判断很直白：PQC 已经从"理论探索"变成"immediate infrastructure necessity"，理由是 Google 与 Cloudflare 把迁移时间线**提前到 2029 年**、多国政府提前到 **2030 年**，所以 Sigstore 也要"立即支持 PQC 算法"。

这背后有一个容易被忽略的矛盾：**供应链签名的验证寿命，比签名算法本身的安全寿命更长**。你今天签的 release，五年后还要能被验证；如果届时 ECDSA 已被认为不安全，"当时的签名"就失去了证明力。所以 Sigstore 的 PQC 迁移不只是"换算法"，还要解决：

- 已存在的 Rekor 条目如何在新算法下继续可验证（重签、双签、还是保留旧算法但加时间戳证据）；
- 长寿命产物的"密码学证据保全"（先把证据存下来，等算法成熟再验证）；
- 客户端兼容（老版本 cosign 读到新算法条目时的降级行为）。

这是整个行业在 2026 年共同面对的问题，不只是 Sigstore 一家。

## 四、Passkeys：API 定稿了，产品问题才刚开始

`WebAuthn Level 3` 在 2026-08-25 成为 W3C Recommendation，这件事的技术含义很具体：passkeys 的浏览器 API 不再是"草案"，跨浏览器行为有了正式基线。配套的 CTAP（客户端到认证器的协议）负责设备与浏览器之间的通信。

Passkeys 用公钥替代密码：注册时设备生成密钥对，私钥留在认证器（安全芯片、手机、密码管理器），服务器只存公钥。登录时服务器发随机挑战，设备用私钥签名。**没有共享秘密可钓**——这是它对钓鱼攻击结构性有效的原因，也是它被各类合规框架归入"phishing-resistant MFA"的原因。

但工程上要区分两种形态：

| 形态 | 私钥在哪 | 优点 | 风险 |
|---|---|---|---|
| 设备绑定（device-bound） | 单台设备的安全芯片 | 无法被复制，强度最高 | 设备丢失即失效，需要备用认证器 |
| 同步（synced） | 平台密码管理器（iCloud/Google/第三方） | 跨设备无缝，恢复容易 | 安全性取决于平台账户，云账户被攻破则连带 |

真正的难点不在注册，在**恢复**。密码可以重置，passkey 不能"重置"——只能走账户恢复流程，而恢复流程往往是整个系统最弱的一环（邮件验证、短信、客服人工核验）。很多组织上线 passkey 后才发现：攻击者不再钓密码，转而攻击恢复入口。所以 passkey 的部署必须与"恢复策略"一起设计，而不是当作一个登录按钮换掉。

## 五、后量子迁移：三类算法、一张参数表

NIST 的三份标准分工明确：

| 标准 | 算法 | 用途 | 公钥大小 | 密文/签名大小 | 备注 |
|---|---|---|---|---|---|
| `FIPS 203` | ML-KEM（原 Kyber） | 密钥封装（KEM） | 800 / 1184 / 1568 B（512/768/1024） | 768 / 1088 / 1568 B | TLS 混合密钥交换的主力 |
| `FIPS 204` | ML-DSA（原 Dilithium） | 数字签名 | 1312 / 1952 / 2592 B（44/65/87） | 2420 / 3309 / 4627 B | 证书、代码签名、固件签名 |
| `FIPS 205` | SLH-DSA（原 SPHINCS+） | 无状态哈希签名 | 32 B（128s） | 7856 B（128s） | 慢但假设最少，做"保守备份" |

（表中大小为公开参数值，用于量级比较；不同参数集的取舍见"不确定项"。）

这张表解释了迁移为什么难：**签名从 64 字节变成 3.3 KB，公钥从 32 字节变成 1.9 KB**。对单次 TLS 握手，多几 KB 还能接受；对证书链、OCSP 响应、嵌入式设备的固件分区、以及那些在协议里写死了"签名字段 64 字节"的系统，就是硬约束。

工程上的主流做法是**混合模式**：TLS 1.3 里用 `X25519MLKEM768` 这类混合组，把经典 ECDH 与 ML-KEM 的共享秘密一起派生会话密钥。这样即使 ML-KEM 日后被攻破，X25519 仍在；反之亦然。混合不是"过渡期的权宜"，而是**降低单点算法风险的标准做法**，预计会长期保留。

迁移优先级可以按"数据寿命"排序，而不是按"系统重要性"：

| 优先级 | 对象 | 理由 |
|---|---|---|
| 最高 | 长期机密数据（病历、身份档案、源代码仓库） | "harvest now, decrypt later"：今天被录走的流量，十年后被解密 |
| 高 | 代码签名与固件签名 | 签名要能被验证十年以上，算法寿命必须匹配 |
| 中 | TLS 服务端证书与内部 mTLS | 依赖 CA/HSM 生态就绪，通常跟随厂商节奏 |
| 低 | 短寿命会话与临时凭据 | 数据寿命短，风险窗口小 |

## 六、对不同角色的实际影响

- **开发者**：把"按 digest 固定依赖"变成默认习惯——`ghcr.io/org/app@sha256:...` 而不是 `:latest`；然后在 CI 里验证 provenance，而不是只在发布页贴一个"已签名"徽章。
- **平台/运维**：准备好"元数据变大"这件事。证书链变长会影响握手字节数、HTTP/2 头部预算、以及某些设备的 MTU 行为；日志与审计存储也会因为 attestation 膨胀。
- **安全团队**：把 SLSA 等级、签名验证、SBOM 三者分开管理。它们解决不同问题：SLSA 管"来源"，签名管"完整性"，SBOM 管"里面有什么"。
- **产品团队**：passkey 上线前先把账户恢复流程 redesign 一遍，否则等于把攻击面从登录页搬到找回页。

## 七、常见坑与局限

- **把 SLSA L3 当成"安全认证"**。它只保证构建链路可信，不保证代码没有漏洞，也不保证依赖没有被投毒。
- **只验证"签名存在"，不验证"签名身份"**。keyless 场景下必须校验 `--certificate-identity` 与 `--certificate-oidc-issuer`，否则任何人都能签一个"合法"的 attestation。
- **忽略透明日志的隐私面**。Rekor 是公开的，签名里如果带了内部仓库路径、邮箱，就会长期可见。
- **passkey 只做注册不做恢复**。恢复流程被攻破的案例，比 passkey 本身被攻破多得多。
- **PQC 迁移按"系统重要性"排序**。正确的排序依据是"数据需要保密多久"，不是"这个系统多重要"。
- **以为混合模式可以省掉算法选择**。混合降低风险，但两套算法都要正确实现；实现 bug 仍是主要失陷原因（这一点在 PQC 上尤其危险，因为新代码没有经过二十年攻击检验）。
- **本文没有覆盖的具体面**：SBOM 格式之争（SPDX/CycloneDX）、SLSA 与 ISO/IEC 42001 等合规框架的映射、以及各国 PQC 法规细节，均未展开。

## 来源

访问日期均为 2026-11-15。

- SLSA v1.2 规范首页（Status: Approved） — https://slsa.dev/spec/v1.2/
- SLSA v1.2 What's new（新增 Source Track、威胁模型更新） — https://slsa.dev/spec/v1.2/whats-new
- SLSA v1.1 规范 — https://slsa.dev/spec/v1.1/
- Sigstore 博客《Post-Quantum Cryptography for Sigstore》（2026-10-02） — https://blog.sigstore.dev/
- Sigstore 官方站 — https://www.sigstore.dev/
- WebAuthn Level 3（W3C Recommendation，2026-08-25） — https://www.w3.org/TR/webauthn-3/
- NIST FIPS 203 最终版 — https://csrc.nist.gov/pubs/fips/203/final
- NIST FIPS 204 最终版 — https://csrc.nist.gov/pubs/fips/204/final
- NIST FIPS 205 最终版 — https://csrc.nist.gov/pubs/fips/205/final
- NIST IR 8547《Transition to Post-Quantum Cryptography Standards》 — https://csrc.nist.gov/pubs/ir/8547/ipd
- NIST 后量子密码项目主页 — https://csrc.nist.gov/projects/post-quantum-cryptography
- IEEE 802.11 工作组（TGbt: Post-Quantum Cryptography） — https://www.ieee802.org/11/
- FIDO Alliance passkeys 页面（仅确认页面存在，正文被 CSS 层挡住） — https://fidoalliance.org/passkeys/

## 不确定项

- **FIPS 203/204/205 的正式发布日期**：CSRC 页面被 JS 壳包裹，只读到标题与"final"状态，未读到日期字段；文中按业界通行的"2024 年 8 月"表述，未精确到日。
- **Sigstore 2026-10-02 文章的完整技术方案**：只读到博客首页的摘要段（含 2029/2030 时间线与"立即支持 PQC"的表态），未读到具体的算法选型与 Rekor 迁移细节；文中对"重签/双签/时间戳保全"的讨论属于问题框架，不是官方方案。
- **PQC 参数表的精确性**：公钥/密文/签名大小为公开参数值，按 FIPS 文本记忆整理，**未逐项对照标准原文**；不同参数集（如 ML-KEM-512 vs 1024）差异很大，工程选型请以标准原文为准。
- **SLSA v1.2 的批准日期**：站点只显示 `Status: Approved` 与 RC1/RC2 的变更记录，未显示正式发布日期，故正文只写"2026 年 11 月"。
- **passkey 的实际采用率**：FIDO 官方页面正文不可读，未取到任何统计数字，本文因此不给渗透率，只讨论机制与风险。
- **CNSA 2.0 等政府时间表**：文中只引用 Sigstore 官方博客转述的"2029/2030"，未直接核对 NSA/CISA 原始文件。
- **IEEE TGbt 的工作范围**：仅从工作组首页确认该任务组存在及名称，未读其项目授权文档（PAR），因此不描述它具体覆盖哪些帧类型。
