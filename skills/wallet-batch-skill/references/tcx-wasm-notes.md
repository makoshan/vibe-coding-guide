# TokenCore WASM Notes

<!-- markdownlint-disable MD013 -->

## Source

TokenCore WASM:
<https://github.com/consenlabs/token-core-monorepo/tree/tenth-anniversary/token-core/tcx-wasm>

The `tcx-wasm` README describes a browser-side WebAssembly build of TokenCore for multi-chain key management, account derivation, transaction signing, message signing, PSBT signing, and message encryption/signing APIs. The public npm package is `@consenlabs/tcx-wasm`.

Important conventions:

- Initialize the WASM module before calling exported functions.
- Most exported APIs accept JSON strings and return JSON strings.
- `create_keystore` creates a native TokenCore HD keystore with `password`, or a Passkey envelope with `prfKey`.
- `derive_accounts` derives public accounts from a keystore and an unlock key.
- `export_mnemonic` exists but must not be used by this skill.

## Environment Setup Pattern

Use the current project if it already depends on `@consenlabs/tcx-wasm`. Otherwise create an isolated temp workspace outside Git:

```bash
mkdir -p /tmp/tcx-wasm-wallet-batch
cd /tmp/tcx-wasm-wallet-batch
npm init -y
npm install @consenlabs/tcx-wasm
```

For upstream local development, the README documents repository-level commands:

```bash
git clone --branch tenth-anniversary https://github.com/consenlabs/token-core-monorepo.git
cd token-core-monorepo
make build-wasm
make dev-wasm
make build-npm
```

Prefer the npm package for wallet batch automation unless the user explicitly asks to develop the upstream Rust/WASM package.

## Wallet Creation Pattern

The local script should:

1. Import and await `init()` from `@consenlabs/tcx-wasm`.
2. Call `create_keystore(JSON.stringify(...))` with a local password and generated mnemonic or entropy.
3. Immediately write the returned keystore JSON to a keystore file without printing it.
4. Call `derive_accounts(JSON.stringify(...))` to get public addresses.
5. Print only non-sensitive metadata for manifest generation.

Minimal API shape:

```typescript
import init, { create_keystore, derive_accounts } from "@consenlabs/tcx-wasm";

await init();

const keystoreJson = create_keystore(JSON.stringify({
  password: process.env.TCX_WASM_BATCH_PASSWORD,
  network: "MAINNET",
}));

const accounts = JSON.parse(derive_accounts(JSON.stringify({
  keystoreJson,
  key: process.env.TCX_WASM_BATCH_PASSWORD,
  derivations: [{
    chain: "ETHEREUM",
    derivationPath: "m/44'/60'/0'/0/0",
    chainId: "1",
    network: "MAINNET",
  }],
})));
```

Do not print `keystoreJson`, password, mnemonic, entropy, PRF key, or full error payloads that may include secret inputs.

## Supported Chain Selection

Use README-supported `derive_accounts` chain values only. Common mappings:

- Ethereum mainnet: `chain = "ETHEREUM"`, `chainId = "1"`, `network = "MAINNET"`, path `m/44'/60'/0'/0/0`.
- Ethereum Sepolia: `chain = "ETHEREUM"`, `chainId = "11155111"`, `network = "TESTNET"`, path `m/44'/60'/0'/0/0`.
- TRON: `chain = "TRON"`, path `m/44'/195'/0'/0/0`.
- Bitcoin-family chains require explicit address type and network choices; ask before assuming.

Do not silently replace an unsupported chain with a supported one.

## Safe Verification

Prefer script output that lists only:

- wallet name
- chain
- network
- derivation path
- public address
- keystore file path
- status

Do not open generated keystore files to inspect their contents. Validate file existence and count with filesystem metadata only.

## Password Handling

Use Keychain-backed password handling on macOS for batch automation.

Default Keychain identity:

```text
account: wallet-batch-skill
service: tcx-wasm-wallet-batch
```

Check whether the item exists:

```bash
security find-generic-password \
  -a wallet-batch-skill \
  -s tcx-wasm-wallet-batch >/dev/null
```

If the item is missing, create a random local password:

```bash
security add-generic-password \
  -U \
  -a wallet-batch-skill \
  -s tcx-wasm-wallet-batch \
  -w "$(openssl rand -base64 48)" >/dev/null
```

Use it only as a process-local environment variable:

```bash
TCX_WASM_BATCH_PASSWORD="$(
  security find-generic-password \
    -a wallet-batch-skill \
    -s tcx-wasm-wallet-batch \
    -w
)" node scripts/create-wallet-batch.mjs
```

Do not print the password. Do not ask the user to paste it into chat. Do not write it to `.env`, Markdown, shell scripts, logs, or committed files.
