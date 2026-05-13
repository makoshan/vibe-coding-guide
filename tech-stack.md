# Tech Stack

## 选择原则

这个项目面向个人自用和 Demo 验证，所以技术栈要简单、稳定、容易部署。

第一版不要追求复杂架构。目标是先把钱包流程跑通：

- 页面能打开。
- Passkey 能创建。
- 地址能生成。
- 余额能显示。
- 测试网能转账。

## 推荐技术栈

| 类型 | 推荐 | 为什么适合 |
| --- | --- | --- |
| 前端框架 | React + Vite | 启动简单，适合做 PC 网页 Demo |
| 开发语言 | TypeScript | 钱包地址、交易参数不容易写错 |
| 包管理 | pnpm | 适合以后复用 UI 组件库 |
| UI 样式 | Tailwind CSS + CSS 变量 | 容易做出干净统一的钱包界面 |
| UI 参考 | `/Users/thursday/Downloads/token-ui-main` | 已经有钱包风格的设计系统 |
| 路由 | React Router | 页面切换简单 |
| 钱包核心 | `@consenlabs/tcx-wasm` | imToken Token Core WASM，适合浏览器钱包 |
| Ethereum / BSC 辅助 | viem | 适合查询余额、构建 EVM 交易 |
| Solana | `@solana/web3.js` | Solana 官方常用前端库 |
| Solana Token 预留 | `@solana/spl-token` | 以后支持 SPL Token 时需要 |
| TRON 辅助 | TronWeb 或 TronGrid API | 用于 TRON 查询、交易构建和广播 |
| Bitcoin 数据 | Esplora 或 mempool API | 查询 UTXO、费率和广播测试网交易 |
| 本地存储 | IndexedDB | 保存加密钱包数据 |
| 浏览器加密 | WebCrypto | 用于加密备份数据 |
| 数据请求 | TanStack Query | 管理余额、交易记录和网络请求 |
| 表单校验 | React Hook Form + Zod | 校验转账地址、金额和表单输入 |
| 测试 | Vitest + Playwright | 检查功能和浏览器流程 |
| 部署 | Vercel 或 Cloudflare Pages | 免费、简单、自动 HTTPS |

## 钱包核心怎么分工

### Token Core WASM

`@consenlabs/tcx-wasm` 用来处理：

- Ethereum 地址。
- BSC 地址。
- Bitcoin 地址。
- TRON 地址。
- 交易签名。
- 消息签名。

### Solana

当前 Token Core WASM 参考文档里没有明确列出 Solana。

所以 Solana 第一版单独使用：

- `@solana/web3.js`
- ed25519 派生和签名相关库

这样更清楚，也更容易排查问题。

## Passkey 怎么用

Passkey 用来创建和解锁钱包。

简单理解：

- Passkey 不是链上私钥。
- Passkey 像一把本地钥匙。
- 它用来解锁钱包里的加密数据。
- 真正签交易的仍然是每条链自己的私钥。

第一版推荐使用“无助记词体验”：

- 平时用户不用记助记词。
- 钱包用 Passkey 解锁。
- 用户可以主动导出助记词做备份。

## 是否需要后端

第一版可以先不做复杂后端。

可以先做：

- 前端网页。
- 本地加密存储。
- 测试网 RPC 请求。

如果后面需要邮箱登录，可以加一个轻量后端。

后端只能保存：

- 邮箱账号。
- 设备信息。
- 用户偏好。
- 加密后的备份数据。

后端绝对不能保存：

- 明文助记词。
- 私钥。
- Passkey 产生的解锁密钥。
- root seed 明文。

## 不推荐第一版使用

第一版不推荐：

- Next.js 全栈复杂架构。
- Redux 保存钱包状态。
- 自己手写加密算法。
- 把私钥或助记词放进浏览器日志。
- 把正式 RPC API Key 写进前端环境变量。
- 一开始就做主网转账。

