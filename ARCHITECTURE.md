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
- 虚拟终端引擎支撑「Try」：pwd / ls / cd / mkdir / touch / cat / cp / mv / rm / where / path / path-add / env / echo / notepad / code / explorer / git --version 等，操作虚拟文件系统，绝不碰真实电脑。

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
- 虚拟终端引擎已支持 touch / cat / cp / mv / rm（-r），ls 对选项给出提示分支。

## 10. Future Roadmap

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
  - 后续 Level 立项时先写下一句话问题，再排 Step。
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
