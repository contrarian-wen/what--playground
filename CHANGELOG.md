# Changelog · 变更记录

## v1.5 · 2026-09-27 — Level 8.3 / 8.4：从「教过」到「学会」

### 背景：试玩反馈驱动的路线重排

- 用户完成 8.1 / 8.2 后反馈：8.2 虽然**介绍过** command / subcommand / option / argument，但实际遇到 `git commit -m "hello"` 仍拆不出结构——**教过 ≠ 学会**。
- 原定路线 8.3 = Command Line in Practice 的编号假设「8.2 已教会句法」，与事实不符。因此把句法独立成关、整体后移：
  - **8.3 = Command Syntax（新）**——补上真正的漏洞
  - **8.4 = Command Line in Practice**（原 8.3 内容）
  - **8.5 = Same Computer, Different Shells**（原 8.4，只设计未实现）
  - 8.6 Git Bash → 8.7 WSL / Ubuntu（更远期，未实现）

### Level 8.3 Command Syntax（新关）

- 回答的认知问题：**"How is a sentence built?"**
- 句法渐进主线：`pwd` → `cd Documents` → `ls -l` → `rm -r oldfolder` → `git commit -m "hello"`，一条比一条复杂，每步都拆 token。
- 核心机制：command vs program（谁执行）、option vs argument 双重判据（`-` 前缀 + 改变行为 vs 操作对象）、subcommand 工具箱直觉（git 是工具箱，commit 是格子）、`-m` 与 `"hello"` 的父子关系（option 领养 argument）、**pattern 不是法律**（`-m` 是约定俗成，查文档才知道）。
- 题型约 18 道：反向组装（给目的拼命令）、为什么题（为什么这么设计）、Unknown Command Challenge（陌生命令现场拆解）、fill / multiselect / matching 综合。
- **五步自问法**（遇到任何陌生命令的通用拆句法）作为本关可带走的工具。
- **28 个 Step**，毕业任务 `git init` → `git commit -m "Finish homework"`（引擎首次支持真实 commit 流程模拟）。

### Level 8.4 Command Line in Practice（新关）

- 回答的认知问题：**"Can I write one myself?"**——从「读懂结构」转向「亲手写出一条命令」。
- 10 个 terminal 实操任务（PowerShell 主线）：热身导航、`. / ..` 上楼、绝对路径直达、建练习场、文件三件套（touch/ls/cd）、引号建 `"my folder"`、通配符圈选 `*.txt`、**安全删除流程**（先 `ls *.txt` 确认再 `rm`）、`explorer .` GUI 召唤、毕业建家任务（mkdir → cd → touch → code . 四拍链）。
- 终端状态跨任务**累积**（后续任务依赖前面建过的目录），贴近真实使用。
- **30 个 Step**，约 20+ 道互动题。

### 虚拟终端引擎

- **引号感知分词**：成对引号内的空格不再切分参数，`result.quoted` 记录每个参数是否带引号（教学用）。
- **通配符**：`*` 和 `?` 支持（`ls *.txt`、`rm -r` 目录匹配等），glob 分支在路径存在性检查之前执行。
- **git 引擎重做**：`git init` 在 cwd 建 `.git` 节点；有 `.git` 时 `git commit -m` 模拟真实输出（`[main (root-commit) a1b2c3d] <msg>`）、`git status` 成功；无 `.git` 仍报 `fatal: not a git repository`（Level 6 旧题回归依赖此错误）。
- help 列表更新：引号、通配符、`git init/commit/status` 行。

### 架构与文档

- §11 新增内容设计原则：**「已介绍 ≠ 已学会」**——每个概念必须有独立的检验环节；试玩反馈优先于课程假设（编号与路线可为补漏洞而调整）。
- 路线图更新（见 ARCHITECTURE.md §10）：Bash 作为**第二阶段**逐渐引入——PowerShell 是当前唯一实操主线，8.5 只做 PowerShell↔Bash 对照观察（不要求会 Bash），8.6 Git Bash、8.7 WSL 更晚。目标：Command Line fundamentals → PowerShell → Bash basics → Git Bash → WSL / Ubuntu / Bash。

### 测试

- Node 结构 + 引擎单测 **480 断言全绿**（13 关结构、引号/通配符/git 引擎行为、8.3 毕业 validate、8.4 十任务**累积状态**模拟、8.0/8.2 旧任务回归）。
- Edge 无头 E2E 自动通关 **Level 8.0 / 8.1 / 8.2 / 8.3 / 8.4 五关**（含全部互动题型与 terminal 任务），五关均到达 Review 页。

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
