# Changelog · 变更记录

## v1.7 · 2026-10-02 — Level 8.6 / 8.7：Bash 与 WSL 实操落地 + 终端跨关重置修复

### Level 8.6 Git Bash（新关，32 Steps）

- 回答的认知问题：**"Can I actually use another shell?"**
- **训练量（按用户要求加大）**：32 个 Step、**23 个互动任务**（17 个 terminal 实操 + 3 fill + 3 predict）+ 复习页 4 道 quiz。
- 内容主线：
  - **引擎 bash 模式**：提示符变 MINGW64 样式（`YOGA@DESKTOP MINGW64 /c/...`）、路径接受 `/c/Users/...` 写法并自动翻译、错误显示 bash 口音（`bash: gitt: command not found`）、新增 `which` 命令；
  - **「翻译两步走」题型**：先 fill 把 PowerShell 命令翻译成 Bash，再在同一步亲手输入同一条命令——8.5 的对照表在这里变成手写输入；
  - 17 个实操任务覆盖：pwd / ls / mkdir / 故意打错看错误口音 / touch+rm / cd+touch+cp / mv 改名 / `ls *.txt` 通配符 / `ls n*` + 安全删除 / 带空格名字 / `/c/...` 绝对路径 / `cd ~` / which git / explorer . / code . / `rm -r` 清理；
  - **毕业考：Bash 四拍建家链**（cd ~ → mkdir bashtest → cd bashtest → touch readme.txt → code .）。

### Level 8.7 WSL（新关，28 Steps）

- 回答的认知问题：**"What changes when the shell is running inside Linux?"**
- **训练量**：28 个 Step、**18 个互动任务**（10 个 terminal 实操 + 4 predict + 3 multiselect + 1 matching + 1 fill）+ 复习页 4 道 quiz。
- 内容主线：
  - **引擎 wsl 模式**：独立 Linux 文件系统（家目录 `/home/yoga`，`~` 显示）、`/mnt/c` 与 Windows C 盘**双向桥接**（同一对象，两边可见）、PATH 不同（`which python3` → `/usr/bin/python3`）、apt 模拟（update / install）；
  - **「两个世界的桥」主线**：从 Linux 世界看 Windows（`cd /mnt/c/Users/YOGA`）、从桥上回家（`cd ~`）、反向召唤 Windows GUI（`explorer.exe .`）、开发者跨世界日常（`code .`）；
  - **辨析收口**：WSL ≠ Terminal ≠ Bash ≠ Ubuntu（8.0 五层模型收口）+ Git Bash vs WSL「方言相同、宿主不同」对照 multiselect；
  - **毕业考：跨世界六拍链**（mkdir project → cd project → touch main.py → touch notes.md → code . → explorer.exe .）。

### 终端引擎双模式（bash / wsl）

- `createTermState(shell)`：缺省 `ps`；`bash` = 同一棵 Windows 树 + MINGW64 home；`wsl` = 独立 Linux FS（home/yoga + mnt/c 挂载 Windows C 盘 + /usr/bin）。
- `displayPath`：ps → `C:\...`；bash → `/c/...`；wsl → `/...` 且 home 显示 `~`（pwd 输出、GUI 消息、拒绝消息统一走它）。
- `resolvePath`：`~` 展开为 home；bash 模式 `/c/...` → `C:\...`；wsl/bash 前导 `/` 视为绝对路径（修掉 `/mnt/c/...` 被当相对路径的 bug）。
- 新命令 `which`（bash 风格查 PATH）；`where` 仅限 PowerShell；`apt update/install` 仅 wsl 模式；未知命令按 shell 切换错误口音；`explorer.exe` 剥 `.exe` 后缀；cd 进 `C:\Windows` 的拒绝检查兼容 `/mnt/c/Windows`。
- `resetTerminal` 按 `LEVELS[store.level].shell` 自动切换模式。

### 真实 Bug 修复：终端跨关状态泄漏

- **症状（E2E 八关连跑暴露）**：从上一关 Review 页「Start Next Level」进入新关时，终端不重置——上一关的 cwd 和 shell 模式会带进去。实例：8.2 毕业考留在 `mylab` 目录 → 8.4 热身 `cd Documents` 直接迷路；8.5 → 8.6 终端仍是 PowerShell 模式，Bash 关卡无法用 Bash 通关。
- **修复**：`render()` 检测 `store.level` 变化时自动 `resetTerminal(true)`（`lastRenderedLevel` 追踪）。关内 Prev/Next 不触发重置，保留关内终端状态累积语义（8.4 / 8.6 / 8.7 依赖）。

### 测试

- Node 结构 + 引擎单测 **1191 断言全绿**（16 关结构、bash/wsl 引擎行为、8.6/8.7 毕业 validate 累计状态模拟、8.0–8.5 回归、ROADMAP 含 8.6/8.7、REVIEWS 五键齐全）。
- 引擎级脚本模拟：44 个终端任务命令序列全部通过真实 validate 函数。
- Edge 无头 E2E 自动通关 **Level 8.0–8.7 八关**（total=8、fails=0、八条 review reached）。

## v1.6 · 2026-10-02 — Level 8.5：跨方言观察与翻译

### Level 8.5 Same Computer, Different Shells（新关）

- 回答的认知问题：**"Why do shells speak different dialects?"**
- **本关契约：纯观察关卡，不要求会写 Bash**——训练目标是「读懂另一种 shell」的眼力，写 Bash 留到 8.6。
- **训练量（按用户要求加大）**：36 个 Step、**23 个互动题**（predict 9 / multiselect 6 / matching 2 / fill 5 / ordering 1）+ 复习页 4 道 quiz，共 27 道检验题。
- 内容主线：
  - **Alias 机制**：ls / dir / gci → Get-ChildItem，cat / type → Get-Content——「PowerShell 的 ls 是借词不是 Bash」这一关键去魅；
  - **核心对照表 + 8 连 matching**：Get-Location↔pwd、Get-ChildItem↔ls、Set-Location↔cd、Copy-Item↔cp、Move-Item↔mv、Remove-Item↔rm、Get-Content↔cat、New-Item↔touch（不对称点）；
  - **提示符认环境**：PS / 裸路径>（CMD）/ MINGW64（Git Bash）/ user@host:~$（WSL），matching 4 连 + 实战题；~ 作为跨方言同源词；
  - **两个世界**：Git Bash（Windows 程序，/c/Users 翻译腔）vs WSL（真 Linux，/home）——「方言相同，宿主不同」；
  - **option 品味差异**（长名 vs 短名连用）+ CMD 第三方言速览（/s 斜杠选项）；「整条句子判断方言，不看单个单词」陷阱题；
  - **三步翻译协议**（认 program → option 对号 → argument 照搬）作为本关毕业证书；
  - **双向翻译演练 5 题**：cp / rm -r / mv / cat（高仿词陷阱）/ touch（不对称点）；
  - **Unknown Command Challenge × 3**：grep -i "error" log.txt、python -m venv env（option 领养 argument 跨方言复刻）、tar -xvf（连用体拆解 + 事实/推断/不确定三分类）；
  - **跨方言五步自问法**（8.3 五步升级版：插入第 ② 步「先认方言」）排序毕业考。
- **REVIEWS["8.5"]** 五键齐全（concepts 8 / commands 8 / terms 8 / mistakes 6 / quiz 4）。

### 8.6 / 8.7 正式设计入档（只设计，未实现）

- ARCHITECTURE.md §10 落地 8.6 Git Bash（*Can I actually use another shell?*）与 8.7 WSL（*What changes when the shell is running inside Linux?*）的 learning objectives、concept map、prerequisite relationship、interaction design 与「不做清单」。
- §11 新增 **Principle 2：先建 mental model，再增词汇**（Teach the mental model before increasing command vocabulary）——与 v1.5 的 Introduced ≠ Learned（Principle 1）并列。

### 内容修订

- 修正 8.5 五道翻译 fill 题的答案数组：答案只留「空格处」的合法填法（如 touch 题填 `new-item` 而非整条 `New-Item new.txt`），避免误导性备选。

### 测试

- Node 结构 + 引擎单测 **1006 断言全绿**（14 关结构、8.5 每步形状、引擎引号/通配符/git 回归、ROADMAP 含 8.5）。
- Edge 无头 E2E 自动通关 **Level 8.0 / 8.1 / 8.2 / 8.3 / 8.4 / 8.5 六关**，全部到达 Review 页。

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
