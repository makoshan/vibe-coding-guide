# Wallet Batch Safety Model

<!-- markdownlint-disable MD013 -->

## Security Objective

Enable repeatable local wallet batch creation without letting the AI process become a custody surface. Codex may coordinate tools and inspect public metadata, but wallet secrets must remain inside local `tcx-wasm` scripts, generated keystore files, and user-controlled secret entry paths.

## Hard Prohibitions

Do not:

- Ask the user to paste mnemonics, private keys, seed phrases, wallet passwords, or production API keys.
- Read wallet JSON files, keystore files, encrypted wallet bodies, PRF keys, or password files.
- Use commands that print secrets to stdout or stderr.
- Aggregate keystores into a single file.
- Save secrets in Markdown, CSV, JSON, shell history, environment templates, screenshots, logs, commits, or chat.
- Run signing, broadcasting, transfer, approval, or funding steps as part of wallet batch creation.

## Safe Data

The following data is safe to read and report:

- Public address.
- Wallet name.
- Chain and network label.
- Batch id.
- Keystore file path.
- Creation status.
- Failure reason that does not include secret material.
- Timestamp.

## Confirmation Gates

Require explicit user confirmation before:

- Creating more than 10 wallets.
- Reusing or overwriting an existing output directory.
- Setting one password for a batch.
- Exporting wallet files.
- Running any command that could sign, broadcast, transfer, approve, or fund.

Do not require confirmation merely because the target is Ethereum mainnet wallet
creation. Wallet creation is local key material generation; the high-risk
actions are signing, broadcasting, funding, approvals, transfers, and secret
export.

Do not require confirmation just because the Keychain password item is missing.
On macOS, create a random batch password locally and store it in Keychain instead.
This keeps the workflow non-interactive without moving the password through chat.

## Refusal Patterns

If the user asks to export private keys or mnemonics, refuse briefly and offer an address-only manifest.

If the user asks to inspect wallet files, explain that the workflow intentionally avoids reading keystore content. Offer to validate existence, count, names, and public addresses through non-sensitive script output instead.

If the CLI prints sensitive content unexpectedly, stop. Do not summarize the output. Tell the user the command produced sensitive material and recommend switching to a safer command or filtering mode.

## Final Safety Statement

Every completion report should include a short statement like:

```text
本次流程只读取和整理了公开地址与状态信息，没有读取、输出或保存助记词、私钥、密码或 keystore 内容。
```
