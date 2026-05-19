# BlogSystem 开发文档

**项目名称**：BlogSystem — 个人博客系统
**文档版本**：v2.0
**最后更新**：2026-05-19
**技术栈**：Vanilla JS + ES Modules + CSS Custom Properties

---

## 一、技术选型

### 1.1 核心技术栈

| 层级 | 技术方案 | 版本 | 选型理由 |
|-----|---------|------|---------|
| 语言 | JavaScript (ES2024+) | ESNext | 原生支持，无框架依赖 |
| 模块系统 | ES Modules | 原生 | 浏览器原生支持，按需加载 |
| 样式 | CSS3 + Custom Properties | CSS3 | 原生变量、嵌套、容器查询 |
| Markdown | marked.js + Shiki | 最新 | 高性能解析 + 精准语法高亮 |
| 搜索 | Pagefind | 1.x | 零后端静态搜索，体积小 |
| 部署 | Cloudflare Pages / Vercel | - | 全球 CDN，边缘计算 |

### 1.2 不使用框架的理由

```
✅ 优势：
- 零依赖，加载速度快
- 构建简单，无需打包工具
- 完全控制，易于定制
- 学习成本低，便于维护

⚠️ 适用场景：
- 内容为主的博客
- 个人项目
- 追求极致性能
```

---

## 二、项目架构

### 2.1 目录结构

```
BlogSystem/
├── index.html                    # SPA 入口
├── 404.html                      # 404 页面
├── favicon.svg                   # 网站图标
├── robots.txt                    # 爬虫配置
│
├── css/
│   ├── variables.css             # CSS 变量（颜色、字号、间距）
│   ├── base.css                  # Reset + 基础样式
│   ├── layout.css                # 布局系统
│   ├── components.css            # 组件样式（BEM）
│   ├── typography.css            # 排版样式
│   ├── code.css                  # 代码块样式
│   └── responsive.css            # 响应式断点
│
├── js/
│   ├── app.js                    # 应用入口
│   ├── modules/
│   │   ├── router.js             # 路由管理
│   │   ├── markdown.js           # Markdown 解析
│   │   ├── scanner.js            # 内容扫描
│   │   ├── theme.js              # 主题切换
│   │   └── search.js             # 搜索管理
│   ├── components/
│   │   ├── header.js             # 头部导航
│   │   ├── post-card.js          # 文章卡片
│   │   ├── post-detail.js        # 文章详情
│   │   ├── toc.js                # 目录导航
│   │   └── back-to-top.js        # 返回顶部
│   └── utils/
│       ├── dom.js                # DOM 工具
│       ├── date.js               # 日期格式化
│       └── lazy-load.js          # 懒加载
│
├── content/
│   ├── index.json                # 文章索引
│   └── posts/
│       └── *.md                  # Markdown 文章
│
├── assets/
│   ├── images/                   # 图片资源
│   ├── icons/                    # 图标
│   └── fonts/                    # 字体文件
│
└── docs/
    └── development-guide.md      # 本文档
```

### 2.2 数据流

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  index.json │───>│   Scanner   │───>│    Router    │
│  (文章索引)  │    │  (读取索引)  │    │  (路由匹配)  │
└─────────────┘    └─────────────┘    └─────────────┘
                           │                  │
                           v                  v
                   ┌─────────────┐    ┌─────────────┐
                   │   Markdown  │───>│   Renderer  │
                   │   (解析MD)  │    │  (渲染DOM)  │
                   └─────────────┘    └─────────────┘
```

---

## 三、开发环境

### 3.1 环境要求

| 工具 | 最低版本 | 推荐版本 | 说明 |
|-----|---------|---------|------|
| Node.js | 18.x | 20.x LTS | 本地开发服务器 |
| npm | 9.x | 10.x | 包管理器 |
| Git | 2.x | 最新版 | 版本控制 |
| VS Code | 最新版 | 最新版 | 推荐编辑器 |

### 3.2 VS Code 扩展

```
必装：
- ESLint          - JS 代码检查
- Prettier        - 代码格式化
- Stylelint       - CSS 代码检查
- Live Server     - 本地开发服务器

推荐：
- Markdown All in One   - Markdown 编辑
- Color Highlight       - CSS 颜色预览
- IntelliSense for CSS  - CSS 类名提示
- Error Lens            - 内联错误显示
```

### 3.3 快速开始

```bash
# 1. 克隆项目
git clone https://github.com/your-username/BlogSystem.git
cd BlogSystem

# 2. 启动开发服务器（任选其一）
npx serve .                    # 零配置
npx live-server --port=8000    # 自动刷新
python -m http.server 8000     # Python 内置

# 3. 访问 http://localhost:8000
```

---

## 四、编码规范

### 4.1 HTML 规范

```html
<!-- ✅ 语义化标签 -->
<header class="header">
  <nav class="header__nav" aria-label="主导航">
    <a href="/" class="header__logo">BlogSystem</a>
  </nav>
</header>

<main class="main" id="main-content">
  <article class="post">
    <h1 class="post__title">文章标题</h1>
    <time class="post__date" datetime="2026-05-19">2026年5月19日</time>
  </article>
</main>

<!-- ❌ 避免无语义标签 -->
<div class="header">
  <div class="nav">...</div>
</div>
```

**要点**：
- 使用语义化标签（header、nav、main、article、section、footer）
- 图片必须有 alt 属性
- 链接必须有明确文字
- 使用 aria-label 增强无障碍性

### 4.2 CSS 规范

```css
/* ✅ BEM 命名 + CSS 变量 */
.post-card {
  background-color: var(--color-bg-primary);
  padding: var(--space-6);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-sm);
}

.post-card__title {
  font-size: var(--text-xl);
  color: var(--color-text-primary);
}

.post-card--featured {
  border-left: 3px solid var(--color-accent);
}

/* ❌ 避免硬编码 */
.post-card {
  background-color: #ffffff;
  padding: 24px;
}
```

**要点**：
- BEM 命名：`.block__element--modifier`
- 使用 CSS 变量，禁止硬编码颜色/间距/字号
- 8px 网格系统
- 使用 Flexbox 或 Grid 布局

### 4.3 JavaScript 规范

```javascript
// ✅ ES6+ 语法
class MarkdownParser {
  #cache = new Map();  // 私有属性

  async parse(rawText) {
    try {
      const { frontmatter, content } = this.#extractFrontmatter(rawText);
      return { metadata: this.#parseYaml(frontmatter), html: this.#render(content) };
    } catch (error) {
      console.error('解析失败:', error);
      throw error;
    }
  }
}

// ✅ 使用 async/await
async function loadPost(slug) {
  const response = await fetch(`content/posts/${slug}.md`);
  if (!response.ok) throw new Error(`文章不存在: ${slug}`);
  return response.text();
}

// ❌ 避免回调
function loadPost(slug, callback) {
  fetch(`content/posts/${slug}.md`)
    .then(response => response.text())
    .then(text => callback(null, text))
    .catch(error => callback(error));
}
```

**要点**：
- 使用 ES6+ 语法（class、箭头函数、解构、模板字符串）
- 使用 async/await 处理异步
- 使用 const 和 let，避免 var
- 单引号字符串，语句末尾分号

### 4.4 命名规范

```
文件名      → kebab-case      post-card.js
目录名      → kebab-case      js/modules/
CSS 类名    → BEM             .post-card__title--featured
JS 变量/函数 → camelCase       getPostIndex
JS 类名     → PascalCase      MarkdownParser
常量        → UPPER_SNAKE     POSTS_PER_PAGE
```

---

## 五、核心模块实现

### 5.1 路由管理 (Router)

基于 History API 的 SPA 路由：

```javascript
// js/modules/router.js
export class Router {
  #routes = [];
  #beforeGuards = [];
  #afterHooks = [];
  #currentRoute = null;

  register(pattern, handler) {
    const paramNames = [];
    const regex = pattern.replace(/:(\w+)/g, (_, name) => {
      paramNames.push(name);
      return '([^/]+)';
    });

    this.#routes.push({
      pattern,
      regex: new RegExp(`^${regex}$`),
      paramNames,
      handler,
    });
  }

  navigate(path, options = {}) {
    if (options.replace) {
      history.replaceState(null, '', path);
    } else {
      history.pushState(null, '', path);
    }
    this.#resolve(path);
  }

  #resolve(path) {
    const [pathname, search] = path.split('?');
    const query = Object.fromEntries(new URLSearchParams(search));

    for (const route of this.#routes) {
      const match = pathname.match(route.regex);
      if (match) {
        const params = {};
        route.paramNames.forEach((name, index) => {
          params[name] = match[index + 1];
        });

        const from = this.#currentRoute;
        const to = { path: pathname, params, query };

        // 执行前置守卫
        for (const guard of this.#beforeGuards) {
          const result = guard(to, from);
          if (result === false) return;
          if (typeof result === 'string') {
            this.navigate(result, { replace: true });
            return;
          }
        }

        this.#currentRoute = to;
        route.handler(params, query);

        // 执行后置钩子
        for (const hook of this.#afterHooks) {
          hook(to, from);
        }
        return;
      }
    }

    // 404 处理
    this.#currentRoute = { path: pathname, params: {}, query };
    this.#handle404();
  }

  beforeEach(guard) {
    this.#beforeGuards.push(guard);
  }

  afterEach(hook) {
    this.#afterHooks.push(hook);
  }

  getCurrentRoute() {
    return this.#currentRoute;
  }

  #handle404() {
    if (window.renderer) {
      window.renderer.render404();
    }
  }

  start() {
    // 监听 popstate
    window.addEventListener('popstate', () => {
      this.#resolve(window.location.pathname + window.location.search);
    });

    // 拦截链接点击
    document.addEventListener('click', (event) => {
      const link = event.target.closest('a');
      if (link && link.href.startsWith(window.location.origin) && !link.hasAttribute('data-external')) {
        event.preventDefault();
        this.navigate(link.pathname + link.search);
      }
    });

    // 解析当前路径
    this.#resolve(window.location.pathname + window.location.search);
  }
}
```

### 5.2 Markdown 解析 (MarkdownParser)

支持 Frontmatter、代码高亮、TOC 生成：

```javascript
// js/modules/markdown.js
import { marked } from 'marked';
import { createHighlighter } from 'shiki';

export class MarkdownParser {
  #highlighter = null;
  #cache = new Map();

  async init() {
    this.#highlighter = await createHighlighter({
      themes: ['github-dark', 'github-light'],
      langs: ['javascript', 'typescript', 'css', 'html', 'bash', 'json', 'markdown'],
    });
  }

  async parse(rawText) {
    if (this.#cache.has(rawText)) {
      return this.#cache.get(rawText);
    }

    const { frontmatter, content } = this.#extractFrontmatter(rawText);
    const metadata = frontmatter ? this.#parseYaml(frontmatter) : {};
    const html = await this.#renderToHtml(content);
    const toc = this.#generateToc(html);
    const readingTime = this.#calculateReadingTime(content);

    const result = {
      metadata: {
        title: metadata.title || '未知标题',
        date: metadata.date || new Date().toISOString().split('T')[0],
        description: metadata.description || '',
        tags: metadata.tags || [],
        draft: metadata.draft || false,
        ...metadata,
      },
      html,
      toc,
      readingTime,
    };

    this.#cache.set(rawText, result);
    return result;
  }

  #extractFrontmatter(rawText) {
    const match = rawText.match(/^---\n([\s\S]*?)\n---\n([\s\S]*)$/);
    if (!match) return { frontmatter: null, content: rawText };
    return { frontmatter: match[1], content: match[2] };
  }

  #parseYaml(yamlText) {
    // 简单的 YAML 解析，或使用 js-yaml 库
    try {
      return jsyaml.load(yamlText);
    } catch {
      console.warn('Frontmatter 解析失败');
      return {};
    }
  }

  async #renderToHtml(markdown) {
    const renderer = new marked.Renderer();

    // 自定义标题渲染（生成 ID）
    renderer.heading = ({ text, depth }) => {
      const slug = text.toLowerCase().replace(/\s+/g, '-').replace(/[^\w-]/g, '');
      return `<h${depth} id="${slug}">${text}</h${depth}>`;
    };

    // 自定义链接渲染（外部链接新窗口打开）
    renderer.link = ({ href, title, text }) => {
      const isExternal = href.startsWith('http');
      const attrs = isExternal ? ' target="_blank" rel="noopener noreferrer"' : '';
      return `<a href="${href}"${title ? ` title="${title}"` : ''}${attrs}>${text}</a>`;
    };

    // 自定义图片渲染（懒加载）
    renderer.image = ({ href, title, text }) => {
      return `<figure>
        <img src="${href}" alt="${text}" loading="lazy" decoding="async" />
        ${title ? `<figcaption>${title}</figcaption>` : ''}
      </figure>`;
    };

    // 代码高亮
    renderer.code = ({ text, lang }) => {
      if (this.#highlighter && lang) {
        return this.#highlighter.codeToHtml(text, {
          lang,
          themes: { light: 'github-light', dark: 'github-dark' },
        });
      }
      return `<pre><code class="language-${lang || 'text'}">${text}</code></pre>`;
    };

    return marked.parse(markdown, { renderer });
  }

  #generateToc(html) {
    const headings = [];
    const regex = /<h([1-6]) id="([^"]*)">(.*?)<\/h[1-6]>/g;
    let match;

    while ((match = regex.exec(html)) !== null) {
      headings.push({
        level: parseInt(match[1]),
        id: match[2],
        text: match[3],
      });
    }

    return headings;
  }

  #calculateReadingTime(text) {
    const wordsPerMinute = 300; // 中文阅读速度
    const words = text.trim().length;
    return Math.ceil(words / wordsPerMinute);
  }
}
```

### 5.3 内容扫描 (Scanner)

```javascript
// js/modules/scanner.js
export class Scanner {
  #contentDir;
  #indexFile;
  #cache = new Map();

  constructor(config = {}) {
    this.#contentDir = config.contentDir || 'content';
    this.#indexFile = config.indexFile || 'index.json';
  }

  async getPostIndex(options = {}) {
    const cacheKey = `index:${JSON.stringify(options)}`;

    if (this.#cache.has(cacheKey)) {
      return this.#cache.get(cacheKey);
    }

    try {
      const response = await fetch(`${this.#contentDir}/${this.#indexFile}`);
      const data = await response.json();

      let posts = data.posts.filter((post) => !post.draft);

      // 标签筛选
      if (options.tag) {
        posts = posts.filter((post) => post.tags?.includes(options.tag));
      }

      // 搜索筛选
      if (options.search) {
        const query = options.search.toLowerCase();
        posts = posts.filter(
          (post) =>
            post.title.toLowerCase().includes(query) ||
            post.description?.toLowerCase().includes(query) ||
            post.tags?.some((tag) => tag.toLowerCase().includes(query))
        );
      }

      // 分页
      const page = options.page || 1;
      const pageSize = options.pageSize || 10;
      const start = (page - 1) * pageSize;
      const end = start + pageSize;

      const result = {
        posts: posts.slice(start, end),
        total: posts.length,
        page,
        pageSize,
        totalPages: Math.ceil(posts.length / pageSize),
      };

      this.#cache.set(cacheKey, result);
      return result;
    } catch (error) {
      console.error('获取文章索引失败:', error);
      throw error;
    }
  }

  async getPostContent(slug) {
    if (this.#cache.has(`post:${slug}`)) {
      return this.#cache.get(`post:${slug}`);
    }

    try {
      const response = await fetch(`${this.#contentDir}/posts/${slug}.md`);
      if (!response.ok) throw new Error(`文章不存在: ${slug}`);
      const content = await response.text();
      this.#cache.set(`post:${slug}`, content);
      return content;
    } catch (error) {
      console.error('获取文章内容失败:', error);
      throw error;
    }
  }

  clearCache() {
    this.#cache.clear();
  }
}
```

### 5.4 主题管理 (ThemeManager)

```javascript
// js/modules/theme.js
export class ThemeManager {
  #storageKey;
  #defaultTheme;
  #callbacks = [];
  #currentTheme = null;

  constructor(config = {}) {
    this.#storageKey = config.storageKey || 'theme';
    this.#defaultTheme = config.defaultTheme || 'system';
  }

  init() {
    const saved = localStorage.getItem(this.#storageKey);
    const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;

    let theme;
    if (saved) {
      theme = saved;
    } else if (this.#defaultTheme === 'system') {
      theme = prefersDark ? 'dark' : 'light';
    } else {
      theme = this.#defaultTheme;
    }

    this.#applyTheme(theme);
    this.#watchSystemTheme();
  }

  #applyTheme(theme) {
    document.documentElement.setAttribute('data-theme', theme);
    this.#currentTheme = theme;
    this.#callbacks.forEach((cb) => cb(theme));
  }

  getCurrentTheme() {
    return this.#currentTheme;
  }

  setTheme(theme) {
    if (theme === 'system') {
      const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
      theme = prefersDark ? 'dark' : 'light';
      localStorage.removeItem(this.#storageKey);
    } else {
      localStorage.setItem(this.#storageKey, theme);
    }
    this.#applyTheme(theme);
  }

  toggle() {
    const next = this.#currentTheme === 'dark' ? 'light' : 'dark';
    this.setTheme(next);
  }

  #watchSystemTheme() {
    window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', (e) => {
      if (!localStorage.getItem(this.#storageKey)) {
        this.#applyTheme(e.matches ? 'dark' : 'light');
      }
    });
  }

  onChange(callback) {
    this.#callbacks.push(callback);
  }
}
```

### 5.5 搜索管理 (SearchManager)

集成 Pagefind 实现零后端静态搜索：

```javascript
// js/modules/search.js
export class SearchManager {
  #pagefind = null;
  #initialized = false;

  async init() {
    try {
      // Pagefind 在构建时生成索引
      this.#pagefind = await import('/pagefind/pagefind.js');
      await this.#pagefind.init();
      this.#initialized = true;
    } catch (error) {
      console.warn('Pagefind 初始化失败，搜索功能不可用:', error);
    }
  }

  async search(query, options = {}) {
    if (!this.#initialized) {
      console.warn('搜索未初始化');
      return [];
    }

    try {
      const search = await this.#pagefind.debouncedSearch(query, options);
      const results = await Promise.all(
        search.results.slice(0, options.limit || 10).map((r) => r.data())
      );

      return results.map((result) => ({
        title: result.meta?.title || result.url,
        url: result.url,
        excerpt: result.excerpt,
        description: result.meta?.description,
        date: result.meta?.date,
        tags: result.meta?.tags?.split(',') || [],
      }));
    } catch (error) {
      console.error('搜索失败:', error);
      return [];
    }
  }

  isAvailable() {
    return this.#initialized;
  }
}
```

---

## 六、CSS 架构

### 6.1 CSS 变量系统

```css
/* css/variables.css */
:root {
  /* 颜色系统 */
  --color-bg-primary: #ffffff;
  --color-bg-secondary: #f8fafc;
  --color-bg-tertiary: #f1f5f9;
  --color-text-primary: #0f172a;
  --color-text-secondary: #475569;
  --color-text-muted: #94a3b8;
  --color-accent: #6366f1;
  --color-accent-hover: #4f46e5;
  --color-border: #e2e8f0;
  --color-code-bg: #f1f5f9;

  /* 字号系统 */
  --text-xs: 0.75rem;
  --text-sm: 0.875rem;
  --text-base: 1rem;
  --text-lg: 1.125rem;
  --text-xl: 1.25rem;
  --text-2xl: 1.5rem;
  --text-3xl: 1.875rem;
  --text-4xl: 2.25rem;

  /* 间距系统（8px 网格） */
  --space-1: 0.25rem;
  --space-2: 0.5rem;
  --space-3: 0.75rem;
  --space-4: 1rem;
  --space-5: 1.25rem;
  --space-6: 1.5rem;
  --space-8: 2rem;
  --space-10: 2.5rem;
  --space-12: 3rem;
  --space-16: 4rem;

  /* 圆角 */
  --radius-sm: 0.25rem;
  --radius-md: 0.5rem;
  --radius-lg: 0.75rem;
  --radius-xl: 1rem;

  /* 阴影 */
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1);

  /* 布局 */
  --content-max-width: 720px;
  --sidebar-width: 280px;
  --header-height: 64px;

  /* 字体 */
  --font-sans: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  --font-mono: 'JetBrains Mono', 'Fira Code', monospace;

  /* 过渡 */
  --transition-fast: 150ms ease;
  --transition-normal: 250ms ease;
  --transition-slow: 350ms ease;
}

/* 暗色主题 */
[data-theme='dark'] {
  --color-bg-primary: #0f172a;
  --color-bg-secondary: #1e293b;
  --color-bg-tertiary: #334155;
  --color-text-primary: #f1f5f9;
  --color-text-secondary: #94a3b8;
  --color-text-muted: #64748b;
  --color-accent: #818cf8;
  --color-accent-hover: #6366f1;
  --color-border: #334155;
  --color-code-bg: #1e293b;
}
```

### 6.2 响应式断点

```css
/* css/responsive.css */
/* 移动端：< 768px */
@media (max-width: 767px) {
  :root {
    --page-padding-x: var(--space-4);
    --content-max-width: 100%;
  }

  .header__nav {
    display: none;
  }

  .header__menu-btn {
    display: flex;
  }

  .sidebar {
    display: none;
  }
}

/* 平板端：768px - 1024px */
@media (min-width: 768px) and (max-width: 1023px) {
  :root {
    --page-padding-x: var(--space-6);
    --content-max-width: 640px;
  }
}

/* 桌面端：>= 1024px */
@media (min-width: 1024px) {
  :root {
    --page-padding-x: var(--space-8);
    --content-max-width: 720px;
  }

  .header__menu-btn {
    display: none;
  }
}
```

### 6.3 View Transitions（页面切换动画）

```css
/* 页面切换动画 */
@view-transition {
  navigation: auto;
}

::view-transition-old(root) {
  animation: fade-out 0.2s ease-in;
}

::view-transition-new(root) {
  animation: fade-in 0.2s ease-out;
}

@keyframes fade-out {
  from { opacity: 1; }
  to { opacity: 0; }
}

@keyframes fade-in {
  from { opacity: 0; }
  to { opacity: 1; }
}
```

---

## 七、开发流程

### 7.1 阶段规划

| 阶段 | 时间 | 主要任务 | 里程碑 |
|-----|------|---------|-------|
| 第一阶段 | 2 天 | 基础架构 | 项目可运行，页面框架完成 |
| 第二阶段 | 4 天 | 核心模块 | 路由、Markdown 解析可用 |
| 第三阶段 | 4 天 | 页面组件 | 所有页面组件完成 |
| 第四阶段 | 3 天 | 功能增强 | 搜索、主题切换、性能优化 |
| 第五阶段 | 2 天 | 部署上线 | 成功部署并验证 |

### 7.2 第一阶段：基础架构（2 天）

**Day 1：HTML + CSS 基础**
- [ ] 创建 index.html（语义化结构）
- [ ] 创建 variables.css（CSS 变量系统）
- [ ] 创建 base.css（Reset + 基础样式）
- [ ] 引入字体（Inter、JetBrains Mono）

**Day 2：布局系统**
- [ ] 创建 layout.css（Header、Main、Footer）
- [ ] 创建 responsive.css（响应式断点）
- [ ] 创建 animations.css（过渡动画）
- [ ] 测试本地服务器

### 7.3 第二阶段：核心模块（4 天）

**Day 3-4：路由模块**
- [ ] 实现 Router 类
- [ ] 实现路由注册和匹配
- [ ] 实现 History API 监听
- [ ] 实现路由守卫

**Day 5-6：Markdown 模块**
- [ ] 实现 MarkdownParser 类
- [ ] 实现 Frontmatter 解析
- [ ] 实现代码高亮（Shiki）
- [ ] 实现 TOC 生成

### 7.4 第三阶段：页面组件（4 天）

**Day 7-8：首页组件**
- [ ] Header 导航组件
- [ ] PostCard 文章卡片
- [ ] TagFilter 标签筛选
- [ ] Pagination 分页

**Day 9-10：文章详情**
- [ ] PostDetail 文章详情
- [ ] PostMeta 元信息
- [ ] TOC 目录导航
- [ ] PostNavigation 文章导航

### 7.5 第四阶段：功能增强（3 天）

**Day 11：搜索功能**
- [ ] 集成 Pagefind
- [ ] 实现 SearchManager
- [ ] 创建搜索 UI

**Day 12：主题切换**
- [ ] 实现 ThemeManager
- [ ] 明暗主题切换
- [ ] 系统偏好检测

**Day 13：性能优化**
- [ ] 图片懒加载
- [ ] 骨架屏加载
- [ ] 资源预加载

### 7.6 第五阶段：部署上线（2 天）

**Day 14：部署准备**
- [ ] 资源压缩
- [ ] 图片优化
- [ ] 创建 robots.txt
- [ ] 配置 favicon

**Day 15：部署上线**
- [ ] 选择部署平台
- [ ] 配置部署参数
- [ ] 执行部署
- [ ] 验证功能

---

## 八、调试与测试

### 8.1 调试技巧

```javascript
// 控制台调试
console.log('调试信息:', data);
console.table(arrayData);
console.group('分组');
console.time('计时器');

// 断点调试
debugger;

// 路由调试
console.log('当前路由:', window.router.getCurrentRoute());

// Markdown 调试
const raw = await fetch('content/posts/test.md').then(r => r.text());
const result = await parser.parse(raw);
console.log('解析结果:', result);

// 主题调试
console.log('当前主题:', document.documentElement.getAttribute('data-theme'));
```

### 8.2 功能测试矩阵

| 功能 | 测试项 | 预期结果 |
|-----|-------|---------|
| 路由 | 点击链接 | URL 变化，页面更新 |
| 路由 | 前进/后退 | 正确响应 |
| 路由 | 直接访问 URL | 正确渲染 |
| 路由 | 不存在路径 | 显示 404 |
| Markdown | 加载文章 | 正确渲染 |
| Markdown | Frontmatter | 正确解析 |
| Markdown | 代码块 | 正确高亮 |
| 主题 | 切换主题 | 颜色变化 |
| 主题 | 刷新页面 | 主题保持 |
| 主题 | 系统偏好 | 自动跟随 |
| 搜索 | 关键词搜索 | 返回结果 |
| 响应式 | 移动端 | 布局正确 |
| 响应式 | 桌面端 | 布局正确 |

### 8.3 性能指标

| 指标 | 目标值 | 说明 |
|-----|-------|------|
| FCP | < 1.0s | 首次内容绘制 |
| LCP | < 2.0s | 最大内容绘制 |
| CLS | < 0.1 | 累积布局偏移 |
| INP | < 200ms | 交互延迟 |
| 总资源 | < 200KB | 不含图片 |

---

## 九、部署指南

### 9.1 部署前检查

```
□ 所有功能测试通过
□ 响应式布局正常
□ 明暗主题切换正常
□ 404 页面配置正确
□ favicon 配置正确
□ robots.txt 配置正确
□ CSS/JS 文件已压缩
□ 图片已优化
□ 搜索索引已生成
```

### 9.2 资源压缩

```bash
# 安装工具
npm install -g clean-css-cli uglify-js

# 压缩 CSS
cleancss -o css/style.min.css css/*.css

# 压缩 JS
uglifyjs js/app.js -o js/app.min.js -c -m

# 生成搜索索引（Pagefind）
npx pagefind --site .
```

### 9.3 Cloudflare Pages 部署

```bash
# 1. 推送代码到 GitHub
git add .
git commit -m "feat: 准备部署"
git push

# 2. 在 Cloudflare Dashboard
# - 连接 GitHub 仓库
# - 构建命令：留空（纯静态）
# - 输出目录：/
# - 环境变量：无需配置

# 3. 自动部署
# 每次 push 自动触发部署
```

### 9.4 Vercel 部署

```bash
# 1. 安装 Vercel CLI
npm install -g vercel

# 2. 登录
vercel login

# 3. 部署
vercel

# 4. 配置 vercel.json
{
  "version": 2,
  "builds": [
    {
      "src": "**/*",
      "use": "@vercel/static"
    }
  ],
  "routes": [
    { "handle": "filesystem" },
    { "src": "/(.*)", "dest": "/index.html" }
  ]
}
```

### 9.5 GitHub Pages 部署

```bash
# 1. 推送代码
git push origin main

# 2. 在 GitHub 仓库设置
# Settings > Pages
# Source: Deploy from a branch
# Branch: main / (root)

# 3. 访问
# https://username.github.io/BlogSystem/
```

### 9.6 部署后验证

```
□ 网站可正常访问
□ 所有页面可打开
□ 路由切换正常
□ Markdown 文章正常显示
□ 主题切换正常
□ 搜索功能正常
□ 响应式布局正常
□ HTTPS 正常工作
□ 404 页面正常显示
```

---

## 十、维护与更新

### 10.1 添加新文章

```bash
# 1. 创建 Markdown 文件
# content/posts/2026-05-19-new-article.md

---
title: "新文章标题"
date: 2026-05-19
description: "文章摘要"
tags: [标签1, 标签2]
---

# 文章正文

这里是文章内容...

# 2. 更新索引文件
# 编辑 content/index.json，添加新文章信息

# 3. 重新生成搜索索引
npx pagefind --site .

# 4. 提交更新
git add .
git commit -m "feat: 添加新文章《新文章标题》"
git push
```

### 10.2 自定义样式

```css
/* 修改主题颜色 */
:root {
  --color-accent: #6366F1;  /* 主色调 */
  --color-bg-primary: #ffffff;  /* 背景色 */
  --color-text-primary: #0f172a;  /* 文字颜色 */
}

/* 修改字体 */
:root {
  --font-sans: 'Your Font', sans-serif;
  --font-mono: 'Your Mono Font', monospace;
}

/* 修改内容宽度 */
:root {
  --content-max-width: 800px;
}
```

### 10.3 Git 提交规范

```
格式：<type>(<scope>): <subject>

类型：
feat:     新功能
fix:      修复 bug
docs:     文档更新
style:    格式调整
refactor: 重构
perf:     性能优化
test:     测试
chore:    构建/工具

示例：
git commit -m "feat(routing): 添加路由守卫"
git commit -m "fix(markdown): 修复代码高亮"
git commit -m "docs(readme): 更新部署说明"
```

---

## 附录

### A. 常见问题

**Q: Markdown 文件不显示？**
A: 检查：
1. 文件在 `content/posts/` 目录下
2. 文件名格式正确（建议 `YYYY-MM-DD-slug.md`）
3. `content/index.json` 包含该文件信息
4. 文件编码为 UTF-8

**Q: 主题切换不生效？**
A: 检查：
1. CSS 变量正确定义
2. `[data-theme="dark"]` 选择器正确
3. LocalStorage 正常工作

**Q: 搜索功能不可用？**
A: 检查：
1. Pagefind 索引已生成（`npx pagefind --site .`）
2. 浏览器控制台无报错
3. 网络请求正常

### B. 参考资源

- [MDN Web Docs](https://developer.mozilla.org/)
- [CSS Variables 指南](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Using_CSS_custom_properties)
- [marked.js 文档](https://marked.js.org/)
- [Shiki 文档](https://shiki.mrsuu.me/)
- [Pagefind 文档](https://pagefind.app/)
- [View Transitions API](https://developer.mozilla.org/en-US/docs/Web/API/View_Transitions_API)

### C. 更新日志

**v2.0 (2026-05-19)**
- 重新设计技术架构
- 集成 Pagefind 搜索
- 支持 View Transitions
- 使用 Shiki 代码高亮
- 优化 CSS 变量系统

**v1.0 (2026-05-19)**
- 初始版本发布

---

**文档结束**
