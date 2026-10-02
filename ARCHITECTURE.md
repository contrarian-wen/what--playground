# Long-term Architecture Constraints · 长期架构约束

> 以下约束长期有效，增量开发时必须遵守；**不要**为提前实现它们而重写网站。

## 1. Project Philosophy

最终形态不是 `Level 0 → 1 → 2 → …` 的线性课程，而是 **Computer Fundamentals Playground**：

```
Computer Fundamentals Playground
├── 01 Computer Basics
├── 02 Command Line ⭐（核心主线）
├── 03 Git & GitHub ⭐（核心主线）
├── 04 Linux & WSL
├── 05 Development Environment
├── 06 Network & Troubleshooting
└── 07 Real Projects
```

- Command Line 与 Git/GitHub 是项目最核心的两个模块——**建立 mental model，不做命令记忆课**。
- 重点辨析：Terminal ≠ Shell、Shell ≠ OS、Command ≠ Program、Git ≠ GitHub。
- 最终目标：Computer Fundamentals → Command Line → Git/GitHub → Linux/WSL → Development Environment → Real Programming Work，知识之间互相连接。

## 2. Learning Architecture

- 结构：**Module → Level → Step**。
- 交互闭环：Understand → Predict → Try → Make Mistake → Diagnose → Review → Ask。
- **先猜后答**：Prediction 先于 Explanation；错误选项必须带 why，错误是教学材料不是失败。
- 虚拟终端引擎支撑「Try」：pwd / ls / cd / mkdir / touch / cat / cp / mv / rm / where / path / path-add / env / echo / notepad / code / explorer / git（init / commit -m / status）等，支持引号参数与 `*` `?` 通配符，操作虚拟文件系统，绝不碰真实电脑。

## 3. Module Structure

- 每个 Module 内部再分 Level，Level 内部再分 Step。
- 实现方式：`LEVELS` 数组中每个 level 带可选 `module` 字段，侧栏按 module 分组渲染。**不允许**把课程写死成 level_0→level_1→… 的线性假设（v1.2 已落地）。
- 产品内 ROADMAP（A–I）是 7 大 Module 的细化路线：A=01 前段（文件系统）、B–C=02 Command Line、D=03 Git & GitHub、E=05 开发环境（含 LaTeX 案例）、F–G=06 网络与系统工具、H=04 Linux & WSL、I=07 排查综合。
- 新增内容类型以新 `type` 值渐进加入，不为未来类型预留空壳代码。

## 4. Level Structure

- 一个 Level 回答**一个具体认知问题**（见 §11），内部 Step 数量按教学需要调整，不硬拆、不硬并。
- Level 编号允许小数段（`8` → `8.1` / `8.2`）：同一认知主题深化时新增 LEVELS 条目，不动旧 Level 内容与进度。
- Level 结尾固定为 Review 页：核心概念、命令表、术语本、易错清单、Quiz、Question Box、下一关入口（Learn → Practice → Review → Next）。

## 5. Progress System

**访问规则（六条，全部已实现）**：

1. Module 之间自由进入
2. Level 之间自由进入
3. 只有 Level 内部的 Steps 按顺序推进
4. Progress 与 Access 分离（进度不锁内容）
5. Progress 只记录学习状态，不限制复习
6. 用户可自由 Review 已完成内容

数据约束：

- 进度 schema（`store.completed[li] = 已完成步骤下标`）冻结；如需 per-step 状态扩展，新增并行字段并做 fallback，不改旧字段语义。
- 每次迭代不得破坏已有进度数据（schema 变更必须 migration/fallback）。
- 进度存 `localStorage`，跨设备用 Export / Import JSON。

## 6. Learning Mode

- 现有关卡类型：concept / predict / explore / terminal / multiselect / matching / ordering / fill / clickmap（v1.3 起后五种为通用渲染器，后续 Level 复用）。
- Terminal 任务卡：Goal / Background / Hint 阶梯（Hint 1 → Hint 2 → … → Answer）/ success / internals（电脑内部发生了什么）/ breakdown（token 逐词解剖）。
- 错误反馈三要素：错在哪、为什么、去哪想；绝不只回「错了」。

## 7. Project Mode（只记录，未实现）

关卡类型预留 `type: "project"`，与 learn 关卡并列。设计定稿：

- **流程**：MISSION 卡（Goal / Context / Requirements / Checkpoints）→ 学生声明 "I'm ready" → 在真实环境（或模拟器）完成 → "Check my work" 自检 → Socratic Project Assistant 用提问引导复盘（不接真 AI，先用规则式 mock）。
- **视觉语言**：Blue = Learn（现有关卡），Orange = Build（Project）。橙色用低饱和暖橙 `#c98f5f`，不用亮橙，保持现有深色 UI 质感。
- **难度递进**：首个 Project Mode 在 Module 03（Git & GitHub）完成后解锁，第一个项目是 **"Create your first Git repository"**；之后每个 Module 末尾挂一个小项目，用该 Module 刚学的工具解决一个小问题。
- Project Mode 不改变六条访问规则；Progress 照常记录，不锁复习。

## 8. AI Question Box

来自 Level 8 需求澄清的架构约束，**现阶段只做记录，不引入任何 backend / API / Node server / API Key**：

- **部署形态（现阶段冻结）**：课程保持纯静态（HTML/CSS/JS/localStorage），Question Box 维持 mock / rule-based。课程必须能在完全无 AI 的环境下完整运行。
- **未来链路**：`Browser → Local Backend → Provider API`。API Key 只存在本地（`.env` + `.gitignore`），浏览器永不直连 API，frontend JS 不硬编码任何 Key。GitHub 仓库公开也不泄露 Key。
- **Provider 可替换**：backend 按兼容接口抽象，DeepSeek / Kimi / 其他兼容 API 可切换，不把项目写死在单一模型上。
- **AI 的角色是 tutor，不是 answer machine**：渐进式 Hint 1 → Hint 2 → Explanation → 最后才给答案（现有 Hint 阶梯已按此交互模型设计）；AI 不能代替用户完成练习。
- **成本透明**：若 API 返回 usage，Tutor 界面显示 input / output / total tokens 与 estimated cost；不为此过度增加复杂度。

## 9. Current Status

- Module A（Levels 0–2）、Module B（Levels 3–5）、Module C（Levels 6–7）已开放并经过多轮试玩打磨。
- **Level 8.0 Terminal Ecosystem（24 Steps）**：已开放。回答 "Where am I?"——GUI vs CLI、Terminal/Shell/Program/Process/OS 骨架、Windows Terminal / PowerShell / CMD / Git Bash / WSL / VS Code integrated terminal 辨析、Permission/Privilege/Elevation、五层诊断法；含关系大图、Unix/Linux/WSL 概念与 What-is-it-NOT 题。
- **Level 8.1 Command Line Foundations（30 Steps）**：已开放。回答 "What am I talking to?"——打字时发生什么、容器与住客、CMD/PowerShell/Bash 口音辨析、GUI↔Terminal 互证。
- **Level 8.2 Command Line as a Language（29 Steps）**：已开放。回答 "How do I speak to it?"——命令的词源与语言感、command/subcommand/option/argument 四件套、引号、code . 与 Terminal 启动 GUI 程序、错误诊断。
- **Level 8.3 Command Syntax（28 Steps）**：已开放。回答 "How is a sentence built?"——句法渐进主线（pwd → cd Documents → ls -l → rm -r oldfolder → git commit -m "hello"）、option vs argument 双重判据、subcommand 工具箱直觉、pattern 不是法律、五步自问法、Unknown Command Challenge；毕业任务 git init → git commit -m。
- **Level 8.4 Command Line in Practice（30 Steps）**：已开放。回答 "Can I write one myself?"——10 个 PowerShell 实操任务（导航 / 引号 / 通配符 / 安全删除 / GUI 召唤 / 毕业建家链），终端状态跨任务累积。
- **Level 8.5 Same Computer, Different Shells（36 Steps）**：已开放。回答 "Why do shells speak different dialects?"——Alias 机制、PowerShell ↔ Bash 核心对照、提示符认环境、Git Bash vs WSL 两个世界、option 品味差异、CMD 第三方言、三步翻译协议、Unknown Command Challenge（grep / python -m / tar）跨方言五步自问法；23 个互动题 + 4 道复习 quiz，纯观察关卡不要求会写 Bash。
- **Level 8.6 Git Bash（32 Steps）**：已开放。回答 "Can I actually use another shell?"——引擎新增 bash 模式（MINGW64 提示符、/c/Users 路径翻译、bash 错误口音、which），17 个 terminal 实操 + 「翻译两步走」题型（先 fill 翻译、再亲手输入同一条命令），毕业考 Bash 四拍建家链；REVIEWS["8.6"] 五键齐全 + 4 道 quiz。
- **Level 8.7 WSL（28 Steps）**：已开放。回答 "What changes when the shell is running inside Linux?"——引擎新增 wsl 模式（独立 Linux 文件系统，/home/yoga 家目录、/mnt/c 与 Windows C 盘双向桥接、apt 模拟），10 个 terminal 实操 + explorer.exe / code . 跨世界召唤、which python3、apt update/install，毕业考跨世界六拍链；REVIEWS["8.7"] 五键齐全 + 4 道 quiz。
- **终端跨关重置修复（v1.7）**：render() 在 store.level 变化时自动 resetTerminal——此前从上一关 Review 页「Start Next Level」会携带旧 cwd / 旧 shell 模式进入新关（例：8.2 毕业考留在 mylab 会污染 8.4 热身；8.5→8.6 仍是 PowerShell 模式）。E2E 八关连跑暴露此真实用户路径 bug，已修。
- 虚拟终端引擎已支持 touch / cat / cp / mv / rm（-r）、引号参数、`*` `?` 通配符、git init / commit -m / status；ls 对选项给出提示分支；bash / wsl 双模式（路径显示与解析翻译、which、apt、错误口音按 shell 切换）。

## 10. Future Roadmap

- **Module C 续：8.6 → 8.7（Bash 分阶段引入策略）**：✅ 已实现（v1.7）。实操主线从 PowerShell 单线扩展为 PowerShell → Bash 双轨——目标是 Command Line fundamentals → PowerShell → Bash basics → Git Bash → WSL / Ubuntu / Bash。Bash **不作为独立门槛**出现，而是渐进对照引入。8.5 完成「对照观察」阶段，8.6 / 8.7 完成「亲手实操」阶段：
  - **8.6 Git Bash — *Can I actually use another shell?***（已开放，32 Steps）
    - Learning objectives：① 在 VS Code / Windows Terminal 里亲手打开 Git Bash 标签页；② 用 Bash 完成 8.4 同款任务链（pwd / ls / cd / mkdir / touch / cp / mv / rm -r / 通配符），体验「mental model 全部通用、只换词汇」；③ 路径翻译实战（/c/Users/... 与 C:\Users\... 互换）；④ 理解 Git Bash 是 Windows 程序（MINGW64、能看到 C 盘、不能跑 apt）。
    - Interaction design（落地版）：引擎 bash 模式 + 「翻译两步走」题型（fill 翻译 → terminal 输入同一条命令）+ 17 个实操任务 + 毕业四拍建家链。
  - **8.7 WSL — *What changes when the shell is running inside Linux?***（已开放，28 Steps）
    - Learning objectives：① 理解 WSL = 完整 Linux 虚拟机（与 Git Bash 的本质区别，8.5 已埋伏笔）；② home 目录、/home vs /mnt/c 双向桥接；③ 最少 Linux 生存词汇（apt、which、~ ）；④ 最终辨析 WSL ≠ Terminal ≠ Bash ≠ VS Code（8.0 五层模型收口）。
    - Interaction design（落地版）：引擎 wsl 模式（独立 Linux FS、/mnt/c 与 C 盘双向可见）+ 「两个世界的桥」主线（explorer.exe . / code . 跨世界召唤）+ apt 模拟 + 毕业跨世界六拍链。
- **Module D Git & GitHub（Levels 9–15）**：Git mental model → GitHub → repository → add/commit → remote/push/pull（含 fetch vs pull 可视化）→ 分支合并 → 阅读真实仓库。
- **Module E Development Environment（16–17）**：VS Code → LaTeX 工具链（.tex → compiler → .pdf；MiKTeX / latexmk / Perl 各是什么）。LaTeX 不单独成 Module，作为「编译器 + 环境变量 + PATH」mental model 的练兵场；用户真实素材（`简历.tex`、`LaTeX-Workshop-2026.zip`）在虚拟文件系统中已埋伏笔，Module 05 回收。之后再教 LaTeX Basics 最小集（`\documentclass`、`\section`、公式、中文排版）。
- **Module F–I**：网络基础 → Windows 系统工具 → WSL & Linux → 排查综合（command not found / permission denied / not a git repository 诊断训练）→ 最终挑战。
- **Project Mode**：见 §7，首个项目在 Module D 之后。
- **AI Tutor 本体**：见 §8，待用户试玩反馈后再定 Phase。

## 11. Content Design Principles

- **每个 Level 必须回答一个具体认知问题**：
  - Level 8.0 — *Where am I?*（终端生态：我在哪、周围都是什么）
  - Level 8.1 — *What am I talking to?*（我在和谁说话：容器、shell、住客）
  - Level 8.2 — *How do I speak to it?*（怎么对它说话：命令的结构与语言）
  - Level 8.3 — *How is a sentence built?*（一句话怎么搭：句法拆解，五步自问法）
  - Level 8.4 — *Can I write one myself?*（我能不能自己写出一条命令：实操）
  - Level 8.5 — *Why do shells speak different dialects?*（为什么 shell 说不同方言：跨方言观察与翻译）
  - Level 8.6 — *Can I actually use another shell?*（我能不能真的用另一个 shell：Git Bash 实操）
  - Level 8.7 — *What changes when the shell is running inside Linux?*（shell 跑在 Linux 里时什么变了：WSL 与两个世界的桥）
  - 后续 Level 立项时先写下一句话问题，再排 Step。
- **先建 mental model，再增词汇（v1.6 新增，Principle 2）**：Command Line 的 mental model（句法四件套、cwd、路径、引号、通配符）跨方言、跨 Level 保值；词汇（具体命令）才是会过时的表层。新增内容时优先扩展模型（新场景、新方言、新题型），而不是堆新命令清单；每种新词汇必须挂在已建立的模型结构上教（8.5 的翻译协议 = 8.3 句法模型 + 对照表词汇）。
- **已介绍 ≠ 已学会（v1.5 新增）**：一个概念在某一关出现过，不代表用户已掌握——每个概念必须有**独立的检验环节**（反向组装、为什么题、陌生场景挑战）。试玩反馈优先于课程假设：如果反馈暴露出知识漏洞，先补漏洞（可以为补漏洞新增 Level / 调整编号），不硬按原计划推进。
- **Mental model 优先**：每个 concept 配 visual diagram；每个 terminal 任务配 internals（电脑内部发生了什么）；抽象概念必须有类比（门禁/邮寄地址/工作台）。
- **Question-driven curriculum**：题型服务于认知目标——词源用 matching/fill、结构用 multiselect、流程用 ordering、辨析用 multiselect/predict、动手用 terminal、综合用 scenario predict。
- **难度递增**：从 pwd / cd / mkdir 开始，再进入 git commit -m；先 predict 再 terminal 实操；错误诊断题放在概念之后。
- **诚实教学**：词源有争议就标争议（如 sudo），不编造词源；模拟器不支持的能力明确提示，不装成真的。
- **语言策略（v1.3 修订，永久基准）**：
  - **术语 English-first**：所有专业名词第一次出现给英文（executable、PATH、environment variable、repository），不翻译、不注音。
  - **教学解释中文为主**：正文讲解、mental model、类比、反馈信息以中文为主；英文用于术语、命令、代码、标题。
  - **不按 Level 数强加英文**：不再规划「Level N 之后正文全英文」的渐进英文化路线。当前强度（术语英文 + 解释中文）即永久基准，随用户阅读能力自然演进，不做硬切换。
  - Show Chinese 全局开关保留，作为按需加深理解的可选层。

## 12. Technical Architecture

- **纯静态**：HTML/CSS/JS/localStorage，无构建、无后端、无框架；单文件 `index.html` 即完整应用。
- **UI 渐进英文化**；正文 English-first + 按需 Show Chinese（v1.2 全局开关落地）。
- **代码分区**：第一部分 虚拟终端引擎（纯函数，不碰 DOM）；第二部分 课程内容数据（LEVELS / REVIEWS / ROADMAP）；第三部分 界面渲染与交互。
- **数据查找按 id 与数组位置解耦**：所有代码按 `level.id` 查找内容（REVIEWS、ROADMAP），允许数字 id 与小数段字符串 id（`8` / `"8.1"` / `"8.2"`）混排；进度按下标存储，两者互不干扰。
- **虚拟文件系统**：目录 = 对象，文件 = null；用户真实素材路径已埋入（`C:\Users\YOGA\...`），供未来 Module 回收。
- **隐藏调试入口**：Ctrl+Shift+D Debug Mode（跳关 / 重置），普通用户不可见。
- **测试**：Node 结构+引擎单测与 Edge 无头 E2E（自动通关整关）在每次内容变更后全绿才算交付。
