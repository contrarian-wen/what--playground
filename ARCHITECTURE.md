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
