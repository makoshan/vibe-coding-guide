<!-- markdownlint-disable MD013 -->

# Passkey 多链钱包方案

## 目标

基于 imToken TokenCore WASM 构建一个浏览器钱包，让用户通过 Passkey 创建和解锁钱包，并管理以下主网地址：

- Ethereum
- BSC
- Bitcoin
- Solana
- TRON

用户可以完成：

- 创建钱包
- 查看五条主网地址
- 复制地址并转入资产
- 查看余额和交易状态
- 从钱包转出资产

## 已确认需求

当前版本定位为个人自用的 PC 端测试网钱包，不面向真实用户资产。

产品范围：

- 目标用户：自己使用，优先做可运行、可验证的完整 Demo。
- 网络环境：只支持测试网，暂不支持主网真实资产。
- 密钥体验：无助记词体验，创建和日常解锁通过 Passkey 完成。
- 备份策略：允许用户导出助记词作为丢失 Passkey 后的备份方案。
- 转账能力：支持测试网广播。
- 链优先级：先跑通 Ethereum Sepolia，再扩展到 BSC Testnet、Bitcoin Testnet/Signet、Solana Devnet、TRON Nile/Shasta。
- 资产范围：第一版只支持原生币，不支持 ERC20、BEP20、SPL Token、TRC20。
- 设备范围：PC Web 版本优先。
- 登录方式：可以支持邮箱登录，但邮箱账号不得接触或恢复钱包私钥。
- 验收目标：实现完整闭环，包括创建钱包、登录/解锁、查看五条链地址、复制收款地址、查看余额、测试网转账、广播后查看 tx hash 和交易状态。

邮箱登录只用于产品账号、设备管理、偏好同步或 encrypted envelope 同步。钱包签名仍必须由本地 Passkey 解锁完成，后端不得保存明文助记词、私钥、PRF key 或 root seed。

## 重要结论

`tcx-wasm` 适合作为 Ethereum、BSC、Bitcoin 和 TRON 的核心签名与派生层。

但截至当前参考 README，`tcx-wasm` 的 supported chains 列表没有列出 Solana。因此 Solana 使用前端专用库实现：

- 交易、RPC、余额查询和广播使用 `@solana/web3.js`。
- 私钥派生和签名使用经过审计的 ed25519 / SLIP-0010 / BIP39 相关库，具体库需要在实现前锁定版本和来源。
- 产品层保持一个钱包入口，但底层允许多签名引擎：TokenCore Engine + Solana Engine。
- Passkey 只作为解锁和加密因子，不直接充当 Solana 私钥。

不要把“Passkey 本身”等同于钱包私钥。Passkey 更适合做解锁因子或密钥加密材料，钱包仍应有明确的 keystore、派生路径、恢复策略和风险提示。

当前确认采用“无助记词体验”，但允许导出助记词备份。这意味着第一版更适合选择 TokenCore-compatible 模式：用户日常不需要看到或输入助记词，但在备份流程中可以主动导出助记词。

可以设计成“不向用户展示、不要求用户备份助记词”的钱包，也可以进一步设计成“严格不生成 mnemonic”的钱包。但两者不是一回事：

- TokenCore-compatible 模式：继续使用 TokenCore keystore，mnemonic 存在于加密 keystore 内，用户不接触明文助记词。
- Strict no-mnemonic 模式：不生成 mnemonic，只保存或派生 root seed，再由多链 engine 直接派生链私钥；这种模式需要自建或扩展签名层，不能简单等同于 `create_keystore`。

无论哪种模式，仍然会存在链上签名私钥或可派生这些私钥的根密钥。当前版本技术口径选择 TokenCore-compatible，产品口径使用“无助记词体验”。

## 推荐 MVP

第一版不要直接做主网真实资产转出。推荐先做测试网 MVP：

- Ethereum Sepolia
- BSC Testnet
- Bitcoin Testnet 或 Signet
- Solana Devnet
- TRON Nile 或 Shasta

MVP 功能范围：

1. 初始化 `@consenlabs/tcx-wasm`。
2. 检测浏览器是否支持 Passkey / WebAuthn PRF。
3. 创建 Passkey-backed keystore 或 encrypted root seed envelope。
4. 派生并展示五条测试网地址。
5. 复制收款地址。
6. 查询测试网余额。
7. 签名测试消息。
8. 构建转账 Review 页面，但默认不广播。

主网转出应放到安全评审后的第二阶段。

### MVP 分期

为了避免一次性做五条链导致排查困难，MVP 按链逐步扩展。

第 1 步：Ethereum Sepolia 完整闭环。

- 创建 Passkey 钱包。
- 解锁钱包。
- 显示 Ethereum Sepolia 地址。
- 复制地址和展示二维码。
- 查询 Sepolia ETH 余额。
- 发起 Sepolia ETH 转账。
- Passkey 解锁签名。
- 二次确认后广播。
- 展示 tx hash、explorer 链接和交易状态。

第 2 步：扩展五链地址和收款。

- BSC Testnet 地址、二维码、余额。
- Bitcoin Testnet 或 Signet 地址、二维码、余额。
- Solana Devnet 地址、二维码、余额。
- TRON Nile 或 Shasta 地址、二维码、余额。

第 3 步：扩展五链原生币测试网转账。

- BSC Testnet BNB 转账。
- Bitcoin Testnet BTC 转账。
- Solana Devnet SOL 转账。
- TRON Testnet TRX 转账。

第 4 步：账号与备份。

- 邮箱登录。
- 设备状态展示。
- 导出助记词备份。
- 导入助记词恢复测试钱包。
- 添加或替换 Passkey。

### MVP 验收标准

完整验收时，用户应该可以在 PC 浏览器完成：

1. 使用邮箱登录或进入本地钱包会话。
2. 点击 `Create Passkey` 创建钱包。
3. 看到 Ethereum、BSC、Bitcoin、Solana、TRON 五条测试网地址。
4. 进入 Deposit Funds，选择任意网络，看到对应二维码、地址和风险提示。
5. 复制任意网络地址。
6. 查看五条链的原生币测试网余额。
7. 在 Ethereum Sepolia 完成一笔测试网转账广播。
8. 看到 tx hash、explorer 链接和交易状态。
9. 导出助记词备份，并看到足够明确的安全提示。
10. 退出后重新通过 Passkey 解锁钱包。

验收时不得出现：

- 主网广播入口。
- ERC20、BEP20、SPL Token、TRC20 转账入口。
- 控制台、日志、错误上报或页面中出现助记词、私钥、PRF key、root seed。
- 未经 Review 直接签名或广播。

## 产品信息架构

设计参考采用干净、可信、低噪音的钱包后台风格：左侧固定导航，顶部显示当前页面标题，主体使用大留白和少量关键卡片承载操作。视觉基调以白色、浅灰、深 navy 字体和高饱和蓝色主按钮为主，避免营销页式大图、复杂渐变和装饰性组件。

本项目 UI 规范参考本地设计系统：`/Users/thursday/Downloads/token-ui-main`。

### 设计系统约束

实现时优先复用 `token-ui-main` 的设计语言和组件思路，而不是重新发明一套视觉系统。

技术参考：

- `DESIGN.md`：设计原则、颜色、排版、组件、布局、动效和响应式规则。
- `packages/ui/src/styles/globals.css`：CSS 变量、语义 token、亮暗色主题、阴影和密度。
- `packages/ui/src/components/`：Button、Card、NavItem、AssetRow、ActionBar、Badge、StepCard、IconBox 等组件规范。
- `apps/web/src/features/wallet/wallet-dashboard.tsx`：钱包 Dashboard 示例组合方式。

视觉原则：

- 使用 token 驱动的设计，不在业务代码里散落 raw hex、任意阴影和临时字号。
- 主色使用 Primary Blue：`#007fff`，用于 CTA、active nav、链接、进度和关键状态。
- 文字使用深 navy：`#111d4a`，辅助文字使用 muted gray。
- 字体使用 Inter + Noto Sans SC，中文场景优先保证 Noto Sans SC 回退。
- 页面背景保持白色或极浅灰，卡片使用白色或 `--card`，强调金融产品的可信和安静。
- 主按钮使用 pill 形态，默认高度 40px，大按钮 48px，hero 按钮 56px。
- 卡片使用 12px、18px 或 20px 圆角，不使用 pill 形卡片。
- 组件间距遵循 4pt grid，常用节奏为 8px、12px、16px、24px、32px、48px。
- 阴影只使用语义 shadow token，避免多层重阴影。
- 支持 dark mode，但 MVP 可先完成 light mode，再按 token 系统补齐 dark mode。

组件优先级：

- 导航项使用 NavItem 语义：active 为蓝底白字并带蓝色 active shadow。
- 主要操作使用 Button / ActionButton，不自定义一次性按钮。
- 收款地址、资产行和交易记录使用 AssetRow 或等价行组件结构。
- 资产总览、收款面板、Review 面板使用 Card 或 SectionPanel。
- Passkey 创建页的指纹/盾牌图形使用 IconBox 风格。
- 钱包创建步骤、安全检查和恢复状态使用 StepCard 或 ChecklistCard。
- 状态提示使用 Badge、Alert、Toast，不用普通正文硬塞错误或成功状态。

### 导航结构

左侧导航固定展示：

- Wallet 品牌区：钱包图标、产品名和简短副标题。
- Dashboard：资产概览和快捷操作。
- Portfolio：资产总览、资产分布和最近活动。
- Trading Agent：后续交易代理或策略入口，MVP 可置灰或隐藏。
- Settings：Passkey、网络、备份、RPC 和安全设置。
- Help：Passkey、转账、链网络和恢复说明。
- Log Out：退出当前本地会话，不删除钱包。

顶部栏展示当前页面标题、副标题和少量图标操作：

- 下载或导出记录。
- 筛选资产或交易。
- 设置入口。
- 当前账户或设备状态。

### 首次创建页

首次打开且没有钱包时，显示独立居中的 Passkey 创建页。

页面内容：

- 指纹或 Passkey 图形。
- 主标题：`Your keys, simplified.`
- 副标题：`No seed phrase to lose. Access your assets with biometrics.`
- 主按钮：`Create Passkey`
- 次级链接：`What is a passkey?`
- 安全标签：`End-to-end encrypted storage`

交互要求：

- 点击 `Create Passkey` 前先检查 HTTPS、WebAuthn、PRF 支持和 `rpId`。
- 不支持 PRF 时展示降级说明，不进入创建流程。
- 创建成功后进入 Dashboard，并显示五条测试网地址已生成。
- 创建失败时解释原因，但不显示任何 secret、PRF key、root seed 或内部堆栈。

### Dashboard

Dashboard 是登录后的默认首页，目标是让用户快速知道钱包状态和下一步能做什么。

页面内容：

- 总资产估值。
- 五条网络的地址状态：Ethereum、BSC、Bitcoin、Solana、TRON。
- 快捷按钮：Receive、Send、Portfolio、Settings。
- 安全状态：Passkey 已启用、是否已添加第二个 Passkey、是否已备份 encrypted root seed envelope。
- 最近交易概览。

MVP 可以先展示空资产状态，但必须保留真实布局，避免后续接入余额后大幅改版。

### Portfolio

Portfolio 页面参考附件中的资产总览页。

页面内容：

- Total Portfolio Value。
- 时间范围切换：24h、7d、30d、1y。
- 资产价值折线图，空状态显示 `$0.00` 和当前时间点。
- Asset Allocation 分布图，空状态显示 `Total Assets 0`。
- Recent Activity 列表，支持搜索交易。
- 顶部工具图标：导出、筛选、设置、账户。

交互要求：

- 资产估值加载失败时，不影响地址查看和转账入口。
- 搜索交易只过滤本地已加载记录，不触碰私钥。
- 隐藏资产金额时，图表和列表同步隐藏具体金额。

### Deposit Funds

Deposit Funds 页面参考附件中的收款页。

页面内容：

- 页面标题：`Deposit Funds`
- 副标题：`Receive crypto to your wallet`
- 网络选择器：Ethereum、BSC、Bitcoin、Solana、TRON。
- 当前网络图标、名称和下拉箭头。
- 收款二维码。
- 地址标签，例如 `Your Ethereum Address`。
- 地址展示框和复制按钮。
- 重要提醒卡片，提示只能向当前网络地址发送对应资产。
- EVM 额外提示：Ethereum 与 BSC 可显示相同地址，但必须选择正确网络。

各网络提醒：

- Ethereum：只向 Ethereum 网络地址发送 ETH/ERC20。
- BSC：只向 BSC 网络地址发送 BNB/BEP20。
- Bitcoin：只发送 BTC，注意地址类型和找零。
- Solana：只发送 SOL/SPL 资产。
- TRON：只发送 TRX/TRC20 资产，注意账户激活、带宽和能量。

交互要求：

- 切换网络时二维码、地址、提示文案和复制按钮同步更新。
- 复制成功后显示短暂状态反馈。
- 地址过长时中间省略，复制仍复制完整地址。
- 页面不显示私钥、助记词、root seed 或调试信息。

### Send Funds

Send Funds 是最高风险页面，必须比 Deposit Funds 更克制。

页面步骤：

1. 选择网络和资产。
2. 输入收款地址和金额。
3. 展示手续费、余额和预计到账信息。
4. 展示交易 Review。
5. Passkey 解锁签名。
6. 二次确认后广播。

Review 页面必须用自然语言解释交易结果，并用结构化字段展示原始关键信息。任何合约调用、TRC20 转账、SPL 转账、token approval 或 Bitcoin PSBT 都不能只显示 raw data。

## 技术栈推荐

第一版推荐做成纯前端优先的钱包应用：浏览器负责 Passkey、加密存储、地址派生、签名和交易 Review；后端只在需要隐藏 RPC key、聚合链上数据或做风控策略时引入。

### 总体推荐

| 层级 | 推荐 | 说明 |
| --- | --- | --- |
| 包管理 | `pnpm` | 与 `/Users/thursday/Downloads/token-ui-main` 保持一致，适合 monorepo |
| 语言 | TypeScript | 钱包、交易和链适配器必须强类型 |
| 前端框架 | React + Vite | MVP 启动快，适合 WASM、Passkey 和静态部署 |
| 路由 | React Router | 与 `token-ui-main` 示例一致 |
| 样式 | Tailwind CSS + CSS Variables | 复用 `token-ui-main` 的 token 驱动设计 |
| UI 组件 | 复用或迁移 `packages/ui` | Button、Card、NavItem、AssetRow、Badge、StepCard 等 |
| 图标 | `lucide-react` | 钱包、复制、设置、筛选、下载、帮助等通用图标 |
| 状态管理 | Zustand | 只放 UI/session 状态，不放私钥、PRF key、root seed |
| 远程数据 | TanStack Query | 管理余额、价格、交易记录、RPC 请求缓存 |
| 表单校验 | React Hook Form + Zod | Send Funds、Settings、RPC 配置和地址校验 |
| 本地存储 | IndexedDB | 保存 encrypted keystore / encrypted root seed envelope |
| 加密 API | WebCrypto | AES-GCM、HKDF、随机数生成 |
| 二维码 | `qrcode.react` 或等价库 | Deposit Funds 收款二维码 |
| 图表 | Recharts 或 UI kit chart 组件 | Portfolio 资产曲线和资产分布 |
| 测试 | Vitest + Testing Library + Playwright | 单元、组件和关键浏览器流程 |
| Mock | MSW | 模拟 RPC、余额、交易记录和广播返回 |
| 代码质量 | Biome + TypeScript strict | 与 `token-ui-main` 风格保持一致 |
| 部署 | Vercel 或 Cloudflare Pages | 静态前端部署，必须 HTTPS |

### 钱包与多链 SDK

| 链 | 推荐栈 | 用途 |
| --- | --- | --- |
| Ethereum | `@consenlabs/tcx-wasm` + `viem` | TokenCore 负责派生/签名，`viem` 负责 RPC、ABI、金额格式化和交易构建辅助 |
| BSC | `@consenlabs/tcx-wasm` + `viem` | 与 Ethereum 共用 EVM adapter，但 chainId、RPC、explorer、gas 策略独立 |
| Bitcoin | `@consenlabs/tcx-wasm` + Esplora/mempool API | TokenCore 负责 BTC 地址/PSBT 签名，API 负责 UTXO、费率、广播和交易查询 |
| Solana | `@solana/web3.js` + `@solana/spl-token` | Solana 地址、余额、SPL token、交易构建和广播 |
| TRON | `@consenlabs/tcx-wasm` + TronWeb 或 TronGrid API | TokenCore 负责 TRON 派生/签名，TronWeb/API 负责交易构建、资源估算和广播 |

补充原则：

- EVM 链使用同一个 `evm-adapter` 抽象，但 Ethereum 和 BSC 必须有独立 chain config。
- Solana 独立 `solana-adapter`，不要把 Solana 硬塞进 EVM 模型。
- Bitcoin 独立 `bitcoin-adapter`，重点处理 UTXO、找零、费率和 PSBT。
- TRON 独立 `tron-adapter`，重点处理带宽、能量、账户激活、TRX/TRC20 差异。

### Passkey 与密钥层

推荐模块：

- `passkey-service`：封装 WebAuthn create/get、PRF capability check、credentialId 管理。
- `crypto-service`：封装 WebCrypto HKDF、AES-GCM、随机数、密钥派生上下文。
- `key-envelope-service`：管理 encrypted keystore 或 encrypted root seed envelope。
- `wallet-session`：只保存短生命周期解锁状态，不持久化任何秘密。

MVP 推荐密钥模式：

- 如果优先复用 TokenCore：选择 TokenCore-compatible 模式，使用 Passkey-backed keystore。
- 如果优先“严格不生成 mnemonic”：选择 Strict no-mnemonic 模式，使用 encrypted root seed envelope，但需要自建多链签名派生层。

更务实的第一版建议：先用 TokenCore-compatible 模式跑通 Ethereum、BSC、Bitcoin、TRON，再用独立 Solana seed envelope 接入 Solana。等整体流程稳定后，再评估 Strict no-mnemonic。

### 数据与后端

MVP 可以无后端：

- 静态前端部署在 HTTPS 域名。
- encrypted envelope 保存在 IndexedDB。
- RPC endpoint 使用公开测试网或用户配置。
- 价格、余额、交易记录从链上 API 读取。

需要后端时，推荐做轻量 BFF：

- Cloudflare Workers / Hono：RPC 代理、API key 隐藏、请求限流。
- PostgreSQL：保存非敏感用户偏好、交易索引缓存和风控配置。
- Redis / Upstash：RPC cache、rate limit、任务状态。

后端不得接触：

- mnemonic
- private key
- PRF key
- root seed 明文
- signed transaction 以外的可恢复秘密

### 推荐目录结构

```text
apps/web/
  src/
    app/
    pages/
      create-passkey/
      dashboard/
      portfolio/
      deposit/
      send/
      settings/
    features/
      wallet/
      passkey/
      portfolio/
      transactions/
    services/
      passkey-service.ts
      crypto-service.ts
      tokencore-service.ts
      key-envelope-service.ts
    chains/
      evm/
      bitcoin/
      solana/
      tron/
    policy/
      transaction-review.ts
      risk-rules.ts
    storage/
      indexed-db.ts
    test/
packages/ui/
  src/
    components/
    styles/
```

### 不推荐

- 不推荐一开始上 Next.js 全栈，除非已经确定需要服务端渲染或账号系统。
- 不推荐 Redux 管理钱包秘密，私钥和 root seed 不应进入全局状态树。
- 不推荐把 RPC key 写进 `VITE_*` 环境变量，浏览器 bundle 会暴露这些值。
- 不推荐一开始接主网真实资产广播，先完成测试网 Review、签名和二次确认。
- 不推荐混用多套 UI 框架，优先沿用 `token-ui-main` 的 token 和组件。

## 产品流程

### 创建钱包

1. 用户点击“创建钱包”。
2. 前端调用 WebAuthn 创建 passkey credential。
3. 浏览器通过 WebAuthn PRF 产出 32 字节 hex key。
4. 按密钥模式创建 Passkey envelope：
   - TokenCore-compatible：使用 `create_keystore` 创建加密 keystore。
   - Strict no-mnemonic：生成 root seed，并用 PRF key 加密成 root seed envelope。
5. 本地保存加密后的 keystore JSON 或 encrypted root seed envelope、credentialId、rpId 和账户元数据。
6. 派生各链地址并展示。

### 解锁钱包

1. 用户点击“解锁”。
2. 前端调用 WebAuthn get credential。
3. 重新取得 32 字节 PRF key。
4. 使用该 key 解锁 keystore 或 root seed envelope。
5. 派生地址或签名交易。
6. 操作结束后清理内存中的敏感材料。

### 查看地址

地址页按链展示：

| 链 | TokenCore chain | 推荐路径 | 说明 |
| --- | --- | --- | --- |
| Ethereum | `ETHEREUM` | `m/44'/60'/0'/0/0` | chainId 为 `1` |
| BSC | `ETHEREUM` | `m/44'/60'/0'/0/0` | EVM 链，chainId 为 `56` |
| Bitcoin | `BITCOIN` | `m/84'/0'/0'/0/0` 或 `m/86'/0'/0'/0/0` | Native SegWit 或 Taproot |
| Solana | Solana Engine | `m/44'/501'/0'/0'` | `@solana/web3.js` + ed25519 派生 |
| TRON | `TRON` | `m/44'/195'/0'/0/0` | secp256k1，地址以 `T` 开头 |

BSC 可复用 Ethereum 地址和 secp256k1 曲线，但必须在交易、余额、RPC 和 explorer 层区分 chainId。

TRON 使用独立地址格式，不与 EVM 地址共用展示。TRON 转账还需要考虑带宽、能量、账户激活和 TRC20 合约调用。

### 转入资产

转入只需要展示地址和二维码，但必须提示：

- Ethereum 和 BSC 地址格式相同，但链不同。
- 不要把 BTC 转到 EVM 地址。
- 不要把 Solana 资产转到 EVM 地址。
- 不要把 TRC20 资产转到 Ethereum 或 BSC 地址。
- TRON 地址通常以 `T` 开头，第一次接收前可能涉及账户激活。
- 第一次转入建议小额测试。

### 转出资产

转出流程必须拆成五步：

1. 用户填写收款地址、金额、资产和网络。
2. 前端校验地址格式、余额、手续费和最小转账额。
3. 构建交易并展示人话摘要。
4. 用户通过 Passkey 解锁并签名。
5. 用户再次确认后广播。

广播前 Review 页面至少展示：

- 网络和 chainId
- 发送地址
- 接收地址
- 转账金额
- 资产符号和精度
- 手续费估算
- nonce 或 UTXO 输入
- 合约 data 或 token approval 风险
- 原始交易摘要

## 技术架构

```text
Web UI
  |
  |-- Wallet Controller
  |     |-- Passkey Service
  |     |-- Keystore Repository
  |     |-- Account Derivation Service
  |     |-- Transaction Review Service
  |
  |-- Chain Adapters
        |-- Ethereum Adapter  -> TokenCore WASM + Ethereum RPC
        |-- BSC Adapter       -> TokenCore WASM + BSC RPC
        |-- Bitcoin Adapter   -> TokenCore WASM + BTC API
        |-- Solana Adapter    -> Solana Engine + @solana/web3.js
        |-- TRON Adapter      -> TokenCore WASM + TRON RPC/API
```

### 前端模块

- `passkey-service`：封装 WebAuthn create/get、PRF 检测、credential 管理。
- `tokencore-service`：初始化 WASM，封装 `create_keystore`、`derive_accounts`、`sign_tx`、`sign_message`、`sign_psbt`。
- `solana-service`：封装 Solana 地址派生、余额查询、交易构建、ed25519 签名和广播。
- `tron-service`：封装 TRON 地址派生、TRX/TRC20 余额、交易构建、资源估算、签名和广播。
- `keystore-repository`：保存加密 keystore，不保存明文助记词、私钥或 PRF key。
- `chain-adapters`：每条链独立封装地址校验、余额、构建交易、签名和广播。
- `transaction-review`：把交易转成人能读懂的摘要与风险提示。
- `policy-engine`：阻止高风险交易，例如错误链、可疑 approval、异常手续费。

### 数据存储

浏览器本地只保存：

- encrypted keystore JSON
- encrypted root seed envelope
- credentialId
- rpId
- walletId
- address index
- chain account metadata
- Solana encrypted seed envelope 或 Solana encrypted key material

不得保存：

- 明文 mnemonic
- 明文 private key
- 明文 PRF key
- 正式 RPC API key

### Solana 密钥方案

Solana 有两种可选路线。

路线 A：独立 Solana seed envelope，推荐用于 MVP。

- 创建钱包时生成独立的 32 字节 Solana seed。
- 使用 Passkey PRF key 通过 WebCrypto AES-GCM 加密 Solana seed。
- 本地只保存密文、iv、credentialId、rpId 和派生元数据。
- 解锁时通过 Passkey 取回 PRF key，解密 seed，在内存中生成 Solana Keypair。
- 操作结束后尽量清理 `Uint8Array`，不要打印 seed 或 secretKey。

优点是不用从 TokenCore keystore 导出助记词，秘密边界更小。缺点是 EVM/BTC 与 Solana 不是同一个助记词派生出来的账户，但用户体验仍然可以是“一个 Passkey 钱包”。

路线 B：统一助记词派生 Solana，适合安全评审后的版本。

- 使用 TokenCore keystore 保存统一 mnemonic。
- 解锁后仅在内存中导出 mnemonic。
- 使用 BIP39 + SLIP-0010/ed25519 派生 Solana 路径 `m/44'/501'/0'/0'`。
- 用 `@solana/web3.js` 构造和签名交易。
- 完成后立即清理可变字节数组，并避免把 mnemonic 放进日志、状态管理、错误上报和浏览器持久存储。

优点是多链账户来自同一个恢复材料。缺点是浏览器 JS 会短暂接触 mnemonic，安全要求更高。

第一版建议选路线 A。等恢复、导出、审计、监控和错误上报脱敏都做好后，再评估是否切换到路线 B。

## Passkey 与恢复策略

必须在产品里说清楚：

- Passkey 丢失后是否还能恢复钱包。
- 是否允许导出助记词。
- 是否支持多设备 passkey。
- 是否支持添加第二个 passkey。
- 浏览器、系统账户或 iCloud/Google Password Manager 的同步行为会影响可恢复性。

生产级建议：

- 创建钱包后引导用户添加第二个 Passkey。
- 提供离线备份方案，但默认隐藏高风险导出入口。
- 导出助记词需要额外确认、遮挡展示和禁止截图提示。
- 恢复流程需要单独安全设计，不能临时拼接。

## 无助记词模式

如果目标是“不保存助记词”，可以采用 Passkey PRF root seed 模式。

注意：这个模式和 TokenCore `create_keystore` 不是同一个模型。`create_keystore` 的优势是复用 TokenCore 的 HD keystore、派生和签名能力；Strict no-mnemonic 的优势是不生成 mnemonic，但需要自己实现或封装 EVM、Bitcoin、Solana、TRON 的私钥派生和签名。

### 核心思路

1. 用户创建 Passkey credential。
2. 浏览器通过 WebAuthn PRF extension 取得稳定的 32 字节 secret。
3. 使用 HKDF 从该 secret 派生钱包 root seed。
4. 再从 root seed 派生各链私钥：
   - Ethereum / BSC：secp256k1，路径可兼容 `m/44'/60'/0'/0/0` 的语义。
   - Bitcoin：secp256k1，根据 P2WPKH 或 Taproot 选择账户路径。
   - Solana：ed25519，使用 `m/44'/501'/0'/0'` 语义。
   - TRON：secp256k1，使用 `m/44'/195'/0'/0/0` 语义。
5. 本地只保存 wallet metadata、credentialId、rpId、地址索引和必要密文，不保存 mnemonic。

### 两种实现方式

方式 A：PRF 直接派生 root seed。

- 优点：本地不需要保存助记词，也不需要保存加密 seed。
- 缺点：完全依赖 Passkey credential、rpId、浏览器兼容性和 passkey 同步能力。
- 风险：credential 丢失、域名变更、PRF 行为变化或不同设备不可用，都可能导致无法恢复钱包。

方式 B：随机生成 root seed，再用 PRF key 加密保存。

- 优点：可以支持添加第二个 Passkey，因为同一个 root seed 可以被多个 Passkey 分别加密。
- 缺点：本地或云端需要保存 encrypted root seed envelope。
- 风险：如果 encrypted envelope 丢失，钱包仍无法恢复。

生产级更推荐方式 B。它不是保存助记词，而是保存“被 Passkey 加密的根密钥密文”，更利于多设备、换 Passkey 和恢复策略。

### 必须避免的误区

- Passkey 的 P-256 credential 不能直接签 Ethereum、Bitcoin、Solana 或 TRON 交易。
- 不要把 WebAuthn assertion 当作链上签名。
- 不要把 PRF key、root seed 或链私钥放进 React state、localStorage、日志、错误上报或 URL。
- 不要承诺“永不丢失”，除非已经设计好多 Passkey、备份和恢复。
- 不要在未审计前承载真实主网资产。

## 实施步骤

### 第 1 阶段：只读测试网钱包

1. 搭建 Vite/React 或 Next.js 前端。
2. 安装并初始化 `@consenlabs/tcx-wasm`。
3. 做 Passkey 支持检测页。
4. 创建测试 keystore 或 encrypted root seed envelope。
5. 派生并展示 Ethereum Sepolia 地址。
6. 扩展到 BSC Testnet 和 Bitcoin Testnet。
7. 使用 `@solana/web3.js` 接入 Solana Devnet 地址展示。
8. 接入 TRON Nile 或 Shasta 地址展示。

验收标准：

- 浏览器能初始化 WASM。
- 创建钱包后刷新页面仍能解锁。
- 地址展示稳定，复制按钮可用。
- 控制台不打印任何秘密。

### 第 2 阶段：交易构建与签名前 Review

1. 接入测试网 RPC。
2. 查询余额与手续费。
3. 构建 EVM 原生币转账。
4. 构建 Bitcoin PSBT。
5. 构建 TRON TRX 转账和 TRC20 转账模板。
6. 展示交易 Review 页面。
7. 加入 policy 检查。
8. Passkey 解锁后只签名，不默认广播。

验收标准：

- 用户能在签名前看懂交易。
- 错链、余额不足、异常手续费会被拦截。
- 所有签名动作都有二次确认。

### 第 3 阶段：广播与主网准备

1. 接入广播 API。
2. 加入交易状态跟踪。
3. 加入主网开关和风险确认。
4. 做安全测试和代码审计。
5. 完成恢复流程和用户教育。

验收标准：

- 广播前必须用户确认。
- 广播后展示 tx hash、explorer 链接和状态。
- 主网入口默认关闭，必须显式开启。
- 安全评审完成前不得承载真实用户资产。

## 关键风险

- `tcx-wasm` 当前文档未列出 Solana，不能假设可直接派生和签名。
- Solana 专用签名库必须锁定版本、来源和许可证，并做最小封装，不要散落在 UI 组件里。
- 如果采用统一助记词路线，浏览器 JS 会短暂接触 mnemonic，必须关闭日志、错误上报和状态持久化里的秘密泄露路径。
- WebAuthn PRF 浏览器兼容性不一致，必须做能力检测和降级提示。
- Passkey 同步和设备丢失会影响钱包恢复。
- BSC 与 Ethereum 地址相同，用户容易跨链误转。
- Bitcoin UTXO、找零、手续费估算和 PSBT 比 EVM 转账更复杂。
- TRON 的能量、带宽、账户激活和 TRC20 合约调用需要单独解释，否则用户容易误判手续费和到账状态。
- 主网真实资产需要安全审计、监控、日志脱敏和事故预案。

## 推荐给 AI 的执行提示词

```text
请基于 documents/Templates and Resources/Passkey 多链钱包方案.md 执行。

第一步只做测试网 MVP 的最小切片：
1. 搭建前端项目。
2. 安装并初始化 @consenlabs/tcx-wasm。
3. 显示 WASM 初始化成功或失败。
4. 不创建钱包，不签名，不广播。

完成后告诉我改了哪些文件、如何启动、如何在浏览器验证。
```

## 参考资料

- TokenCore WASM: <https://github.com/consenlabs/token-core-monorepo/tree/tenth-anniversary/token-core/tcx-wasm>
- 本仓库 TokenCore skill: `skills/tokencore-wallet/SKILL.md`
