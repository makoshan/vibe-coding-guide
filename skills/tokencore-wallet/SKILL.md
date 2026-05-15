---
name: tokencore-wallet
description: "imToken TokenCore 钱包开发 skill。用于构建或评审 TokenCore/tcx-wasm Passkey 网页钱包、批量钱包创建、账户派生、交易签名、消息签名、PSBT 签名、WebAuthn PRF keystore 和测试网钱包自动化。"
---

<!-- markdownlint-disable MD013 -->

# TokenCore Wallet Skill

使用这个 skill 来构建、评审或解释基于 imToken TokenCore 和 `tcx-wasm` 的钱包工作流。

## 什么时候使用

当任务涉及以下内容时使用：

- 基于 `@consenlabs/tcx-wasm` 构建浏览器钱包。
- 设计 Passkey 或 WebAuthn PRF 钱包流程。
- 创建、导入、列出或导出钱包 keystore。
- 通过 TokenCore 派生账户或地址。
- 签名消息、交易或 PSBT。
- 分析、模拟、签名或广播 EVM 交易。
- 编写钱包 Agent 工作流、提示词、规则或 Review 清单。

## 边界与安全规则

- 不处理真实用户资产，除非用户明确要求且已经有生产级安全评审。
- 不索要、保存、打印、提交真实助记词、私钥、种子短语或正式 API Key。
- 不自动签名，不自动广播交易。
- 不把模型输出当成安全审计结论。
- 示例、教程和生成代码默认使用测试网。
- 如果缺少目标链、网络、用户动作或安全等级，先询问或默认降级为测试网 Demo。

## 默认工作流

1. 识别钱包场景：浏览器 WASM、Node/WASM 自动化脚本、Passkey 钱包，还是 Agent Skill。
2. 确认网络和资产风险。默认使用 Sepolia 或其它测试网。
3. 先阅读相关 README 或本地代码，再提出实现方案。
4. 输出短计划，并为每一步写清楚验证方式。
5. 每次只实现一个小切片。
6. 签名前展示人能看懂的交易或消息摘要。
7. 广播前要求用户明确确认。
8. 用测试、非敏感脚本输出或浏览器 walkthrough 验证结果。

## TokenCore WASM 快速参考

来源：`tenth-anniversary` 分支的 `token-core/tcx-wasm`。

- 公开包：`@consenlabs/tcx-wasm`。
- 浏览器应用必须先初始化 WASM 模块，再调用导出函数。
- 大多数导出 API 接收 JSON 字符串，并返回 JSON 字符串。
- 常用 API 包括 `create_keystore`、`derive_accounts`、`sign_tx`、`sign_message` 和 `sign_psbt`。
- `create_keystore` 支持基于 `password` 的原生 TokenCore HD keystore。
- `create_keystore` 也支持由 32 字节十六进制 WebAuthn PRF key 加密的 Passkey envelope。
- Passkey envelope 创建时需要 `prfKey`、`userId`、`credentialId` 和 `rpId`。
- 对 Passkey 调用方来说，解锁用的 `key` 是 32 字节十六进制 PRF key。

浏览器初始化基本模式：

```typescript
import init, {
  create_keystore,
  derive_accounts,
  sign_message,
  sign_tx,
} from "@consenlabs/tcx-wasm";

await init();
```

## Passkey 网页钱包蓝图

先做测试网最小版本：

- 在浏览器里初始化 `@consenlabs/tcx-wasm`。
- 创建或解锁 Passkey-backed keystore。
- 派生一个 Ethereum Sepolia 账户。
- 显示地址和复制按钮。
- 签一条普通测试消息。
- 只有在做出可读 Review 页面后，才进入交易签名。
- 在模拟和二次确认做好之前，保持广播功能关闭。

Passkey 相关检查：

- 确认应用部署在预期的 HTTPS 域名下。
- 确认 `rpId` 与 relying-party 域名匹配。
- 把 WebAuthn PRF 当成浏览器和平台能力，不假设所有环境都可用。
- 在产品说明中写清楚恢复、导出、设备丢失和多设备行为。

## WASM 批量钱包快速参考

批量钱包 Skill 应基于 `@consenlabs/tcx-wasm`。

基本模式：

- 在隔离的本地 Node 工作区安装 `@consenlabs/tcx-wasm`。
- `await init()` 初始化 WASM。
- 用 `create_keystore` 创建 keystore，但不要把返回的 keystore JSON 打印给模型。
- 用 `derive_accounts` 派生公开地址。
- 输出只包含钱包名、链、网络、派生路径、公开地址、keystore 文件路径和状态的 manifest/CSV。
- 禁止调用 `export_mnemonic`，除非用户在受控 UI 中自行操作，且不经过 AI 对话。

## 钱包安全清单

交付任何钱包相关代码或说明前，先检查：

- 默认网络是测试网。
- 代码、日志、文档、截图和提示词里没有真实助记词、私钥、种子短语或正式 API Key。
- `.env.example` 只包含占位符。
- 任何 `VITE_*` 值都可以安全暴露在浏览器 bundle 中。
- 签名前展示 `chainId`、`from`、`to`、`value`、`data`、gas 字段和 token approval 风险。
- 广播前必须先分析、签名，并得到用户明确确认。
- 错误信息解释发生了什么，但不泄露秘密。
- 创建 keystore 时写清楚恢复和数据丢失风险。

## 提示词模板

先调研：

```text
请先阅读 TokenCore 相关 README 和当前项目结构。
不要写代码。
请说明你理解到的钱包能力、风险点、缺失信息和推荐实现计划。
计划必须默认使用测试网，并为每一步写出验证方式。
```

Passkey 钱包最小版本：

```text
基于 @consenlabs/tcx-wasm 设计一个 Passkey 网页钱包 MVP。
只支持 Ethereum Sepolia。
功能只包括：初始化 WASM、创建或解锁 Passkey keystore、派生地址、复制地址、签名测试消息。
不要做主网、Swap、真实资产、自动广播。
先给计划，不要直接写代码。
```

批量钱包 Skill：

```text
基于 @consenlabs/tcx-wasm 设计一个钱包批量创建 Skill。
自然语言输入批量数量、链和网络后，只创建本地 keystore 并导出公开地址清单。
不要读取、打印或导出助记词、私钥、密码、PRF key 或 keystore 内容。
```

## 示例

### 示例 1：Passkey 网页钱包

- 输入："做一个基于 TokenCore WASM 的 Passkey 网页钱包。"
- 步骤：调研 `tcx-wasm`，定义测试网 MVP，实现初始化，加入 Passkey keystore 流程，派生地址，加入消息签名，再评审交易签名。
- 验收：浏览器能加载 WASM，能创建或解锁测试 keystore，能显示 Sepolia 地址，能签测试消息，并且不触碰主网。

### 示例 2：WASM 批量钱包 Skill

- 输入："基于 TokenCore WASM 做一个 Wallet Batch Skill。"
- 步骤：调研 `tcx-wasm`，定义安全边界，设计 Node/WASM 批量脚本，派生公开地址，生成 manifest/CSV。
- 验收：Skill 能批量创建测试钱包地址，同时不读取或输出任何秘密材料。

### 示例 3：钱包 Skill 编写

- 输入："把钱包开发流程整理成一个 Agent Skill。"
- 步骤：提取触发条件，定义安全边界，列出命令，加入提示词模板，加入只使用测试网的例子。
- 验收：Skill 能阻止不安全默认行为，并给 Agent 提供具体的钱包工作流。

## 参考

- `references/source-notes.md`：来源事实和安全说明。
- TokenCore WASM: <https://github.com/consenlabs/token-core-monorepo/tree/tenth-anniversary/token-core/tcx-wasm>
