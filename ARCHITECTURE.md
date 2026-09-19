# Long-term Architecture Constraints · 长期架构约束

> 记录于 v1.2。以下约束长期有效，增量开发时必须遵守；**不要**为提前实现它们而重写网站。

## 1. 课程结构：Module → Level → Step

最终形态不是 `Level 0 → 1 → 2 → …` 的线性课程，而是：

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

- 每个 Module 内部再分 Level，Level 内部再分 Step。
- 实现方式：`LEVELS` 数组中每个 level 带可选 `module` 字段，侧栏按 module 分组渲染。**不允许**把课程写死成 level_0→level_1→… 的线性假设（v1.2 已落地）。

## 2. 访问规则（六条，全部已实现）

1. Module 之间自由进入
2. Level 之间自由进入
3. 只有 Level 内部的 Steps 按顺序推进
4. Progress 与 Access 分离（进度不锁内容）
5. Progress 只记录学习状态，不限制复习
6. 用户可自由 Review 已完成内容

## 3. 核心主线

Command Line 与 Git/GitHub 是项目最核心的两个模块——**建立 mental model，不做命令记忆课**。
重点辨析：Terminal ≠ Shell、Shell ≠ OS、Command ≠ Program、Git ≠ GitHub。

## 4. 技术原则

- 保持纯静态（HTML/CSS/JS/localStorage），不引入后端与框架
- UI 渐进英文化；正文 English-first + 按需 Show Chinese（v1.2 全局开关落地）
- 交互闭环：Understand → Predict → Try → Make Mistake → Diagnose → Review → Ask
- Question Box 保持「引导优先」定位；AI backend 单独 Phase 实现
- 每次迭代不得破坏已有进度数据（schema 变更必须 migration/fallback）


## 5. 语言策略（v1.3 修订，永久基准）

- **术语 English-first**：所有专业名词第一次出现给英文（executable、PATH、environment variable、repository），不翻译、不注音。
- **教学解释中文为主**：正文讲解、mental model、类比、反馈信息以中文为主；英文用于术语、命令、代码、标题。
- **不按 Level 数强加英文**：不再规划「Level N 之后正文全英文」的渐进英文化路线。当前强度（术语英文 + 解释中文）即永久基准，随用户阅读能力自然演进，不做硬切换。
- Show Chinese 全局开关保留，作为按需加深理解的可选层。

## 6. LaTeX 的规划位置（Module 05 中段）

LaTeX 不单独成 Module，归入 **05 Development Environment 中段**，作为「编译器工具链」的真实案例教学：

- 先讲清链路：`.tex`（纯文本）→ compiler（`latex.exe`）→ `.dvi`/`.pdf`；`latexmk` 是自动化驱动器，用 Perl 写成，所以装 MiKTeX 时顺带了 Strawberry Perl——PATH 里因此多出两个文件夹。
- 再教 **LaTeX Basics** 最小集：`\documentclass`、`\begin{document}`、`\section`、行内/行间数学、简单矩阵、`aligned` 环境、希腊字母、`\frac`、中文方案（ctex）。
- 用户的真实素材（`简历.tex`、`LaTeX-Workshop-2026.zip`）在虚拟文件系统中已埋伏笔，Module 05 回收。
- 教学定位：LaTeX 是「编译器 + 环境变量 + PATH」mental model 的练兵场，不是排版艺术课。

## 7. Project Mode 设计（本次只记录，不实现）

关卡类型预留 `type: "project"`，与 learn 关卡并列。设计定稿：

- **流程**：MISSION 卡（Goal / Context / Requirements / Checkpoints）→ 学生声明 "I'm ready" → 在真实环境（或模拟器）完成 → "Check my work" 自检 → Socratic Project Assistant 用提问引导复盘（不接真 AI，先用规则式 mock）。
- **视觉语言**：Blue = Learn（现有关卡），Orange = Build（Project）。橙色用低饱和暖橙 `#c98f5f`，不用亮橙，保持现有深色 UI 质感。
- **难度递进**：首个 Project Mode 在 Module 03（Git & GitHub）完成后解锁，第一个项目是 **"Create your first Git repository"**；之后每个 Module 末尾挂一个小项目，用该 Module 刚学的工具解决一个小问题。
- Project Mode 不改变六条访问规则；Progress 照常记录，不锁复习。

## 8. 数据结构的扩展约束（Module → Level → Step）

- 现有 `LEVELS` 数组 + `module` 字段已满足 Module 分组；**不得**引入 `level_0 → level_1 → …` 的线性索引假设，所有代码按 `id` 与数组位置解耦查找。
- 新增内容类型（project / playground / challenge）以新 `type` 值渐进加入，不为未来类型预留空壳代码。
- 进度 schema（`store.completed[li] = 已完成步骤下标`）冻结；如需 per-step 状态扩展，新增并行字段并做 fallback，不改旧字段语义。
