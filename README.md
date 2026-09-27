# Computer Fundamentals Playground · 计算机基础训练场

一个纯静态的交互式学习网页：Command Line、Git/GitHub 与计算机基础能力的渐进式训练场。

**零依赖**：只有 `HTML + CSS + JavaScript + localStorage`，没有构建步骤、没有后端、没有框架。双击即可离线学习，也可以部署到 GitHub Pages 手机访问。

## 项目定位

核心路径（mental model 主线，不是命令记忆课）：

```
Computer Literacy
→ Command Line（Terminal / Shell / PATH / 命令语言）
→ Git & GitHub
→ Linux / WSL
→ Development Environment
→ Real Programming Work
```

Command Line 与 Git/GitHub 是核心主线；知识之间互相连接，最终服务于真实的编程工作。

## Current Status · 当前进度

| Module | Levels | 状态 |
|---|---|---|
| A · File System & Paths 文件系统与路径 | 0–2 | ✅ Open |
| B · Terminal & Shell 终端与 Shell | 3–5 | ✅ Open |
| C · Programs, PATH & Terminal Ecosystem | 6, 7, 8.0, 8.1, 8.2, 8.3, 8.4 | ✅ Open（8.3 / 8.4 为最新内容） |
| D · Git & GitHub | 9–15 | 🔜 Coming soon |
| E · Development Environment（含 LaTeX 工具链） | 16–17 | 🔜 Coming soon |
| F · Network Basics 网络基础 | 18–19 | 🔜 Coming soon |
| G · Windows System Tools 系统工具 | 20–21 | 🔜 Coming soon |
| H · WSL & Linux Basics | 22 | 🔜 Coming soon |
| I · Troubleshooting & Final 排查与综合 | 23 | 🔜 Coming soon |

课程结构为 **Module → Level → Step**：Module / Level 自由进入，只有 Level 内部 Step 顺序解锁；Progress 只记录状态，不锁复习。

## 文件说明

| 文件 | 作用 |
|---|---|
| `index.html` | 课程主体（单文件应用，双击即可离线学习） |
| `ARCHITECTURE.md` | 长期架构约束（课程结构、访问规则、语言策略、AI Tutor 预留等） |
| `CHANGELOG.md` | 版本与内容变更记录 |
| `manifest.json` | PWA 最小配置（部署到 https 后支持「安装到桌面」） |
| `icon.png` | 应用图标 |
| `.nojekyll` | 告诉 GitHub Pages 按原样发布 |

## 本地运行

直接双击 `index.html`，用 Edge / Chrome 打开即可。

## 部署到 GitHub Pages（手机也能访问）

### 方法一：网页上传（推荐，不需要命令行）

1. 在 GitHub 右上角点 **＋ → New repository**
2. Repository name 填：`cf-playground`（随便起名），选 **Public**，勾选 **Add a README file**，点 **Create repository**
3. 进入新仓库 → **Add file → Upload files** → 把本文件夹的 **所有文件** 拖进去 → **Commit changes**
4. **Settings → Pages** → Source 选 **Deploy from a branch** → Branch 选 `main` / `(root)` → **Save**
5. 等 1～2 分钟，访问 `https://你的用户名.github.io/cf-playground/` ✅

### 方法二：git 命令行（顺便练习 Git）

```bash
git init
git add .
git commit -m "Initial release"
git branch -M main
git remote add origin https://github.com/你的用户名/cf-playground.git
git push -u origin main
```

然后同样到 **Settings → Pages** 开启。

## 学习进度

- 进度保存在浏览器 `localStorage`（按设备分开）
- 跨设备同步：网页侧栏 **Export** 导出 JSON → 另一台设备 **Import** 导入
- 部署到 https 之后，手机浏览器可以用「添加到主屏幕」获得接近 App 的体验
