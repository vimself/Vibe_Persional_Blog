# BlogSystem

一个简约大气的个人博客系统，纯前端实现，支持 Markdown 文档自动读取和渲染。

## 特性

- **纯前端架构**：无需后端服务器，静态文件部署
- **Markdown 自动读取**：上传 .md 文件到指定目录，前端自动扫描并渲染
- **精准代码高亮**：基于 Shiki 的语法高亮，支持明暗主题
- **静态搜索**：集成 Pagefind，零后端实现全文搜索
- **页面切换动画**：View Transitions API 提供流畅的页面过渡
- **简约大气设计**：遵循现代设计美学
- **响应式布局**：完美适配桌面端和移动端
- **快速加载**：优化性能，提供流畅阅读体验

## 设计理念

- **内容优先**：专注于文字内容的展示和阅读体验
- **简约美学**：干净、现代、专业的视觉设计
- **用户友好**：直观的导航和舒适的阅读环境
- **无干扰**：不包含评论、打赏等社交功能

## 技术栈

- JavaScript (ES2024+) + ES Modules
- CSS3 + Custom Properties + BEM
- marked.js + Shiki（Markdown 解析 + 代码高亮）
- Pagefind（静态搜索）
- View Transitions API（页面切换动画）

## 项目结构

```
BlogSystem/
├── index.html              # 主页面
├── 404.html                # 404 页面
├── css/
│   ├── variables.css       # CSS 变量定义
│   ├── base.css            # 基础样式
│   ├── layout.css          # 布局系统
│   ├── components.css      # 组件样式
│   ├── typography.css      # 排版样式
│   ├── code.css            # 代码块样式
│   └── responsive.css      # 响应式样式
├── js/
│   ├── app.js              # 应用入口
│   ├── modules/
│   │   ├── router.js       # 路由管理
│   │   ├── markdown.js     # Markdown 解析
│   │   ├── scanner.js      # 内容扫描
│   │   ├── theme.js        # 主题切换
│   │   └── search.js       # 搜索管理
│   ├── components/         # UI 组件
│   └── utils/              # 工具函数
├── content/
│   ├── index.json          # 文章索引
│   └── posts/              # Markdown 文章
├── assets/
│   ├── images/
│   ├── icons/
│   └── fonts/
└── docs/
    └── development-guide.md
```

## 快速开始

### 1. 克隆项目

```bash
git clone https://github.com/your-username/BlogSystem.git
cd BlogSystem
```

### 2. 添加博客文章

将 Markdown 文件上传到 `content/posts/` 目录，并更新 `content/index.json`：

```bash
# 示例：添加一篇新文章
cp your-article.md content/posts/my-first-post.md
```

### 3. 本地运行

```bash
# 使用 Node.js（推荐）
npx serve .

# 使用 Python
python -m http.server 8000

# 使用 live-server（自动刷新）
npx live-server --port=8000 --open=/
```

然后在浏览器中访问 `http://localhost:8000`

### 4. 生成搜索索引

```bash
npx pagefind --site .
```

## Markdown 文章格式

每篇文章需要包含 frontmatter 头部信息：

```markdown
---
title: 文章标题
date: 2026-05-19
description: 文章简介（可选）
tags: [标签1, 标签2]（可选）
draft: false（可选，默认 false）
---

# 文章正文

这里是文章内容...
```

## 设计规范

### 色彩方案

- **主色调**：中性灰色系
- **强调色**：柔和的靛蓝色（#6366f1）
- **背景色**：纯白或浅灰
- **文字色**：深灰或黑色
- **暗色主题**：自动跟随系统偏好，支持手动切换

### 字体选择

- **标题字体**：Inter、SF Pro Display
- **正文字体**：Inter、SF Pro Text
- **代码字体**：JetBrains Mono、Fira Code

### 间距系统

遵循 8px 网格系统：
- 基础间距：8px
- 常用间距：16px、24px、32px、48px
- 大间距：64px、96px

## 响应式断点

- **移动端**：< 768px
- **平板端**：768px - 1024px
- **桌面端**：> 1024px

## 部署

支持任意静态文件托管服务，推荐 Cloudflare Pages：

- **Cloudflare Pages**（推荐）：全球 CDN，边缘计算
- Vercel
- GitHub Pages
- Netlify

### 部署步骤

```bash
# 1. 压缩资源
npx clean-css-cli -o css/style.min.css css/*.css
npx uglify-js js/app.js -o js/app.min.js -c -m

# 2. 生成搜索索引
npx pagefind --site .

# 3. 推送到 Git 仓库
git add .
git commit -m "feat: 准备部署"
git push

# 4. 在部署平台配置
# - 构建命令：留空（纯静态）
# - 输出目录：/
```

## 贡献

欢迎提交 Issue 和 Pull Request！

## 许可证

MIT License

---

**注意**：本项目专注于内容展示，不包含评论系统和打赏功能。
