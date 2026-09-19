# Computer Fundamentals Playground · 计算机基础训练场

一个纯静态的交互式学习网页：Command Line、Git/GitHub 与计算机基础能力的渐进式训练场。

**零依赖**：只有 `HTML + CSS + JavaScript + localStorage`，没有构建步骤、没有后端、没有框架。

## 文件说明

| 文件 | 作用 |
|---|---|
| `index.html` | 课程主体（单文件应用，双击即可离线学习） |
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
