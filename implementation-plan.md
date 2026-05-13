# Implementation Plan

## 实施原则

每一步只做一个小功能。

每一步完成后都要能验证，不要一次性做完整钱包。

推荐顺序是：

1. 先做页面和基础结构。
2. 再接 Passkey。
3. 再接 Ethereum Sepolia。
4. 再扩展其他链。
5. 最后做测试网转账和备份。

## 第 1 步：搭建网页项目

目标：

- 创建 React + Vite 项目。
- 页面能在浏览器打开。
- 显示基础钱包页面。

验收：

- 本地能启动网页。
- 浏览器能看到首页。

## 第 2 步：接入设计系统

目标：

- 参考 `/Users/thursday/Downloads/token-ui-main`。
- 做出基础布局：左侧导航、顶部标题、主内容区。
- 使用蓝色主按钮、白色卡片和简洁金融风格。

验收：

- 页面风格接近参考图。
- 有 `Dashboard`、`Portfolio`、`Deposit Funds`、`Settings` 入口。

## 第 3 步：创建 Passkey 页面

目标：

- 做一个 `Create Passkey` 页面。
- 检查浏览器是否支持 Passkey。
- 不支持时显示友好提示。

验收：

- PC 浏览器打开后能看到创建入口。
- 不支持时不会继续创建钱包。

## 第 4 步：初始化 Token Core WASM

目标：

- 安装并加载 `@consenlabs/tcx-wasm`。
- 页面显示初始化成功或失败。

验收：

- 浏览器能成功加载 WASM。
- 控制台没有明显错误。

## 第 5 步：创建 Ethereum Sepolia 测试钱包

目标：

- 用户点击创建 Passkey。
- 创建加密钱包。
- 派生 Ethereum Sepolia 地址。

验收：

- 页面显示 Ethereum Sepolia 地址。
- 地址可以复制。
- 页面不显示助记词和私钥。

## 第 6 步：做 Deposit Funds 页面

目标：

- 显示收款二维码。
- 显示 Ethereum Sepolia 地址。
- 提供复制按钮。
- 显示“只发送 Sepolia ETH”的提示。

验收：

- 二维码和地址对应同一个地址。
- 点击复制后有成功提示。

## 第 7 步：查询 Sepolia ETH 余额

目标：

- 接入 Ethereum Sepolia RPC。
- 查询当前地址余额。
- 在 Dashboard 和 Portfolio 显示余额。

验收：

- 页面能显示 Sepolia ETH 余额。
- 网络错误时显示友好提示。

## 第 8 步：Ethereum Sepolia 测试网转账

目标：

- 用户输入接收地址和金额。
- 页面展示转账 Review。
- 用户用 Passkey 解锁签名。
- 用户确认后广播测试网交易。

验收：

- 广播成功后显示 tx hash。
- 页面提供 explorer 链接。
- 签名前必须显示转账摘要。

## 第 9 步：扩展 BSC Testnet

目标：

- 显示 BSC Testnet 地址。
- 查询 BNB 测试币余额。
- 支持 BSC Testnet 原生币转账。

验收：

- BSC 页面能收款和转出测试 BNB。
- 页面提醒 Ethereum 和 BSC 地址相同但网络不同。

## 第 10 步：扩展 Bitcoin Testnet 或 Signet

目标：

- 显示 Bitcoin 测试网地址。
- 查询 BTC 测试币余额。
- 构建并签名 Bitcoin 测试网交易。

验收：

- 页面能显示 BTC 测试网地址。
- 转账前能展示 UTXO、手续费和找零提示。

## 第 11 步：扩展 Solana Devnet

目标：

- 使用 Solana 专用库生成地址。
- 查询 SOL Devnet 余额。
- 支持 SOL Devnet 转账。

验收：

- 页面能显示 Solana Devnet 地址。
- 能完成一笔 SOL Devnet 测试转账。

## 第 12 步：扩展 TRON 测试网

目标：

- 显示 TRON 测试网地址。
- 查询 TRX 测试币余额。
- 支持 TRX 测试网转账。

验收：

- 页面能显示以 `T` 开头的 TRON 地址。
- 转账前显示带宽、能量或账户激活相关提示。

## 第 13 步：邮箱登录

目标：

- 用户可以用邮箱登录。
- 邮箱只管理账号和设备，不接触私钥。

验收：

- 登录后能进入钱包。
- 退出后可以再次登录。
- 后端不保存明文钱包秘密。

## 第 14 步：导出助记词备份

目标：

- 用户可以主动导出助记词备份。
- 导出前显示强安全提示。
- 导出过程需要 Passkey 解锁。

验收：

- 未解锁不能导出。
- 导出页面有明确风险说明。
- 不在日志里打印助记词。

## 第 15 步：完整验收和部署

目标：

- 跑完整流程。
- 检查错误处理。
- 部署到 HTTPS 网站。

验收：

- 创建钱包、收款、查余额、转账、备份都能完成。
- 部署地址可以在 PC 浏览器打开。

