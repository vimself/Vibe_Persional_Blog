# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

BlogSystem 是一个纯前端个人博客系统，无需后端服务器，支持 Markdown 文档自动读取和渲染。

## 技术栈

- JavaScript (ES2024+) + ES Modules
- CSS3 + Custom Properties
- marked.js + Shiki（Markdown 解析 + 代码高亮）
- Pagefind（静态搜索）
- View Transitions API（页面切换动画）

## 常用命令

```bash
# 本地开发服务器（任选其一）
npx serve .
python -m http.server 8000
npx live-server --port=8000 --open=/

# 代码规范检查
npx eslint js/**/*.js
npx prettier --check .
npx stylelint css/**/*.css

# 资源压缩（部署前）
npx clean-css-cli -o css/style.min.css css/*.css
npx uglify-js js/app.js -o js/app.min.js -c -m

# 生成搜索索引（Pagefind）
npx pagefind --site .
```

## 架构设计

**核心模块**（js/modules/）：

- `Router` - 基于 History API 的路由管理，支持路由守卫
- `MarkdownParser` - 解析 Markdown 和 frontmatter，生成 HTML 和 TOC（使用 Shiki 代码高亮）
- `Scanner` - 通过 Fetch API 读取 content/index.json 索引文件获取文章列表
- `ThemeManager` - 明暗主题切换，支持系统偏好检测和 LocalStorage 持久化
- `SearchManager` - 集成 Pagefind 实现零后端静态搜索

**数据流**：

1. Scanner 从 `content/index.json` 获取文章索引
2. Router 匹配路由，触发对应的页面渲染
3. MarkdownParser 解析 `content/posts/*.md` 文件（包含 YAML frontmatter）
4. 组件渲染 HTML 到 DOM

**CSS 架构**：

- `variables.css` - CSS 变量定义（颜色、字号、间距、断点、阴影）
- `base.css` - 基础样式和 reset
- `layout.css` - 布局系统
- `components.css` - 组件样式（BEM 命名）
- `typography.css` - 排版样式
- `code.css` - 代码块样式
- `responsive.css` - 响应式断点（<768px 移动端、768-1024px 平板、>1024px 桌面）

## 编码规范

**JavaScript**：

- ES6+ 语法（class、箭头函数、async/await、解构、私有属性 `#`）
- 单引号字符串，语句末尾分号
- 变量/函数 camelCase，类名 PascalCase，常量 UPPER_SNAKE_CASE
- 使用 ES Modules 导出

**CSS**：

- BEM 命名：`.block__element--modifier`
- 使用 CSS 变量，禁止硬编码颜色/间距/字号
- 8px 网格系统

**文件命名**：kebab-case（如 `markdown-parser.js`）

**Git 提交**：我的远程仓库地址：https://github.com/vimself/Vibe_Persional_Blog.git

```
feat(scope): 新功能
fix(scope): 修复 bug
docs: 文档更新
style: 格式调整
refactor: 重构
perf: 性能优化
```

## 关键文件说明

- `content/index.json` - 文章索引文件，新增/删除文章时必须同步更新
- `content/posts/*.md` - 博客文章，需包含 YAML frontmatter（title、date、description、tags）

## 部署

推荐 Cloudflare Pages（全球 CDN，边缘计算），也支持 GitHub Pages、Vercel、Netlify 等静态托管。

部署前需生成搜索索引：`npx pagefind --site .`

Vercel 部署需配置 `vercel.json` 将所有路由重写到 `/index.html`。
