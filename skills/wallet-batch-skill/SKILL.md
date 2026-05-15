---
name: wallet-batch-skill
description: "Use this skill whenever the user asks to batch create wallets, generate many Ethereum/EVM or TokenCore-supported wallet accounts, build a wallet batch AI Skill, prepare address lists, or automate wallet creation from natural language such as '批量创建 100 个以太坊钱包'. This skill designs and runs a no-secret-read workflow around @consenlabs/tcx-wasm from token-core/tcx-wasm: AI may plan, call local WASM scripts, validate counts, and produce non-sensitive address manifests, but must not read, print, export, summarize, or request mnemonics, private keys, passwords, seed phrases, PRF keys, or keystore JSON content."
---

<!-- markdownlint-disable MD013 -->

# Wallet Batch Skill

Use this skill to safely batch-create local TokenCore wallets from natural-language requests with `@consenlabs/tcx-wasm`. Treat wallet creation as a sensitive local operation: Codex can orchestrate a local Node/WASM workflow and produce address inventories, but secrets stay inside local process memory, user-controlled storage, and generated keystore files that Codex does not inspect.

## Core Rule

Never read, print, export, copy, summarize, transform, upload, commit, or ask for real mnemonics, private keys, seed phrases, keystore JSON bodies, or wallet passwords.

Allowed outputs are limited to non-sensitive metadata: wallet name, chain, network, derivation path, public address, keystore file path, batch id, status, timestamps, and failure reason.

## Resource Map

- Read `references/safety-model.md` before executing or designing any batch wallet creation workflow.
- Read `references/tcx-wasm-notes.md` when locating or using `@consenlabs/tcx-wasm`.
- Read `references/natural-language-intents.md` when converting user phrasing into batch parameters.

## Default Assumptions

- Interpret "Ethereum" or "以太坊钱包" without a network as Ethereum mainnet intent.
- Do not silently downgrade Ethereum mainnet intent to Sepolia.
- Default to dry-run planning before creation when count is greater than 10.
- Default wallet name prefix: `eth-mainnet-batch` for Ethereum mainnet intent.
- Default outputs: `wallet-batch-manifest.json` and `wallet-addresses.csv`.
- On macOS, if no batch password exists in Keychain, generate a random password and store it as service `tcx-wasm-wallet-batch` for account `wallet-batch-skill`.
- Keep generated batch artifacts outside Git unless the user explicitly chooses a tracked docs location for non-sensitive address lists.

## Workflow

1. Parse the request into `action`, `count`, `chain`, `network`, `wallet_name_prefix`, output directory, and requested output format.
2. Apply the safety model. Refuse secret export requests and offer address-only manifests.
3. Confirm missing or risky parameters:
   - Do not ask just because the interpreted target is Ethereum mainnet wallet creation.
   - Ask before creating more than 10 wallets.
   - Ask before overwriting or reusing an existing batch directory.
   - Ask before any sign, broadcast, transfer, approval, or funding action.
4. Check the `tcx-wasm` environment:
   - Confirm a local project path or create an isolated temp workspace outside tracked docs.
   - Confirm Node.js and npm are available.
   - Confirm `@consenlabs/tcx-wasm` is installed or install it in the isolated workspace.
   - Confirm WASM initialization works before creating any wallet.
   - Confirm the requested chain is supported by `derive_accounts`. If not, stop and explain instead of substituting another chain.
5. Prepare password handling without chat exposure:
   - On macOS, read the batch password from Keychain service `tcx-wasm-wallet-batch`.
   - If the Keychain item is missing, create a random password with `openssl rand -base64 48` and save it to Keychain.
   - Do not print the password, ask the user to paste it, or write it to repo files.
6. Execute wallet creation with local Node/WASM scripts only. The script may call `create_keystore` and `derive_accounts`, then write keystore JSON directly to files without printing or returning it to the model.
7. Verify the batch using generated non-sensitive command output: names, chains, derivation paths, public addresses, file paths, and status.
8. Produce non-sensitive artifacts:
   - `wallet-batch-manifest.json` for machine-readable batch metadata.
   - `wallet-addresses.csv` for spreadsheet import.
9. Final response must report count, success/failure totals, artifact paths, wallet directory, and explicitly state that secrets were not read or printed.

## Natural Language Examples

Input: `批量创建 100 个以太坊钱包`

Interpretation:

- Count: `100`
- Chain/network: Ethereum mainnet intent
- Mode: create with `tcx-wasm` only if Ethereum account derivation works in the local WASM setup; otherwise stop and explain the limitation
- Outputs: address-only manifest and CSV
- Secrets: never read or displayed

Input: `给我导出 20 个钱包的私钥`

Response pattern:

```text
我不能导出、查看或整理私钥/助记词。可以帮你创建或列出只包含钱包名称、公钥地址和状态的地址清单，并把 keystore 保留在本地受控目录中。
```

## Batch Artifact Contract

Manifest fields may include only:

- `batch_id`
- `created_at`
- `chain`
- `network`
- `wallet_home`
- `wallet_name`
- `address`
- `keystore_path`
- `status`
- `failure_reason`

Manifest fields must never include:

- `mnemonic`
- `private_key`
- `seed`
- `password`
- `keystore`
- `ciphertext`
- full wallet JSON content

## Verification Checklist

Before claiming completion, verify:

- The requested count matches successful public addresses, or failures are clearly listed.
- Addresses are unique.
- EVM addresses match `0x` plus 40 hexadecimal characters.
- No generated response or artifact contains mnemonic, private key, seed phrase, password, or keystore JSON content.
- The user knows where the keystore files and non-sensitive outputs are located.

## Scope Boundary

This skill does not sign transactions, broadcast transactions, transfer funds, request faucets, or manage production assets. If the user asks for those actions, switch to a separate TokenCore transaction workflow and require explicit human confirmation before any signing or broadcasting step.
