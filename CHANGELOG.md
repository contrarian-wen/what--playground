# Changelog · 变更记录

## v1.4 · 2026-09-27 — Level 8 Restructuring

### Level 8.0 Terminal Ecosystem（保留 + 增强）

- 标题改为 **Terminal Ecosystem (8.0)**，明确与 8.1 / 8.2 的递进关系。
- 新增 Step：终端生态**关系大图**（全景 concept，置于「骨架」concept 之后）。
- 新增 Step：**Unix / Linux / WSL 概念**（WSL 与 Git Bash 的本质区别）。
- 新增 Step：**What is it NOT** multiselect（Terminal ≠ Shell ≠ OS ≠ Program 边界辨析）。
- 现共 **24 个 Step**；Level 1–7 内容冻结未动。

### Level 8.1 Command Line Foundations（新关）

- 回答的认知问题：**"What am I talking to?"**
- 内容：打字时 terminal 只做 echo、Enter 才转交 shell；Window/Tab/Pane 容器 vs Shell/Process 住客；CMD / PowerShell / Bash 口音辨析；GUI ↔ Terminal 互证。
- **30 个 Step**，含 Review 页（concepts / commands / terms / mistakes / quiz）。

### Level 8.2 Command Line as a Language（新关）

- 回答的认知问题：**"How do I speak to it?"**
- 内容：命令的词源与「语言感」（诚实词源观，争议标注如 sudo）；command / subcommand / option / argument 四件套；引号；`code .` 与 Terminal 启动 GUI 程序；错误诊断（command not found / rm 拒删 / cat 文件夹）。
- **29 个 Step**、24 道互动题，难度从 pwd / cd / mkdir 递进到 git commit -m；含 3 个 terminal 实操任务（建目录、复制、毕业小考）。

### 虚拟终端引擎

- 新增命令：**touch / cat / cp / mv / rm（文件夹需 -r）**。
- `ls` 对 `-la` 类选项输出提示分支（模拟器暂不支持选项，但解释真实含义）。
- help 列表同步更新。

### 架构与文档

- 确立「**每个 Level 回答一个具体认知问题**」的内容设计原则（8.0 Where am I? / 8.1 What am I talking to? / 8.2 How do I speak to it?）。
- `README.md` 全量重写（项目定位、Current Status 表、文件说明、部署与进度）。
- `ARCHITECTURE.md` 重排为 12 节（原 9 节内容全部并入，新增 Current Status、Content Design Principles）。
- 新建 `CHANGELOG.md`（本文件）。
- 编号方案：`8.1` / `8.2` 作为独立 LEVELS 条目（字符串 id）插在 Level 8 之后；进度按下标存储，Level 0–8 已有进度零影响；全部代码按 `level.id` 查找，兼容数字/字符串 id 混排。

### Bug 修复

- **修复线上页面内容空白**（侧栏框架在、内容区全空）：电脑浏览器里残留的旧版/损坏进度数据（localStorage）会使渲染越界崩溃。启动加载现对进度做防御性钳制（越界 level/step、数组/ null completed 一律钳回有效范围），并加启动渲染 try/catch 最后防线——任何损坏数据都不再导致白屏。

### 测试

- Node 结构 + 引擎单测 574 断言全绿（每 Step 形状校验、新命令行为、全部 terminal 任务 validate 模拟、旧命令回归）。
- Edge 无头 E2E 自动通关 Level 8.0 / 8.1 / 8.2（含全部互动题型与 terminal 任务），三关均到达 Review 页。
- 修复试玩阻塞点：8.2「体验错误」任务与「第一句」任务共用文件夹名 test 导致后者无法完成——体验错误任务改用 `lab`。

## v1.3 · 2026-09-22 — Level 8 Terminal Ecosystem

- 新增 Level 8：Terminal Ecosystem（Terminal / Shell / OS / Program / Process 骨架、Windows Terminal 与四种 shell 辨析、Permission / Privilege / Elevation、五层诊断法），21 个 Step。
- 新增通用题型渲染器：multiselect / matching / ordering / fill / clickmap，供后续 Level 复用。
- 语言策略定为永久基准：术语 English-first、解释中文为主、不按 Level 数强加英文。

## v1.2 · 2026-09-19/20 — Module 结构落地

- 新增 Levels 4–7：PowerShell / CMD / Bash 辨析、命令语法（选项/参数/引号）、可执行文件与 PATH、环境变量。
- 侧栏 Module → Level 分组渲染（`module` 字段），打破线性课程假设。
- English-first 正文框架 + 全局 Show Chinese 开关。
- 长期架构约束记录（Module → Level → Step、六条访问规则、Progress/Access 分离）。

## v1.1 · 2026-09-19 — 试玩迭代

- 根据首轮试玩反馈修复交互与视觉问题（边框、滚动条色调统一等）。
- 项目接入 GitHub 仓库，确立增量迭代流程。

## v1.0 · 2026-09-18 — Initial Release

- 纯静态单文件应用（HTML/CSS/JS/localStorage）。
- Levels 0–3：操作对象、文件与文件夹、路径、terminal 初识。
- 虚拟终端引擎 v1：pwd / ls / cd / 基础 PATH 模拟。
