<!-- markdownlint-disable MD013 -->

# TokenCore Wallet Source Notes

## 来源

- TokenCore WASM: <https://github.com/consenlabs/token-core-monorepo/tree/tenth-anniversary/token-core/tcx-wasm>

## TokenCore WASM 事实

`tcx-wasm` README 将它描述为 TokenCore 的 WebAssembly 构建，可用于浏览器侧多链密钥管理、账户派生、交易签名、消息签名、PSBT 签名，以及 Nostr 风格的消息加密和签名 API。

公开 npm 包是 `@consenlabs/tcx-wasm`。

README 中的重要约定：

- 调用导出函数前需要初始化 WASM 模块。
- 大多数 API 接收 JSON 字符串，并返回 JSON 字符串。
- `create_keystore` 可以通过 `password` 创建原生 TokenCore HD keystore。
- `create_keystore` 也可以通过 `prfKey` 创建 Passkey envelope。
- Passkey envelope 创建需要 `userId`、`credentialId` 和 `rpId`。
- 对 Passkey envelope 来说，`key` 是 32 字节十六进制 PRF key。
- `derive_accounts` 支持多条链，包括 Ethereum 和 Bitcoin-family chains。
- 签名 API 包括 `sign_tx`、`sign_message`、`sign_psbt` 和批量版本。

## 安全说明

即使只是 Demo，钱包代码也属于安全敏感场景。生成示例时应保持测试网优先，签名前要求人话摘要，广播前要求明确确认，并且永远不要让用户把真实秘密粘贴进聊天。批量创建钱包时，AI 只能接触公开地址、状态、路径等非敏感信息，不能读取或输出助记词、私钥、密码、PRF key 或 keystore JSON 内容。
