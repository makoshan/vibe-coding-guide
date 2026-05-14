<!-- markdownlint-disable MD013 MD033 MD041 -->

<p align="center">
  <img src="https://github.com/tukuaiai.png" alt="Vibe Coding 指南" width="80px">
</p>

<div align="center">

# Vibe Coding 指南

**给完全非程序员看的 AI 编程入门手册：从一句话想法，到一个能打开、能点击、能分享的软件。**

---

<p>
  <a href="./prompts/"><img src="https://img.shields.io/badge/提示词-精选-purple?style=for-the-badge" alt="提示词精选"></a>
  <a href="./skills/"><img src="https://img.shields.io/badge/Skills-技能大全-forestgreen?style=for-the-badge" alt="Skills 技能大全"></a>
  <a href="./libs/external/prompts-library/prompt_docs/"><img src="https://img.shields.io/badge/提示词-大全-orange?style=for-the-badge" alt="提示词大全"></a>
  <a href="https://docs.google.com/spreadsheets/d/1ngoQOhJqdguwNAilCl1joNwTje7FWWN9WiI2bo5VhpU/edit?gid=2093180351#gid=2093180351&range=A1"><img src="https://img.shields.io/badge/提示词-在线表格-blue?style=for-the-badge" alt="提示词在线表格"></a>
  <a href="https://github.com/x1xhlol/system-prompts-and-models-of-ai-tools"><img src="https://img.shields.io/badge/系统提示词-参考仓库-slateblue?style=for-the-badge" alt="系统提示词参考仓库"></a>
</p>

</div>

---

## 什么是 Vibe Coding

简单来说：

> 你说清楚想要什么，AI 先写出来，你来检查和调整。

人负责：

- 提目标
- 看结果
- 做决定

AI 负责：

- 写草稿
- 补代码
- 解释原因

软件就是这样从一句话，慢慢变成一个能跑的东西。

但注意，Vibe Coding 不是“躺着等 AI 交付”。

更准确的说法是：

> 像产品经理一样说需求，像测试一样验结果。

---

## 不是纯 Vibe，而是人机协同

大家习惯叫它 Vibe Coding，也有人强调是 Human-Agent Engineering：人类与 AI Agent 协同工程。

意思是：AI 不是替你负责的外包团队，而是一个很快、很勤奋、但需要你管理的实习生。其实 AI 写出来的代码质量上限，取决于你的需求能力、结构能力、判断能力和验收能力。

如果 AI 表现不好，很多时候不是模型太差，而是：

- 你的需求没说清楚
- 项目结构太乱
- 上下文给得不够
- 没有测试和验收标准
- 你没有认真检查 AI 写出来的东西

所以真正可靠的 AI 编程不是“随便说一句，让 AI 全权负责”，而是：

> 人负责规划、上下文、判断和验收；AI 负责加速实现。

---

## 最短路径

如果你只想先做出第一个能打开的 imToken 钱包 Demo，不要先学一堆工具。

先准备这 4 个就够了：

| 工具 | 用来做什么 | 小白理解 |
| --- | --- | --- |
| [Cursor](https://cursor.com/) | 写代码、改代码、和 AI 对话 | 你的 AI 编程工作台 |
| [GitHub](https://github.com/) | 保存代码、备份版本、方便回滚 | 代码网盘 + 历史记录，也可以直接用 GitHub Pages 部署网站 |
| [Vercel](https://vercel.com/) | 把网页钱包 Demo 部署到网上 | 一键生成公开访问链接 |
| AI 模型 | 真正帮你思考和写代码 | 软件大脑 |

第一阶段不要追求工具最全，只追求流程跑通：

> Cursor 做钱包 Demo → GitHub 保存 → Github Page 部署 → 链接发给别人看。

---

## 工具有哪些

### 小白必备

- [Cursor](https://cursor.com/)：推荐小白的第一站。类似的产品 VS Code，Antigravity，可以看项目、改文件、执行命令。
- [GitHub](https://github.com/)：每完成一步就保存一次。项目坏了，可以回到之前的版本。
- [GitHub CLI](https://docs.github.com/zh/github-cli/github-cli/quickstart)：命令叫 `gh`，可以在终端登录 GitHub、创建仓库和提交 PR。
- [Vercel](https://vercel.com/)：适合部署网页钱包 Demo、前端应用和简单全栈应用。把项目连上 GitHub 后，可以自动生成访问链接。

### AI 模型推荐

| 场景 | 推荐 |
| --- | --- |
| 复杂项目、长任务、重构 | [Claude Opus 4.7](https://www.anthropic.com/news/claude-opus-4-7)、[GPT-5.5](https://openai.com/index/introducing-gpt-5-5/)、[GPT-5.2-Codex](https://openai.com/index/introducing-gpt-5-2-codex/) |
| Cursor 内优先尝试 | [Claude Opus 4.7](https://www.anthropic.com/news/claude-opus-4-7)、[GPT-5.5](https://openai.com/index/introducing-gpt-5-5/)、[Composer 2](https://cursor.com/blog/composer-2) |
| 国内或低成本备选 | [GLM-5.1](https://docs.z.ai/guides/llm/glm-5.1)、[Kimi K2.6](https://www.kimi.com/ai-models/kimi-k2-6)、[DeepSeek V4-Pro / V4-Flash](https://api-docs.deepseek.com/news/news260424)、[MiniMax M2.5](https://www.minimax.io/models/text)、[Qwen 3.6 Plus](https://docs.qwencloud.com/developer-guides/getting-started/text-generation-models) |
| Google 路线 | [Gemini 3.1 Pro Preview](https://deepmind.google/models/model-cards/gemini-3-1-pro) |

<details>
<summary>进阶工具：跑通第一个项目后再看</summary>

等你跑通第一个项目，再看这些：

- [Claude Code](https://www.anthropic.com/claude-code)：适合复杂项目、终端工作流、代码库级修改。
- [Codex CLI](https://github.com/openai/codex) / [ChatGPT Codex](https://chatgpt.com/codex)：适合大型代码任务、代码审查、自动化开发。
- [GitHub Copilot](https://github.com/features/copilot)：适合已经在 GitHub 和 IDE 中工作的开发者。
- [Bolt](https://bolt.new/) / [Lovable](https://lovable.dev/) / [v0](https://v0.dev/)：适合快速生成网页钱包界面或应用原型。
- [Semgrep](https://semgrep.dev/)：扫描安全问题和明显代码风险。
- [Playwright](https://playwright.dev/)：让 AI 自动打开浏览器、点击页面、检查功能是否符合预期。
- [Graphite](https://graphite.dev/) / [CodeRabbit](https://www.coderabbit.ai/) / [Qodo](https://www.qodo.ai/)：辅助代码评审。
- [MCP](https://modelcontextprotocol.io/) / [SubAgent](https://docs.anthropic.com/en/docs/claude-code/sub-agents)：让 AI 连接外部工具，或拆成多个角色协作。

</details>

---

## 提示词如何写

AI 不怕你说得多，怕你说得含糊。

### 1. 说清楚输入和输出

不要只说：

```text
帮我做一个钱包功能。
```

更好的说法：

```text
用户点击“创建钱包”后：
1. 系统创建 Ethereum、BSC、Bitcoin、Solana、TRON 五条主网地址。
2. 创建成功后，显示五条链地址和复制按钮。
3. 如果当前浏览器不支持 Passkey，显示“当前浏览器暂不支持 Passkey，请换 Chrome 或 Safari 再试”。
4. 允许用户主动备份助记词，但必须显示离线保存、不要截图、不要上传、不要发给 AI 的安全提示。
5. 暂时不做余额查询、转账、广播和资产管理。
先不要写代码，先告诉我你理解的流程和需要改哪些文件。
```

你要尽量说清楚：

- 用户做了什么
- 系统应该返回什么
- 出错时怎么办
- 哪些功能暂时不要做

### 2. 给参考案例和上下文

不要只说：

```text
钱包首页做得好看一点。
```

更好的说法：

```text
我希望 imToken 钱包 Demo 页面干净、可信、不要像游戏或营销页。
布局参考 imToken 官网和常见钱包首页：顶部显示产品名，中间显示 Demo 钱包状态，下面显示五条链地址、复制按钮和助记词备份入口。
主色用黑白灰，关键按钮用蓝色，危险动作用红色。
我会给你截图或链接，你先总结风格，再写实现方案。
```

可以给 AI 的参考包括：

- 截图
- 链接
- 竞品页面
- 你喜欢的颜色和排版
- 你不想要的风格

### 3. 分步骤迭代

不要一次性提十个需求。

正确节奏是：

1. 先做核心功能。
2. 跑起来。
3. 检查结果。
4. 保存版本。
5. 再加下一个细节。

一句话：

> 每次只改一个点，改完验证，再做下一步。

---

## 方法论

### 40 / 20 / 40

AI 编程时代，写代码本身反而不是最重要的部分。

一个很实用的时间分配是：

| 阶段 | 比例 | 你要做什么 |
| --- | --- | --- |
| Planning | 40% | 想清楚目标、边界、页面、数据、验收标准 |
| Coding | 20% | 让 AI 按计划写代码 |
| Testing | 40% | 运行、检查、修 bug、Review、确认能交付 |

也就是说：

> 前面想清楚，后面验明白，中间让 AI 加速。

### 核心开发循环

每次做功能，都按这个循环走：

1. **Research 调研**：让 AI 先阅读项目、理解背景、查官方文档。
2. **Plan 规划**：让 AI 先给方案和步骤，你确认后再执行。
3. **Implement 实现**：一次只做一个小功能。
4. **Test 测试**：跑起来，看有没有报错，检查是否符合预期。
5. **Review 评审**：看页面、看代码、看边界情况，再决定是否进入下一步。

关键提示词：

```text
先不要写代码。请先阅读项目结构，说明你理解了什么，再给出实现计划。
计划必须分成小步骤，每一步都要有验证方式。等我确认后，你再执行第一步。
```

---

<details>
<summary>如何一步一步开始：两个可以跟做的 Demo</summary>

## 如何一步一步开始

这一节只保留两个可以跟做的 Demo，不讲大段概念。

1. **Demo 1**：基于 Token Core WASM 做 Passkey 多链地址 Demo。
2. **Demo 2**：基于 Token Core CLI Demo 做一个给 AI 用的钱包 Skill。

### Demo 1：基于 Token Core WASM 做 Passkey 多链地址 Demo

你可以把每一步里的提示词复制给 AI，让 AI 带你做。每一步完成后先测试，再保存一个本地 Git 版本。

已经有一个可参考的 Demo：

- GitHub 仓库：[makoshan/MyWallet](https://github.com/makoshan/MyWallet)
- 在线预览：[Passkey 多链钱包 Demo](https://makoshan.github.io/MyWallet/)

真实开发不是直线。你一开始可能会想做完整钱包：多链、余额、收款、转账、广播、邮箱登录、资产管理全都要。

但如果时间有限，第一版最好先收窄成一个能验证核心能力的小 Demo：

> 创建 Ethereum、BSC、Bitcoin、Solana、TRON 五条主网地址，展示地址，并完成助记词备份。

暂时不要做：

- 余额查询
- 转账和广播
- 交易记录
- 邮箱登录
- Portfolio 资产管理
- 代币资产，例如 ERC20、TRC20、SPL Token

#### 第 0 步：准备工具

你需要：

- 安装 Cursor
- 注册 GitHub
- 安装 Git
- 安装 GitHub CLI，命令叫 `gh`
- 注册 Vercel
- 准备一个可用 AI 模型

如果你不会安装，可以直接问 AI：

```text
我不会编程。请一步一步带我准备开发环境，用来做一个 PC 网页钱包 Demo。

我需要：
1. 安装 Cursor。
2. 注册 GitHub。
3. 安装 Git。
4. 安装 GitHub CLI，命令叫 gh。
5. 注册 Vercel 或确认我可以用 GitHub Pages 部署。

请每一步都告诉我：
1. 去哪里下载或注册。
2. 需要点击什么。
3. 完成后如何验证。
4. 如果失败，我应该复制哪段错误给你。

先不要写代码。
```

#### 第 1 步：写一句话目标

先不要想技术，先写人话。

示例：

```text
我要做一个个人自用的 PC 网页钱包 Demo。

目标：
1. 基于 imToken 开源的 Token Core WASM：
   https://github.com/consenlabs/token-core-monorepo/tree/tenth-anniversary/token-core/tcx-wasm
2. UI 参考 imToken token-ui：
   https://github.com/consenlabs/token-ui
3. 用户可以用 Passkey 创建和解锁钱包。
4. 第一版支持五条主网地址：
   - Ethereum
   - BSC
   - Bitcoin
   - Solana
   - TRON
5. Solana 如果 Token Core WASM 不直接支持，请用 Solana 专用库自己扩展地址派生。
6. 用户可以主动导出助记词备份。
7. 导出助记词前必须用 Passkey 解锁，并显示安全提醒。

暂时不做：
1. Swap。
2. 跨链。
3. 法币入金。
4. 邮箱登录。
5. 复杂交易记录。
6. 后台托管私钥。
7. 自动上传助记词备份。
8. 未经二次确认的自动广播交易。

请先不要写代码。
请先复述你理解的项目目标、UI 参考、暂时不做的功能、主要页面和安全边界。
```

#### 第 2 步：让 AI 先问你问题

把一句话目标发给 AI，然后让它反问你。

```text
我不会编程。请你像产品经理一样，围绕这个钱包 Demo 问我 10 个问题，帮我把需求整理清楚。
先只问问题，不要写代码。
```

回答完问题后，再让 AI 整理：

```text
请根据我的回答，整理一份清晰的项目说明。
要求包含：目标用户、核心功能、暂时不做的功能、页面结构、钱包安全边界、验收标准。
```

如果你说“实现所有功能”，要让 AI 帮你翻译成可落地范围。真实项目里，“所有功能”通常只是愿望，不是实施计划。

#### 第 3 步：生成项目文档

让 AI 生成 5 个文件内容：

| 文件 | 用途 |
| --- | --- |
| `project-brief.md` | 项目说明：钱包 Demo 做什么、给谁用、解决什么问题 |
| `tech-stack.md` | 技术方案：Token Core WASM、Passkey、前端框架怎么选 |
| `design.md` | 设计规范：页面结构、视觉风格、组件规则、交互状态 |
| `implementation-plan.md` | 实施计划：一步一步做 |
| `review-checklist.md` | 验收清单：怎么判断钱包流程做对了 |

可复制提示词：

```text
请把这个项目整理成 5 份 Markdown 文档：
1. project-brief.md：项目目标、用户、核心功能、非目标。
2. tech-stack.md：推荐最简单稳定的技术栈，并解释为什么适合 imToken Token Core WASM 和 Passkey Demo。
3. design.md：设计规范，UI 参考 imToken token-ui，包含页面结构、视觉风格、组件规则、交互状态和安全提示文案。
4. implementation-plan.md：分步骤实施计划，每一步只做一个小功能。
5. review-checklist.md：验收清单，包含功能、页面、错误处理、钱包安全和部署检查。

要求：
1. 面向不会编程的人。
2. 语言要简单。
3. 不要直接写代码。
4. implementation-plan.md 必须一步一步推进，每一步都要有验收方式。
5. 安全要求必须明确写入文档：助记词和私钥不能上传服务器，不能提交 GitHub，不能出现在日志、截图、控制台。
6. design.md 必须参考这个 UI 项目总结风格，但不要直接照搬无关组件：
   https://github.com/consenlabs/token-ui
```

#### 第 4 步：只做最小版本

第一次不要做大而全。

最小版本只需要：

- 能打开页面
- 能看到“个人 Demo，先小额验证，不要直接存入大额资产”的安全提示
- 能完成一个最主要的动作，例如创建钱包并展示 ETH 地址
- 没有明显报错

可复制提示词：

```text
请阅读 project-brief.md、tech-stack.md、design.md、implementation-plan.md 和 review-checklist.md。
现在只执行 implementation-plan.md 的第 1 步，做最小可用版本。
完成后停下来，不要继续第 2 步。
请告诉我你改了哪些文件，以及我如何验证。
```

#### 第 5 步：本地运行和测试

如果你不会运行项目，让 AI 教你。

```text
我不会运行项目。请一步一步告诉我在 Cursor 里怎么启动。
每一步都写清楚我要输入什么命令、在哪里输入、成功后应该看到什么。
如果报错，请告诉我应该复制哪一段错误给你。
```

每完成一个小功能，都让 AI 测试一次：

```text
请测试刚才完成的功能。

要求：
1. 先说明你准备用什么方式测试。
2. 如果需要启动本地服务，请告诉我命令和访问地址。
3. 如果可以自动化测试，请用 Playwright 或浏览器打开页面检查。
4. 如果测试失败，请先解释失败原因，再修复。
5. 修复后重新测试。
6. 测试通过后，告诉我结果和剩余风险。

注意：
不要只说“应该可以”，必须给出实际测试结果。
```

验证时不要只看“AI 说完成了”，你要自己打开页面。

提示：安装 Playwright 后，可以让 AI 自动化打开浏览器、点击按钮、填写输入框，并检查页面功能是否符合预期。

检查：

- 页面能不能打开
- “个人 Demo，先小额验证，不要直接存入大额资产”提示是否明显
- 创建钱包按钮能不能点
- Ethereum、BSC、Bitcoin、Solana、TRON 五条地址是否展示
- 复制地址按钮是否可用
- 助记词备份流程是否有明确安全提示
- 报错提示是否能看懂
- 刷新后是否还正常
- 控制台、日志、截图或 Git 仓库里有没有出现助记词、私钥等敏感信息

#### 第 6 步：保存版本

每完成一个小版本，都建议保存一次。最推荐的节奏是先保存到本地 Git；不需要每次都提交到 GitHub。
GitHub 更适合在一个阶段稳定后再推送，用来备份、部署和分享。

如果你想在终端里操作 GitHub，需要先准备 3 件事：

1. 注册 GitHub 账号。
2. 安装 Git，它负责本地版本管理。
3. 安装 GitHub CLI，它的命令叫 `gh`，负责在终端里登录 GitHub、创建仓库和推送代码。

最简单的用法：

```bash
# macOS，如果已经安装 Homebrew
brew install git gh

# 检查 Git 是否安装成功
git --version

# 登录 GitHub
gh auth login

# 检查是否登录成功
gh status
```

Windows 或 Linux 用户可以按 GitHub 官方文档安装：

- [Git 安装说明](https://git-scm.com/downloads)
- [GitHub CLI 快速入门](https://docs.github.com/zh/github-cli/github-cli/quickstart)

可复制提示词：

```text
现在这个小版本已经测试通过。
请帮我保存一个本地 Git 版本。

要求：
1. 先检查 git status。
2. 总结本次改了哪些文件。
3. 确认没有助记词、私钥、API Key、临时截图或构建产物被提交。
4. 使用清楚的 commit message。
5. 只提交和本步骤有关的文件。
6. 不需要每次都推送到 GitHub，除非我明确要求。
```

你要养成这个习惯：

> 一个小功能完成并验证后，先保存一个本地 Git 版本；阶段稳定后，再推送到 GitHub。

#### 第 7 步：继续下一步，但一次只做一个功能

后续迭代时，不要说“帮我优化整个钱包”。

可复制提示词：

```text
我已经验证当前版本可以用。
请继续 implementation-plan.md 的下一步。

要求：
1. 只做下一步。
2. 不要顺手做后面的功能。
3. 完成后先测试。
4. 测试通过后告诉我如何验证。
5. 等我确认后，再保存本地 Git 版本。
```

常见迭代顺序：

1. 创建基础页面。
2. 创建 Passkey 钱包入口。
3. 接入 `@consenlabs/tcx-wasm`。
4. 生成 Ethereum 地址。
5. 生成 BSC、Bitcoin、TRON 地址。
6. 用 Solana 专用库扩展 Solana 地址。
7. 做 Receive 页面，支持下拉切换五条链地址和二维码。
8. 做 Settings 页面，支持 Passkey 解锁后导出助记词。
9. 做部署检查。

#### 第 8 步：阶段稳定后再推送 GitHub 和部署

项目能本地运行后，再部署出去。

可复制提示词：

```text
当前阶段已经本地测试通过。
现在请帮我准备推送到 GitHub 并部署。

要求：
1. 先检查 git status 和最近 commit。
2. 确认没有助记词、私钥、API Key。
3. 如果有 .env 文件，确认它没有被提交。
4. 如果使用 GitHub Pages，请检查构建配置和 base path。
5. 如果使用 Vercel，请检查构建命令、输出目录和环境变量。
6. 如果存在 VITE_* 环境变量，请判断它们是否会暴露到浏览器，不能放任何正式 API Key、助记词或私钥。
7. 推送前先告诉我将执行哪些命令。
8. 部署完成后，告诉我公开访问链接和验证步骤。
```

部署完成后，检查：

- 公开链接能不能打开
- 手机能不能打开
- 页面刷新是否正常
- 控制台有没有明显报错
- GitHub 更新后 Vercel 是否自动重新部署
- 公开页面和构建产物里有没有出现助记词、私钥、正式 API Key

钱包项目每次都要附带这段安全提示词：

```text
这是个人钱包 Demo，不要直接存入大额真实资产。

安全要求：
1. 不要把助记词、私钥、Passkey PRF key 打印到页面、控制台或日志。
2. 不要把助记词保存到服务器。
3. 不要把正式 RPC API Key 写进前端代码或 GitHub Pages 构建产物。
4. 导出助记词时必须提示用户手抄离线保存，不要截图，不要上传，不要发给 AI。
5. 如果后续加入签名和广播，必须先显示人能看懂的交易摘要，并要求用户二次确认。
6. 每次提交前都检查是否误提交了敏感信息。
```

---

### Demo 2：基于 Token Core CLI Demo 做 AI Skill

参考项目：

- [Token Core CLI Demo](https://github.com/consenlabs/token-core-monorepo/tree/demo/token-core-cli/token-core/tcx-examples/cli)

这个 Demo 的目标不是再做一个网页钱包，而是做一个给 AI 用的 Skill。以后你让 AI 做钱包、签名、交易分析、广播相关功能时，它会先按这个 Skill 的规则工作。

#### 第 0 步：让 AI 阅读 CLI Demo

```text
请阅读这个 Token Core CLI Demo：
https://github.com/consenlabs/token-core-monorepo/tree/demo/token-core-cli/token-core/tcx-examples/cli

目标：
我要基于它做一个给 AI 用的钱包 Skill。

请先不要写代码。
请先总结这个 CLI Demo 里有哪些能力，例如：
1. 创建钱包。
2. 导入钱包。
3. 列出钱包。
4. 分析交易请求或 signed raw tx。
5. 签名交易。
6. 广播交易。
7. policy 预检查。
8. Tenderly 模拟。
9. AI 交易摘要。

请用小白能懂的话说明这些能力适合沉淀成哪些 AI 工作规则。
```

#### 第 1 步：生成 Skill 设计文档

```text
请基于刚才的分析，帮我设计一个钱包开发 Skill。

Skill 目标：
让 AI 以后处理钱包开发、交易分析、签名、广播相关任务时，不是凭空发挥，而是按固定安全流程工作。

要求：
1. 默认测试网优先。
2. 签名前必须解释交易内容。
3. 广播前必须二次确认。
4. 不让 AI 接触真实助记词和私钥。
5. 不把正式 API Key 放进前端环境变量。
6. 先分析，再签名，最后才考虑广播。
7. 如果用户要求主网操作，必须给出风险提醒。
8. 如果用户粘贴交易 JSON，先输出人话摘要和风险点。

请先输出 Skill 设计文档，不要写代码。
```

#### 第 2 步：生成 `skills/tokencore-wallet/SKILL.md`

```text
请根据 Skill 设计文档，创建或更新：

skills/tokencore-wallet/SKILL.md

要求：
1. 文档使用中文。
2. 面向 AI Agent，而不是普通用户。
3. 写清楚触发场景。
4. 写清楚必须遵守的钱包安全规则。
5. 写清楚交易分析、签名、广播的工作顺序。
6. 写清楚哪些事情不能做，例如接触真实助记词、私钥、正式 API Key。
7. 不要加入和 Token Core CLI Demo 无关的内容。
```

#### 第 3 步：用一个任务测试 Skill

```text
请参考 skills/tokencore-wallet/SKILL.md，
基于 imToken Token Core CLI Demo，设计一个钱包交易分析助手。

要求：
1. 只支持测试网。
2. 用户粘贴 tx request JSON 后，先给人话摘要。
3. 再给风险检查结果。
4. 最后才允许进入签名步骤。
5. 广播前必须二次确认。
6. 先给计划，不要直接写代码。
```

#### 第 4 步：保存 Skill 版本

```text
这个 Skill 已经测试可以用。
请帮我保存一个本地 Git 版本。

要求：
1. 先检查 git status。
2. 只提交 skills/tokencore-wallet/SKILL.md 和必要文档。
3. 确认没有助记词、私钥、API Key。
4. commit message 使用：docs: add token core wallet skill guide
5. 不需要推送 GitHub，除非我明确要求。
```

</details>

---

<details>
<summary>做坏了怎么办：报错、页面不好看、AI 改乱了</summary>

## 做坏了怎么办

小白一定会遇到报错。不要慌，报错是正常开发的一部分。

### 页面报错

把报错复制给 AI：

```text
我运行 imToken 钱包 Demo 时报错了。下面是错误信息。
请先解释这个错误是什么意思，再告诉我最可能的 3 个原因。
先不要改代码，先给排查步骤。
如果错误和 Passkey、WASM、Sepolia 网络或环境变量有关，请单独指出。

错误信息：
【粘贴错误】
```

### 页面不好看

不要说“优化一下”，要指出具体问题：

```text
这个钱包页面看起来太拥挤。请只调整布局和间距，不要改功能。
目标风格：干净、可信、像 imToken 的测试网钱包 Demo，不要像营销页。
请先说明你准备改哪些地方，再执行。
```

### AI 改乱了

立刻停止继续加需求。

```text
现在项目可能被改乱了。请先不要继续写新功能。
请帮我检查最近改动，总结哪些文件被改了，哪些改动可能有风险。
然后给我一个回退或修复方案。
```

如果你已经用 GitHub 保存过版本，就可以回到上一个稳定版本。

### 聊天太长了

长对话容易让 AI 忘记重点。

做法：

1. 让 AI 总结当前项目状态。
2. 保存到 `progress.md`。
3. 新开聊天。
4. 让新聊天先读文档再继续。

可复制提示词：

```text
请总结当前项目状态，写成 progress.md 的内容。
包括：已经完成什么、还没完成什么、已知问题、下一步建议。
```

新聊天开头：

```text
请先阅读 project-brief.md、tech-stack.md、design.md、implementation-plan.md、progress.md 和 review-checklist.md。
读完后总结你理解的项目状态。
先不要写代码。
```

</details>

---

<details>
<summary>新手项目目录：推荐保留哪些文档</summary>

## 新手项目目录

建议每个 AI 项目都保留这些文件：

```text
your-project/
├── project-brief.md          # 项目说明
├── tech-stack.md             # 技术方案
├── design.md                 # 设计规范
├── implementation-plan.md    # 实施计划
├── progress.md               # 进度记录
├── review-checklist.md       # 验收清单
├── README.md                 # 给人看的项目说明
└── src/ 或 app/              # 代码目录
```

最重要的是文档，不是代码。

因为 AI 需要上下文，而文档就是上下文。

文件命名建议：

- `README.md` 这种全大写文件名很常见，适合作为“人优先阅读”的入口文档。
- `project-brief.md`、`tech-stack.md`、`design.md`、`implementation-plan.md`、`review-checklist.md` 这种小写加短横线的命名也很常见，适合项目内部规划文档。
- 本教程统一使用 `design.md`，方便和其它规划文档保持同一种命名风格。
- 搜索这些文件时，优先用 `rg --files` 或 `find`，不要手动翻目录。

常用搜索命令：

```bash
# 精确查看这几个规划文档是否存在
ls -1 README.md project-brief.md tech-stack.md design.md implementation-plan.md review-checklist.md

# 在项目里搜索这些文档名出现的位置
rg -n "project-brief.md|tech-stack.md|design.md|implementation-plan.md|review-checklist.md" .

# 只列出 Markdown 文件，适合交给 AI 先读上下文
rg --files -g "*.md"
```

</details>

---

<details>
<summary>进阶学习路线：跑通第一个项目后再补能力</summary>

## 进阶学习路线

参考 Stanford CS146S 和 Mihail Eric 的作业结构，可以按这个顺序进阶：

| 阶段 | 学什么 | 你能获得什么 |
| --- | --- | --- |
| 1 | 提示词基础 | 让 AI 更稳定地理解你 |
| 2 | Cursor 项目实践 | 会用 AI 改真实项目 |
| 3 | 项目文档和规则 | 让 AI 读得懂你的项目 |
| 4 | MCP 和工具调用 | 让 AI 连接外部工具和数据 |
| 5 | 自动化工作流 | 把测试、文档、重构变成可复用流程 |
| 6 | 安全扫描 | 用工具发现明显风险 |
| 7 | 代码 Review | 学会检查 AI 写的代码 |
| 8 | 多技术栈和部署 | 同一个想法能用不同方式实现并上线 |

你不需要一天学完。每做一个小项目，就补一块能力。

</details>

---

<details>
<summary>本仓库怎么用：提示词、技能和文档入口</summary>

## 本仓库怎么用

这个仓库不是一个单独的软件项目，而是一个 AI 编程资料库。

你可以这样使用：

- `prompts/`：找提示词模板。
- `skills/`：找特定工具或领域的 AI 技能。
- `documents/`：看方法论、开发经验、项目模板。
- `libs/external/prompts-library/`：管理提示词的 Excel 与 Markdown 转换工具。
- `libs/common/utils/backups/`：本地备份脚本，做大改动前可以参考。

如果你是小白，建议先从这几个入口开始：

1. 读完本 README。
2. 复制“如何一步一步开始”里的提示词。
3. 做一个最小 imToken 钱包 Demo。
4. 部署到 Vercel。
5. 再回来研究提示词库和技能库。

</details>

---

<details>
<summary>常见问题：不会代码、项目变乱、何时新开聊天</summary>

## 常见问题

### 我完全不会代码，可以学吗？

可以，但你不能完全不看结果。

你不需要一开始手写代码，但你需要学会：

- 描述需求
- 看页面结果
- 复制报错
- 判断功能是否符合预期
- 保存版本

### AI 能不能直接做完整软件？

可以做出原型，但生产级软件仍然需要人的规划、测试和评审。

纯粹“随便说一句，让 AI 全权发挥”很容易得到一个看起来能跑、后面很难维护的项目。

### 我应该先学编程，还是先学 Vibe Coding？

可以同时来。

先用 Vibe Coding 做出东西，建立反馈感；再反过来学习 HTML、CSS、JavaScript、Python、数据库这些基础，会更有动力。

### 项目越改越乱怎么办？

通常是因为没有计划、没有版本保存、一次改太多。

解决方法：

- 新需求先写进计划
- 每次只改一个点
- 改完就验证
- 稳定后保存到 GitHub
- 大改动前让 AI 先读文档

### 什么时候该新开聊天？

当出现这些情况时，建议新开：

- 对话太长
- AI 开始忘记前面约定
- 一个功能已经完成
- 项目方向发生变化
- 你要从修 bug 切换到做新功能

新聊天前，让 AI 先写 `progress.md`。

</details>

---

## 参考资料

- [CS146S: The Modern Software Developer](https://bulletin.stanford.edu/courses/2274401)
- [CS146S Assignments](https://github.com/mihail911/modern-software-dev-assignments)
- [Mihail Eric: AI Software Development](https://maven.com/the-modern-software-developer/ai-course)
- [Cursor](https://cursor.com/)
- [GitHub](https://github.com/)
- [Vercel](https://vercel.com/)
- [imToken TokenCore WASM](https://github.com/consenlabs/token-core-monorepo/tree/tenth-anniversary/token-core/tcx-wasm)
- [imToken Token Core CLI Demo](https://github.com/consenlabs/token-core-monorepo/tree/demo/token-core-cli/token-core/tcx-examples/cli)
- [OpenAI Models](https://developers.openai.com/api/docs/models)
- [Claude Opus 4.7](https://www.anthropic.com/news/claude-opus-4-7)
- [Gemini Models](https://ai.google.dev/gemini-api/docs/models)
- [DeepSeek API Docs](https://api-docs.deepseek.com/)
- [Qwen Code Docs](https://qwenlm.github.io/qwen-code-docs/)

---

## 许可证

本项目采用 [MIT](LICENSE) 许可证。
