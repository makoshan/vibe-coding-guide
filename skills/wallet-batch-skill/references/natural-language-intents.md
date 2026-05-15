# Natural Language Intent Mapping

<!-- markdownlint-disable MD013 -->

## Intent Schema

Map user requests into:

```text
action: batch_create_wallets | list_wallet_batch | verify_wallet_batch | resume_wallet_batch
count: positive integer
chain: ethereum | evm | explicit chain name
network: ethereum-mainnet | explicit network
wallet_name_prefix: string
output_dir: path
outputs: manifest_json, addresses_csv
security_mode: no-secret-read
confirmation_required: boolean
```

## Defaults

- "以太坊钱包" means Ethereum mainnet intent unless the user explicitly says testnet.
- Never silently replace Ethereum mainnet intent with Sepolia.
- Use `tcx-wasm` supported chain values and stop if the requested chain cannot be derived safely.
- "EVM 钱包" means ask for the target EVM network unless the surrounding context already names one.
- "批量创建 N 个" maps to `count = N`.
- "地址清单" maps to `addresses_csv`.
- "manifest" or "清单" maps to `wallet-batch-manifest.json`.
- "测试钱包" maps to testnet.

## Examples

Input:

```text
批量创建 100 个以太坊钱包
```

Parsed intent:

```text
action: batch_create_wallets
count: 100
chain: ethereum
network: ethereum-mainnet
wallet_name_prefix: eth-mainnet-batch
outputs: manifest_json, addresses_csv
security_mode: no-secret-read
confirmation_required: false
tool_support_required: tcx-wasm Ethereum account derivation
```

Input:

```text
生成 5 个 Base Sepolia 钱包地址
```

Parsed intent:

```text
action: batch_create_wallets
count: 5
chain: base
network: base-sepolia
wallet_name_prefix: base-sepolia-batch
outputs: manifest_json, addresses_csv
security_mode: no-secret-read
confirmation_required: false
```

Input:

```text
把 20 个钱包的私钥导出来给我
```

Parsed intent:

```text
action: refused_secret_export
security_mode: no-secret-read
```

Safe response:

```text
我不能导出或查看私钥。可以帮你生成只包含钱包名称、公钥地址和状态的地址清单。
```
