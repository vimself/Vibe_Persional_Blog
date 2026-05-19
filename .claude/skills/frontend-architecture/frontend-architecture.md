# BlogSystem 前端架构文档

**项目名称**：BlogSystem -- 个人博客系统
**文档版本**：v1.0
**最后更新**：2026-05-19
**文档类型**：前端技术架构设计

---

## 一、技术栈选型

### 1.1 核心技术栈

| 技术领域 | 技术选型 | 版本要求 | 选型理由 |
|---------|---------|---------|---------|
| 标记语言 | HTML5 | - | 语义化标签，提升可访问性和 SEO |
| 样式方案 | CSS3 | - | CSS Variables、Flexbox、Grid 现代布局 |
| 脚本语言 | JavaScript | ES6+ | 模块化、Promise、async/await 等现代特性 |
| Markdown 解析 | marked.js | >= 4.0 | 轻量、高性能、扩展性强 |
| YAML 解析 | js-yaml | >= 4.0 | Frontmatter 元数据解析 |
| 代码高亮 | highlight.js | >= 11.0 | 丰富的语言支持、主题丰富 |
| HTTP 请求 | Fetch API | 原生 | 无需依赖、Promise 原生支持 |

### 1.2 可选增强库

| 库名称 | 用途 | 引入方式 |
|-------|------|---------|
| DOMPurify | XSS 防护，净化 HTML | CDN / npm |
| dayjs | 日期格式化（轻量级） | CDN / npm |
| lodash.debounce | 搜索防抖、滚动防抖 | 按需引入 |

### 1.3 开发工具链

| 工具 | 用途 |
|-----|------|
| ESLint | JavaScript 代码规范检查 |
| Stylelint | CSS 代码规范检查 |
| Prettier | 代码格式化 |
| Live Server | 本地开发服务器 |
| Chrome DevTools | 调试和性能分析 |

---

## 二、项目结构设计

### 2.1 完整目录结构

```
BlogSystem/
├── index.html                    # 主入口页面
├── 404.html                      # 404 错误页面
├── favicon.ico                   # 网站图标
├── robots.txt                    # 爬虫配置
├── sitemap.xml                   # 站点地图（可选）
│
├── css/                          # 样式文件目录
│   ├── variables.css             # CSS 自定义属性（颜色、字号、间距等）
│   ├── reset.css                 # CSS Reset / Normalize
│   ├── base.css                  # 基础元素样式
│   ├── layout.css                # 布局样式（Header、Main、Footer）
│   ├── components.css            # 组件样式（PostCard、Tag、Pagination 等）
│   ├── article.css               # 文章内容渲染样式
│   ├── animations.css            # 动画定义
│   ├── utilities.css             # 工具类
│   └── responsive.css            # 响应式媒体查询
│
├── js/                           # JavaScript 模块目录
│   ├── app.js                    # 应用入口，初始化和生命周期管理
│   ├── modules/                  # 功能模块
│   │   ├── router.js             # 路由管理模块
│   │   ├── markdown.js           # Markdown 解析模块
│   │   ├── scanner.js            # 文件扫描模块
│   │   ├── theme.js              # 主题切换模块
│   │   ├── renderer.js           # 页面渲染模块
│   │   ├── cache.js              # 缓存管理模块
│   │   └── logger.js             # 日志模块
│   ├── components/               # UI 组件
│   │   ├── header.js             # 头部导航组件
│   │   ├── post-card.js          # 文章卡片组件
│   │   ├── post-detail.js        # 文章详情组件
│   │   ├── pagination.js         # 分页组件
│   │   ├── tag-filter.js         # 标签筛选组件
│   │   ├── back-to-top.js        # 返回顶部组件
│   │   ├── loading.js            # 加载状态组件
│   │   └── error-boundary.js     # 错误边界组件
│   └── utils/                    # 工具函数
│       ├── dom.js                # DOM 操作工具
│       ├── fetch.js              # Fetch 封装
│       ├── format.js             # 格式化工具（日期、文本等）
│       ├── debounce.js           # 防抖节流
│       └── constants.js          # 常量定义
│
├── content/                      # Markdown 内容目录
│   ├── posts/                    # 博客文章
│   │   ├── 2026-01-01-hello-world.md
│   │   ├── 2026-03-15-css-variables.md
│   │   └── ...
│   └── pages/                    # 静态页面（关于、归档等）
│       └── about.md
│
├── assets/                       # 静态资源目录
│   ├── images/                   # 图片资源
│   │   ├── logo.svg
│   │   ├── og-image.jpg          # Open Graph 分享图
│   │   └── posts/                # 文章配图
│   ├── icons/                    # 图标资源
│   │   ├── sun.svg               # 浅色主题图标
│   │   ├── moon.svg              # 深色主题图标
│   │   └── arrow-up.svg          # 返回顶部图标
│   └── fonts/                    # 字体文件（可选，本地托管）
│       ├── inter-variable.woff2
│       └── jetbrains-mono-variable.woff2
│
├── lib/                          # 第三方库（本地托管时使用）
│   ├── marked.min.js
│   ├── js-yaml.min.js
│   └── highlight.min.js
│
└── docs/                         # 项目文档
    ├── architecture.md           # 架构文档（本文件）
    ├── ui-ux-design.md           # UI/UX 设计文档
    └── deployment.md             # 部署文档
```

### 2.2 模块依赖关系

```
app.js（入口）
    │
    ├── router.js（路由）
    │       │
    │       └── renderer.js（渲染）
    │               │
    │               ├── scanner.js（文件扫描）
    │               │       │
    │               │       └── cache.js（缓存）
    │               │
    │               ├── markdown.js（解析）
    │               │       │
    │               │       └── highlight.js（代码高亮）
    │               │
    │               └── components/*（UI 组件）
    │
    ├── theme.js（主题）
    │
    └── logger.js（日志）
```

### 2.3 文件加载顺序

```html
<!-- CSS 加载顺序（按依赖关系） -->
<link rel="stylesheet" href="css/variables.css">
<link rel="stylesheet" href="css/reset.css">
<link rel="stylesheet" href="css/base.css">
<link rel="stylesheet" href="css/layout.css">
<link rel="stylesheet" href="css/components.css">
<link rel="stylesheet" href="css/article.css">
<link rel="stylesheet" href="css/animations.css">
<link rel="stylesheet" href="css/utilities.css">
<link rel="stylesheet" href="css/responsive.css">

<!-- JavaScript 加载策略 -->
<script src="lib/marked.min.js" defer></script>
<script src="lib/js-yaml.min.js" defer></script>
<script src="lib/highlight.min.js" defer></script>
<script type="module" src="js/app.js"></script>
```

---

## 三、核心模块设计

### 3.1 路由管理模块 (router.js)

**职责**：管理 SPA 路由，监听 URL 变化，触发页面渲染。

**设计要点**：
- 使用 History API（pushState/popState）
- 支持 Hash 模式降级
- 路由守卫机制
- 路由参数解析

**接口设计**：

```javascript
/**
 * 路由管理器
 */
class Router {
  /**
   * 注册路由规则
   * @param {string} pattern - 路由模式，如 '/post/:slug'
   * @param {Function} handler - 路由处理函数
   */
  register(pattern, handler) {}

  /**
   * 导航到指定路由
   * @param {string} path - 目标路径
   * @param {Object} options - 配置项
   * @param {boolean} options.replace - 是否替换当前历史记录
   */
  navigate(path, options = {}) {}

  /**
   * 获取当前路由信息
   * @returns {{ path: string, params: Object, query: Object }}
   */
  getCurrentRoute() {}

  /**
   * 注册路由变化前守卫
   * @param {Function} guard - 守卫函数，返回 false 可阻止导航
   */
  beforeEach(guard) {}

  /**
   * 注册路由变化后钩子
   * @param {Function} hook - 钩子函数
   */
  afterEach(hook) {}

  /**
   * 启动路由监听
   */
  start() {}
}
```

**路由规则定义**：

| 路由模式 | 页面类型 | 说明 |
|---------|---------|------|
| `/` | 首页 | 文章列表 |
| `/post/:slug` | 文章详情 | slug 为文件名（不含 .md） |
| `/tag/:name` | 标签筛选 | 按标签筛选文章 |
| `/page/:num` | 分页 | 文章列表分页 |
| `/about` | 关于页面 | 静态页面 |
| `/archive` | 归档页面 | 按年月归档 |
| `*` | 404 | 未匹配路由 |

**路由参数解析示例**：

```javascript
// 路由模式：/post/:slug
// 实际路径：/post/css-variables-guide
// 解析结果：{ slug: 'css-variables-guide' }

// 路由模式：/tag/:name/page/:num
// 实际路径：/tag/javascript/page/2
// 解析结果：{ name: 'javascript', num: '2' }
```

---

### 3.2 文件扫描模块 (scanner.js)

**职责**：扫描 content/posts 目录，获取 Markdown 文件列表和元数据。

**设计方案**：

由于纯前端无法直接读取文件系统，采用以下策略：

**方案一：静态索引文件（推荐）**

在 content 目录维护一个 `index.json` 文件，记录所有文章的元数据：

```json
{
  "posts": [
    {
      "slug": "hello-world",
      "file": "posts/hello-world.md",
      "title": "Hello World",
      "date": "2026-01-01",
      "description": "我的第一篇博客文章",
      "tags": ["随笔", "入门"],
      "draft": false
    }
  ],
  "lastUpdated": "2026-05-19T10:00:00Z"
}
```

**方案二：目录列表文件**

在支持目录列表的服务器上，通过 Fetch 获取目录页面并解析文件列表。

**方案三：构建时自动生成**

使用构建脚本在部署前自动生成索引文件。

**接口设计**：

```javascript
/**
 * 文件扫描器
 */
class Scanner {
  /**
   * 构造函数
   * @param {Object} config - 配置项
   * @param {string} config.contentDir - 内容目录路径
   * @param {string} config.indexFile - 索引文件名
   */
  constructor(config) {}

  /**
   * 获取文章索引列表
   * @param {Object} options - 查询选项
   * @param {string} options.tag - 按标签筛选
   * @param {number} options.page - 页码
   * @param {number} options.pageSize - 每页数量
   * @param {boolean} options.includeDrafts - 是否包含草稿
   * @returns {Promise<{ posts: Array, total: number, page: number, pageSize: number }>}
   */
  async getPostIndex(options = {}) {}

  /**
   * 获取单篇文章的原始 Markdown 内容
   * @param {string} slug - 文章 slug
   * @returns {Promise<string>} - 原始 Markdown 文本
   */
  async getPostContent(slug) {}

  /**
   * 获取所有标签及对应文章数量
   * @returns {Promise<Array<{ name: string, count: number }>>}
   */
  async getTags() {}

  /**
   * 获取归档数据（按年月分组）
   * @returns {Promise<Object>} - { '2026': { '01': [...], '03': [...] } }
   */
  async getArchive() {}

  /**
   * 刷新索引缓存
   * @returns {Promise<void>}
   */
  async refreshIndex() {}
}
```

---

### 3.3 Markdown 解析模块 (markdown.js)

**职责**：解析 Markdown 文本，提取 Frontmatter，转换为 HTML。

**处理流程**：

```
原始 Markdown
    │
    ├── Frontmatter 提取（--- 分隔符）
    │       │
    │       └── YAML 解析 -> 元数据对象
    │
    ├── Markdown 正文提取
    │       │
    │       ├── marked.js 解析
    │       │       │
    │       │       ├── 代码块高亮（highlight.js）
    │       │       ├── 自定义渲染器
    │       │       └── XSS 净化（DOMPurify）
    │       │
    │       └── HTML 字符串
    │
    └── 结果：{ metadata, html, toc }
```

**接口设计**：

```javascript
/**
 * Markdown 解析器
 */
class MarkdownParser {
  /**
   * 构造函数
   * @param {Object} config - 配置项
   * @param {boolean} config.enableToc - 是否生成目录
   * @param {boolean} config.enableHighlight - 是否启用代码高亮
   * @param {boolean} config.sanitize - 是否净化 HTML
   */
  constructor(config) {}

  /**
   * 解析完整的 Markdown 文本（包含 Frontmatter）
   * @param {string} rawText - 原始 Markdown 文本
   * @returns {Promise<{ metadata: Object, html: string, toc: Array }>}
   */
  async parse(rawText) {}

  /**
   * 提取 Frontmatter
   * @param {string} rawText - 原始文本
   * @returns {{ frontmatter: string, content: string }}
   */
  extractFrontmatter(rawText) {}

  /**
   * 解析 YAML Frontmatter
   * @param {string} yamlText - YAML 文本
   * @returns {Object} - 解析后的元数据对象
   */
  parseFrontmatter(yamlText) {}

  /**
   * 将 Markdown 转换为 HTML
   * @param {string} markdown - Markdown 文本
   * @returns {string} - HTML 字符串
   */
  renderToHtml(markdown) {}

  /**
   * 生成目录（Table of Contents）
   * @param {string} html - HTML 字符串
   * @returns {Array<{ level: number, id: string, text: string }>}
   */
  generateToc(html) {}

  /**
   * 计算阅读时间
   * @param {string} text - 纯文本内容
   * @returns {number} - 阅读分钟数
   */
  calculateReadingTime(text) {}
}
```

**Frontmatter 字段规范**：

```typescript
interface PostMetadata {
  title: string;           // 文章标题（必填）
  date: string;            // 发布日期，ISO 格式（必填）
  description?: string;    // 文章摘要（可选）
  tags?: string[];         // 标签数组（可选）
  draft?: boolean;         // 是否草稿，默认 false
  cover?: string;          // 封面图片路径（可选）
  author?: string;         // 作者（可选，默认站点配置）
}
```

**自定义渲染器配置**：

```javascript
const renderer = {
  // 标题添加 id，支持锚点导航
  heading(text, level) {
    const slug = generateSlug(text);
    return `<h${level} id="${slug}">${text}</h${level}>`;
  },

  // 链接添加 target="_blank"（外部链接）
  href(href, text) {
    const isExternal = href.startsWith('http');
    const target = isExternal ? ' target="_blank" rel="noopener noreferrer"' : '';
    return `<a href="${href}"${target}>${text}</a>`;
  },

  // 图片添加懒加载
  image(href, title, text) {
    return `<figure>
      <img src="${href}" alt="${text}" loading="lazy" />
      ${title ? `<figcaption>${title}</figcaption>` : ''}
    </figure>`;
  },

  // 代码块添加语言标识
  code(code, language) {
    const highlighted = highlight.js.highlight(code, { language });
    return `<pre><code class="hljs language-${language}">${highlighted.value}</code></pre>`;
  }
};
```

---

### 3.4 主题切换模块 (theme.js)

**职责**：管理明暗主题切换，持久化用户偏好。

**设计方案**：

- 使用 `data-theme` 属性控制主题
- CSS Variables 实现主题色切换
- 支持系统偏好检测
- LocalStorage 持久化

**接口设计**：

```javascript
/**
 * 主题管理器
 */
class ThemeManager {
  /**
   * 构造函数
   * @param {Object} config - 配置项
   * @param {string} config.defaultTheme - 默认主题 ('light' | 'dark' | 'system')
   * @param {string} config.storageKey - 存储键名
   */
  constructor(config) {}

  /**
   * 初始化主题
   * 检测系统偏好，读取用户设置，应用主题
   */
  init() {}

  /**
   * 获取当前主题
   * @returns {'light' | 'dark'}
   */
  getCurrentTheme() {}

  /**
   * 设置主题
   * @param {'light' | 'dark' | 'system'} theme - 主题类型
   */
  setTheme(theme) {}

  /**
   * 切换主题（light <-> dark）
   */
  toggle() {}

  /**
   * 监听系统主题变化
   * @param {Function} callback - 变化回调
   */
  watchSystemTheme(callback) {}

  /**
   * 注册主题变化回调
   * @param {Function} callback - 回调函数
   */
  onChange(callback) {}
}
```

**主题切换实现逻辑**：

```javascript
// 1. 初始化时检测
const initTheme = () => {
  const saved = localStorage.getItem('theme');
  const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
  const theme = saved || (prefersDark ? 'dark' : 'light');
  document.documentElement.setAttribute('data-theme', theme);
};

// 2. 切换时更新
const toggleTheme = () => {
  const current = document.documentElement.getAttribute('data-theme');
  const next = current === 'dark' ? 'light' : 'dark';
  document.documentElement.setAttribute('data-theme', next);
  localStorage.setItem('theme', next);
};

// 3. 监听系统变化
window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', (e) => {
  if (!localStorage.getItem('theme')) {
    document.documentElement.setAttribute('data-theme', e.matches ? 'dark' : 'light');
  }
});
```

---

### 3.5 页面渲染模块 (renderer.js)

**职责**：根据路由状态渲染对应页面，管理页面生命周期。

**页面类型**：

| 页面类型 | 渲染函数 | 数据来源 |
|---------|---------|---------|
| 首页 | renderHome() | scanner.getPostIndex() |
| 文章详情 | renderPost(slug) | scanner.getPostContent(slug) |
| 标签筛选 | renderTagList(tag) | scanner.getPostIndex({ tag }) |
| 归档页 | renderArchive() | scanner.getArchive() |
| 关于页 | renderAbout() | content/pages/about.md |
| 404 | render404() | 静态内容 |

**接口设计**：

```javascript
/**
 * 页面渲染器
 */
class Renderer {
  /**
   * 构造函数
   * @param {HTMLElement} container - 渲染容器
   * @param {Scanner} scanner - 文件扫描器实例
   * @param {MarkdownParser} parser - Markdown 解析器实例
   */
  constructor(container, scanner, parser) {}

  /**
   * 渲染首页
   * @param {Object} options - 渲染选项
   * @param {number} options.page - 页码
   * @param {string} options.tag - 标签筛选
   */
  async renderHome(options = {}) {}

  /**
   * 渲染文章详情页
   * @param {string} slug - 文章 slug
   */
  async renderPost(slug) {}

  /**
   * 渲染归档页面
   */
  async renderArchive() {}

  /**
   * 渲染关于页面
   */
  async renderAbout() {}

  /**
   * 渲染 404 页面
   */
  render404() {}

  /**
   * 渲染加载状态
   */
  renderLoading() {}

  /**
   * 渲染错误状态
   * @param {Error} error - 错误对象
   */
  renderError(error) {}

  /**
   * 清空容器
   */
  clear() {}

  /**
   * 页面进入动画
   */
  animateIn() {}
}
```

---

### 3.6 缓存管理模块 (cache.js)

**职责**：管理数据缓存，减少重复请求。

**接口设计**：

```javascript
/**
 * 缓存管理器
 */
class CacheManager {
  /**
   * 构造函数
   * @param {Object} config - 配置项
   * @param {number} config.maxSize - 最大缓存条目数
   * @param {number} config.ttl - 缓存过期时间（毫秒）
   */
  constructor(config) {}

  /**
   * 获取缓存
   * @param {string} key - 缓存键
   * @returns {*} - 缓存值，不存在返回 null
   */
  get(key) {}

  /**
   * 设置缓存
   * @param {string} key - 缓存键
   * @param {*} value - 缓存值
   * @param {number} ttl - 过期时间（可选，覆盖默认值）
   */
  set(key, value, ttl) {}

  /**
   * 删除缓存
   * @param {string} key - 缓存键
   */
  delete(key) {}

  /**
   * 清空所有缓存
   */
  clear() {}

  /**
   * 检查缓存是否存在且未过期
   * @param {string} key - 缓存键
   * @returns {boolean}
   */
  has(key) {}

  /**
   * 获取缓存统计信息
   * @returns {{ size: number, hits: number, misses: number }}
   */
  getStats() {}
}
```

**缓存策略**：

```javascript
// 内存缓存实现
class MemoryCache {
  constructor(config) {
    this.store = new Map();
    this.maxSize = config.maxSize || 100;
    this.defaultTTL = config.ttl || 5 * 60 * 1000; // 5 分钟
  }

  get(key) {
    const entry = this.store.get(key);
    if (!entry) return null;
    if (Date.now() > entry.expiry) {
      this.store.delete(key);
      return null;
    }
    return entry.value;
  }

  set(key, value, ttl = this.defaultTTL) {
    // LRU 淘汰策略
    if (this.store.size >= this.maxSize) {
      const firstKey = this.store.keys().next().value;
      this.store.delete(firstKey);
    }
    this.store.set(key, {
      value,
      expiry: Date.now() + ttl
    });
  }
}
```

---

### 3.7 日志模块 (logger.js)

**职责**：统一日志管理，支持不同级别日志输出。

```javascript
/**
 * 日志管理器
 */
class Logger {
  /**
   * @param {string} module - 模块名称
   * @param {string} level - 日志级别 ('debug' | 'info' | 'warn' | 'error')
   */
  constructor(module, level = 'info') {}

  debug(message, ...args) {}
  info(message, ...args) {}
  warn(message, ...args) {}
  error(message, ...args) {}
}
```

---

## 四、数据流设计

### 4.1 整体数据流架构

```
用户操作
    │
    ▼
┌─────────────┐
│   Router    │  URL 变化检测
└──────┬──────┘
       │
       ▼
┌─────────────┐
│  Renderer   │  页面渲染调度
└──────┬──────┘
       │
       ├───► Scanner.getPostIndex() ──► Cache ──► Fetch(index.json)
       │
       ├───► Scanner.getPostContent() ──► Cache ──► Fetch(.md file)
       │
       ▼
┌─────────────┐
│   Parser    │  Markdown 解析
└──────┬──────┘
       │
       ├───► Frontmatter 提取
       │
       ├───► Markdown -> HTML
       │
       ▼
┌─────────────┐
│   DOM       │  页面更新
└─────────────┘
```

### 4.2 首页加载流程

```
1. 页面加载
   │
   ├── 2. 初始化模块（Router, Scanner, Parser, Renderer, Theme）
   │
   ├── 3. Scanner.getPostIndex()
   │      │
   │      ├── 检查内存缓存
   │      │      │
   │      │      ├── 命中 -> 返回缓存数据
   │      │      └── 未命中 -> 继续
   │      │
   │      ├── Fetch('content/index.json')
   │      │      │
   │      │      ├── 成功 -> 解析 JSON
   │      │      └── 失败 -> 错误处理
   │      │
   │      ├── 写入缓存
   │      │
   │      └── 返回文章列表
   │
   ├── 4. Renderer.renderHome(posts)
   │      │
   │      ├── 渲染 Hero 区域
   │      ├── 渲染标签筛选栏
   │      ├── 渲染文章卡片列表
   │      └── 渲染分页组件
   │
   └── 5. 页面就绪
```

### 4.3 文章详情加载流程

```
1. 用户点击文章卡片 / URL 变化
   │
   ├── 2. Router 解析路径
   │      │
   │      └── 提取 slug 参数
   │
   ├── 3. Renderer.renderPost(slug)
   │      │
   │      ├── 渲染加载状态
   │      │
   │      ├── Scanner.getPostContent(slug)
   │      │      │
   │      │      ├── 检查缓存
   │      │      │      │
   │      │      │      ├── 命中 -> 返回缓存内容
   │      │      │      └── 未命中 -> 继续
   │      │      │
   │      │      ├── Fetch('content/posts/{slug}.md')
   │      │      │      │
   │      │      │      ├── 成功 -> 返回原始文本
   │      │      │      └── 失败 -> 404 或错误处理
   │      │      │
   │      │      └── 返回 Markdown 原文
   │      │
   │      ├── MarkdownParser.parse(rawText)
   │      │      │
   │      │      ├── 提取 Frontmatter
   │      │      │      │
   │      │      │      └── YAML 解析 -> metadata
   │      │      │
   │      │      ├── Markdown -> HTML
   │      │      │      │
   │      │      │      ├── 代码高亮
   │      │      │      └── XSS 净化
   │      │      │
   │      │      ├── 生成目录 TOC
   │      │      │
   │      │      └── 返回 { metadata, html, toc }
   │      │
   │      ├── 渲染文章头部（标题、日期、标签）
   │      ├── 渲染文章正文
   │      ├── 渲染文章导航（上/下一篇）
   │      └── 滚动到页面顶部
   │
   └── 4. Router 更新浏览器历史
```

### 4.4 路由切换流程

```
URL 变化（pushState / popState）
    │
    ▼
Router.resolve(path)
    │
    ├── 匹配路由规则
    │      │
    │      ├── 匹配成功 -> 提取参数
    │      └── 匹配失败 -> 404 路由
    │
    ▼
执行 beforeEach 守卫
    │
    ├── 返回 true -> 继续
    └── 返回 false / redirect -> 中断/重定向
    │
    ▼
Renderer 清空当前页面
    │
    ▼
Renderer 渲染新页面
    │
    ▼
执行 afterEach 钩子
    │
    ▼
更新文档标题
    │
    ▼
滚动到页面顶部
```

---

## 五、API 设计

### 5.1 文件扫描 API

```javascript
// 获取文章列表
const posts = await scanner.getPostIndex({
  page: 1,           // 页码，从 1 开始
  pageSize: 10,      // 每页数量，默认 10
  tag: 'javascript', // 标签筛选（可选）
  includeDrafts: false // 是否包含草稿
});
// 返回值：
// {
//   posts: [
//     {
//       slug: 'css-variables-guide',
//       title: 'CSS Variables 完全指南',
//       date: '2026-03-15',
//       description: '深入理解 CSS 自定义属性...',
//       tags: ['CSS', '前端'],
//       readingTime: 8
//     }
//   ],
//   total: 42,
//   page: 1,
//   pageSize: 10,
//   totalPages: 5
// }

// 获取单篇文章内容
const rawMarkdown = await scanner.getPostContent('css-variables-guide');
// 返回值：原始 Markdown 字符串（包含 Frontmatter）

// 获取所有标签
const tags = await scanner.getTags();
// 返回值：[{ name: 'CSS', count: 12 }, { name: 'JavaScript', count: 8 }]

// 获取归档数据
const archive = await scanner.getArchive();
// 返回值：
// {
//   '2026': {
//     '05': [{ slug: '...', title: '...', date: '...' }],
//     '03': [{ slug: '...', title: '...', date: '...' }]
//   },
//   '2025': {
//     '12': [{ slug: '...', title: '...', date: '...' }]
//   }
// }
```

### 5.2 Markdown 解析 API

```javascript
// 解析完整 Markdown 文档
const result = await parser.parse(rawText);
// 返回值：
// {
//   metadata: {
//     title: 'CSS Variables 完全指南',
//     date: '2026-03-15',
//     description: '深入理解 CSS 自定义属性...',
//     tags: ['CSS', '前端'],
//     draft: false
//   },
//   html: '<h1 id="css-variables">CSS Variables</h1><p>...</p>',
//   toc: [
//     { level: 1, id: 'css-variables', text: 'CSS Variables 完全指南' },
//     { level: 2, id: '基础概念', text: '基础概念' },
//     { level: 3, id: '什么是-css-变量', text: '什么是 CSS 变量' }
//   ],
//   readingTime: 8
// }

// 单独提取 Frontmatter
const { frontmatter, content } = parser.extractFrontmatter(rawText);
// 返回值：
// {
//   frontmatter: 'title: CSS Variables\ndate: 2026-03-15\n...',
//   content: '# CSS Variables\n\n正文内容...'
// }

// 单独渲染 Markdown
const html = parser.renderToHtml(markdownContent);
// 返回值：HTML 字符串
```

### 5.3 路由管理 API

```javascript
// 注册路由
router.register('/', (params) => {
  renderer.renderHome();
});

router.register('/post/:slug', (params) => {
  renderer.renderPost(params.slug);
});

router.register('/tag/:name', (params) => {
  renderer.renderHome({ tag: params.name });
});

router.register('/tag/:name/page/:num', (params) => {
  renderer.renderHome({ tag: params.name, page: parseInt(params.num) });
});

// 编程式导航
router.navigate('/post/css-variables-guide');
router.navigate('/tag/javascript', { replace: true });

// 获取当前路由
const route = router.getCurrentRoute();
// { path: '/post/css-variables-guide', params: { slug: 'css-variables-guide' }, query: {} }

// 路由守卫
router.beforeEach((to, from) => {
  // 可用于页面切换动画、权限检查等
  return true; // 返回 false 阻止导航
});

// 路由后置钩子
router.afterEach((to, from) => {
  // 更新页面标题、记录浏览历史等
  document.title = `${to.params.title || '博客'} - BlogSystem`;
});
```

### 5.4 主题管理 API

```javascript
// 初始化主题
theme.init();

// 获取当前主题
const current = theme.getCurrentTheme(); // 'light' 或 'dark'

// 设置主题
theme.setTheme('dark');
theme.setTheme('light');
theme.setTheme('system'); // 跟随系统

// 切换主题
theme.toggle();

// 监听主题变化
theme.onChange((newTheme) => {
  console.log('主题切换为:', newTheme);
  // 更新图标等
});

// 监听系统主题变化
theme.watchSystemTheme((isDark) => {
  console.log('系统主题变化:', isDark ? '深色' : '浅色');
});
```

### 5.5 缓存管理 API

```javascript
// 设置缓存
cache.set('post:css-guide', markdownContent, 10 * 60 * 1000); // 10 分钟过期

// 获取缓存
const content = cache.get('post:css-guide');

// 检查缓存
if (cache.has('post:css-guide')) {
  // 使用缓存
}

// 删除缓存
cache.delete('post:css-guide');

// 清空所有缓存
cache.clear();

// 获取缓存统计
const stats = cache.getStats();
// { size: 15, hits: 42, misses: 8 }
```

---

## 六、性能优化策略

### 6.1 懒加载策略

**图片懒加载**：

```html
<img src="image.jpg" alt="描述" loading="lazy" decoding="async">
```

**路由级懒加载**：

```javascript
// 按需加载页面组件
const loadPostComponent = async () => {
  const module = await import('./components/post-detail.js');
  return module.default;
};
```

**字体懒加载**：

```css
@font-face {
  font-family: 'Inter';
  src: url('fonts/inter-variable.woff2') format('woff2');
  font-display: swap; /* 字体加载期间使用备用字体 */
  unicode-range: U+0000-00FF; /* 仅加载拉丁字符 */
}
```

### 6.2 缓存策略

**内存缓存**：

```javascript
// 文章内容缓存
const contentCache = new Map();

// 索引数据缓存
let indexCache = null;
let indexCacheTime = 0;
const INDEX_CACHE_TTL = 5 * 60 * 1000; // 5 分钟
```

**HTTP 缓存**：

```
# Nginx 配置示例
location /content/ {
    add_header Cache-Control "public, max-age=300"; # 5 分钟
}

location /assets/ {
    add_header Cache-Control "public, max-age=31536000, immutable"; # 1 年
}

location /css/ {
    add_header Cache-Control "public, max-age=86400"; # 1 天
}
```

**Service Worker 缓存（可选）**：

```javascript
// sw.js
const CACHE_NAME = 'blog-v1';
const STATIC_ASSETS = [
  '/',
  '/css/variables.css',
  '/css/base.css',
  '/js/app.js'
];

self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME).then((cache) => cache.addAll(STATIC_ASSETS))
  );
});

self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request).then((response) => {
      return response || fetch(event.request);
    })
  );
});
```

### 6.3 资源优化

**CSS 优化**：

```css
/* 使用 CSS containment */
.post-card {
  contain: layout style paint;
}

/* 使用 will-change 提示浏览器优化 */
.page-enter {
  will-change: opacity, transform;
}

/* 避免布局抖动 */
img {
  aspect-ratio: 16 / 9;
  width: 100%;
  height: auto;
}
```

**JavaScript 优化**：

```javascript
// 使用 requestAnimationFrame 优化滚动处理
let ticking = false;
window.addEventListener('scroll', () => {
  if (!ticking) {
    requestAnimationFrame(() => {
      handleScroll();
      ticking = false;
    });
    ticking = true;
  }
});

// 使用 IntersectionObserver 代替 scroll 事件
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      // 元素进入视口
    }
  });
}, { threshold: 0.1 });
```

**资源压缩**：

- CSS/JS 文件压缩（minify）
- 图片压缩（WebP 格式优先）
- Gzip/Brotli 压缩传输
- SVG 图标内联或使用 sprite

### 6.4 渲染优化

**避免强制同步布局**：

```javascript
// 不好的做法
function bad() {
  element.style.height = '100px';
  const height = element.offsetHeight; // 强制同步布局
  element.style.marginTop = height + 'px';
}

// 好的做法
function good() {
  const height = element.offsetHeight; // 先读取
  element.style.height = '100px';
  element.style.marginTop = height + 'px'; // 后写入
}
```

**虚拟列表（长列表优化）**：

```javascript
// 当文章数量很大时，使用虚拟滚动
class VirtualList {
  constructor(container, itemHeight, renderItem) {
    this.container = container;
    this.itemHeight = itemHeight;
    this.renderItem = renderItem;
    this.items = [];
    this.visibleCount = Math.ceil(container.clientHeight / itemHeight) + 2;
  }

  render(items) {
    this.items = items;
    this.container.style.height = items.length * this.itemHeight + 'px';
    this.updateVisibleItems();
  }

  updateVisibleItems() {
    const scrollTop = this.container.scrollTop;
    const startIndex = Math.floor(scrollTop / this.itemHeight);
    const endIndex = Math.min(startIndex + this.visibleCount, this.items.length);
    // 渲染可见区域的元素
  }
}
```

**骨架屏**：

```html
<div class="skeleton-card">
  <div class="skeleton-line skeleton-line--short"></div>
  <div class="skeleton-line skeleton-line--title"></div>
  <div class="skeleton-line"></div>
  <div class="skeleton-line"></div>
  <div class="skeleton-line skeleton-line--short"></div>
</div>
```

```css
.skeleton-line {
  height: 16px;
  background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
  background-size: 200% 100%;
  animation: skeleton-loading 1.5s infinite;
  border-radius: 4px;
  margin-bottom: 12px;
}

@keyframes skeleton-loading {
  0% { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}
```

---

## 七、错误处理机制

### 7.1 错误类型定义

```javascript
/**
 * 自定义错误类
 */
class BlogError extends Error {
  constructor(message, code, details = {}) {
    super(message);
    this.name = 'BlogError';
    this.code = code;
    this.details = details;
  }
}

// 错误码定义
const ErrorCodes = {
  NETWORK_ERROR: 'NETWORK_ERROR',
  FILE_NOT_FOUND: 'FILE_NOT_FOUND',
  PARSE_ERROR: 'PARSE_ERROR',
  ROUTE_NOT_FOUND: 'ROUTE_NOT_FOUND',
  CACHE_ERROR: 'CACHE_ERROR',
  RENDER_ERROR: 'RENDER_ERROR'
};
```

### 7.2 文件读取错误处理

```javascript
class Scanner {
  async getPostContent(slug) {
    try {
      const response = await fetch(`content/posts/${slug}.md`);

      if (!response.ok) {
        if (response.status === 404) {
          throw new BlogError(
            `文章 "${slug}" 不存在`,
            ErrorCodes.FILE_NOT_FOUND,
            { slug, status: 404 }
          );
        }
        throw new BlogError(
          `加载文章失败: ${response.statusText}`,
          ErrorCodes.NETWORK_ERROR,
          { slug, status: response.status }
        );
      }

      return await response.text();
    } catch (error) {
      if (error instanceof BlogError) throw error;

      throw new BlogError(
        '网络请求失败，请检查网络连接',
        ErrorCodes.NETWORK_ERROR,
        { slug, originalError: error }
      );
    }
  }
}
```

### 7.3 Markdown 解析错误处理

```javascript
class MarkdownParser {
  async parse(rawText) {
    try {
      // 提取 Frontmatter
      const { frontmatter, content } = this.extractFrontmatter(rawText);

      // 解析元数据
      let metadata = {};
      if (frontmatter) {
        try {
          metadata = this.parseFrontmatter(frontmatter);
        } catch (yamlError) {
          console.warn('Frontmatter 解析失败，使用默认值:', yamlError);
          metadata = {
            title: '未知标题',
            date: new Date().toISOString().split('T')[0]
          };
        }
      }

      // 渲染 HTML
      const html = this.renderToHtml(content);

      return { metadata, html, toc: this.generateToc(html) };
    } catch (error) {
      throw new BlogError(
        'Markdown 解析失败',
        ErrorCodes.PARSE_ERROR,
        { originalError: error }
      );
    }
  }
}
```

### 7.4 路由错误处理

```javascript
class Router {
  resolve(path) {
    // 尝试匹配路由
    const match = this.matchRoute(path);

    if (!match) {
      // 路由未找到，渲染 404
      this.renderer.render404();
      return;
    }

    try {
      // 执行路由处理
      match.handler(match.params);
    } catch (error) {
      console.error('路由处理错误:', error);
      this.renderer.renderError(error);
    }
  }
}
```

### 7.5 全局错误边界

```javascript
class ErrorBoundary {
  constructor(container) {
    this.container = container;

    // 捕获未处理的错误
    window.addEventListener('error', (event) => {
      this.handleError(event.error);
    });

    // 捕获未处理的 Promise 错误
    window.addEventListener('unhandledrejection', (event) => {
      this.handleError(event.reason);
    });
  }

  handleError(error) {
    console.error('捕获到未处理的错误:', error);

    // 渲染错误页面
    this.container.innerHTML = `
      <div class="error-boundary">
        <h2>出现了错误</h2>
        <p>${error.message || '未知错误'}</p>
        <button onclick="location.reload()">刷新页面</button>
        <a href="/">返回首页</a>
      </div>
    `;
  }
}
```

### 7.6 错误状态 UI

**404 页面**：

```html
<div class="error-page error-page--404">
  <div class="error-page__code">404</div>
  <h1 class="error-page__title">页面未找到</h1>
  <p class="error-page__message">
    您访问的页面不存在或已被移除
  </p>
  <div class="error-page__actions">
    <a href="/" class="btn btn--primary">返回首页</a>
    <button onclick="history.back()" class="btn btn--secondary">返回上一页</button>
  </div>
</div>
```

**文章加载失败**：

```html
<div class="error-page error-page--load-failed">
  <div class="error-page__icon">⚠️</div>
  <h2 class="error-page__title">文章加载失败</h2>
  <p class="error-page__message">
    无法加载文章内容，请检查网络连接后重试
  </p>
  <div class="error-page__actions">
    <button onclick="location.reload()" class="btn btn--primary">重试</button>
    <a href="/" class="btn btn--secondary">返回首页</a>
  </div>
</div>
```

**网络错误提示**：

```javascript
// Toast 提示
function showToast(message, type = 'error') {
  const toast = document.createElement('div');
  toast.className = `toast toast--${type}`;
  toast.textContent = message;
  document.body.appendChild(toast);

  setTimeout(() => {
    toast.classList.add('toast--visible');
  }, 10);

  setTimeout(() => {
    toast.classList.remove('toast--visible');
    setTimeout(() => toast.remove(), 300);
  }, 3000);
}
```

---

## 八、部署方案

### 8.1 静态文件部署

**支持的部署平台**：

| 平台 | 配置文件 | 特点 |
|-----|---------|------|
| GitHub Pages | 无需配置 | 免费，自动部署 |
| Vercel | vercel.json | 免费，边缘网络，自动 HTTPS |
| Netlify | netlify.toml | 免费，表单处理，函数支持 |
| Cloudflare Pages | _redirects | 免费，全球 CDN |
| 阿里云 OSS | - | 国内访问快，按量付费 |
| 腾讯云 COS | - | 国内访问快，按量付费 |

### 8.2 Vercel 配置

```json
{
  "version": 2,
  "builds": [
    {
      "src": "**/*",
      "use": "@vercel/static"
    }
  ],
  "routes": [
    {
      "src": "/content/(.*)",
      "headers": {
        "Cache-Control": "public, max-age=300"
      }
    },
    {
      "src": "/assets/(.*)",
      "headers": {
        "Cache-Control": "public, max-age=31536000, immutable"
      }
    },
    {
      "handle": "filesystem"
    },
    {
      "src": "/(.*)",
      "dest": "/index.html"
    }
  ]
}
```

### 8.3 Netlify 配置

```toml
# netlify.toml
[build]
  publish = "."

[[headers]]
  for = "/assets/*"
  [headers.values]
    Cache-Control = "public, max-age=31536000, immutable"

[[headers]]
  for = "/content/*"
  [headers.values]
    Cache-Control = "public, max-age=300"

[[headers]]
  for = "/css/*"
  [headers.values]
    Cache-Control = "public, max-age=86400"

# SPA 路由回退
[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

### 8.4 CDN 配置

**静态资源 CDN**：

```
# 将以下资源托管到 CDN
/assets/images/*
/assets/fonts/*
/lib/*
/css/*
```

**CDN 配置要点**：

1. **CORS 配置**：

```nginx
location /content/ {
    add_header Access-Control-Allow-Origin *;
    add_header Access-Control-Allow-Methods "GET, OPTIONS";
}
```

2. **MIME 类型**：

```nginx
types {
    text/markdown md;
    application/json json;
    text/css css;
    application/javascript js;
    image/svg+xml svg;
    font/woff2 woff2;
}
```

3. **Gzip 压缩**：

```nginx
gzip on;
gzip_types text/plain text/css application/json application/javascript text/markdown;
gzip_min_length 1000;
gzip_comp_level 6;
```

### 8.5 缓存策略配置

**分层缓存策略**：

| 资源类型 | Cache-Control | 说明 |
|---------|---------------|------|
| HTML | `no-cache` | 每次验证新鲜度 |
| CSS/JS | `max-age=86400` | 1 天，配合文件名 hash |
| 字体 | `max-age=31536000, immutable` | 1 年，几乎不变 |
| 图片 | `max-age=31536000, immutable` | 1 年，配合文件名 hash |
| Markdown 内容 | `max-age=300` | 5 分钟，内容可能更新 |
| index.json | `max-age=300` | 5 分钟，索引可能更新 |

**版本化文件名**：

```html
<!-- 使用内容 hash 作为文件名 -->
<link rel="stylesheet" href="css/base.a1b2c3d4.css">
<script src="js/app.e5f6g7h8.js"></script>
```

### 8.6 跨域处理

**开发环境代理**：

```javascript
// 本地开发服务器配置（Vite 示例）
export default {
  server: {
    proxy: {
      '/content': {
        target: 'http://localhost:3000',
        changeOrigin: true
      }
    }
  }
};
```

**生产环境 CORS**：

```
# Nginx 配置
location /content/ {
    add_header Access-Control-Allow-Origin "https://yourdomain.com";
    add_header Access-Control-Allow-Methods "GET, OPTIONS";
    add_header Access-Control-Allow-Headers "Content-Type";
    add_header Access-Control-Max-Age 86400;
}
```

### 8.7 部署检查清单

```
□ 所有 CSS/JS 文件已压缩
□ 图片已优化（WebP 格式优先）
□ 字体文件已子集化
□ 缓存策略已配置
□ CORS 已配置
□ HTTPS 已启用
□ 404 页面已配置
□ SPA 路由回退已配置
□ robots.txt 已配置
□ sitemap.xml 已生成
□ OG 元标签已配置
□ Favicon 已配置
```

---

## 九、开发指南

### 9.1 本地开发环境

```bash
# 克隆项目
git clone https://github.com/your-username/BlogSystem.git
cd BlogSystem

# 启动本地服务器
# 方式 1：Python
python -m http.server 8000

# 方式 2：Node.js
npx serve .

# 方式 3：VS Code Live Server
# 右键 index.html -> Open with Live Server
```

### 9.2 添加新文章

1. 在 `content/posts/` 目录创建 `.md` 文件
2. 文件名格式：`YYYY-MM-DD-slug.md`
3. 添加 Frontmatter 元数据
4. 刷新页面自动显示

### 9.3 代码规范

- **HTML**：语义化标签，属性使用双引号
- **CSS**：BEM 命名，使用 CSS Variables
- **JavaScript**：ES6+，async/await，错误处理

### 9.4 测试要点

- [ ] 响应式布局在不同设备正常显示
- [ ] 明暗主题切换正常
- [ ] 路由切换无闪烁
- [ ] Markdown 渲染正确（代码高亮、表格、图片等）
- [ ] 错误状态显示正常（404、网络错误等）
- [ ] 性能指标达标（FCP < 1s, LCP < 2s）

---

## 十、附录

### 10.1 性能指标参考

| 指标 | 目标值 | 测量工具 |
|-----|-------|---------|
| First Contentful Paint (FCP) | < 1.0s | Lighthouse |
| Largest Contentful Paint (LCP) | < 2.0s | Lighthouse |
| Cumulative Layout Shift (CLS) | < 0.1 | Lighthouse |
| First Input Delay (FID) | < 100ms | Web Vitals |
| Time to Interactive (TTI) | < 3.0s | Lighthouse |
| 总资源大小（不含图片） | < 200KB | Network 面板 |

### 10.2 浏览器兼容性

| 浏览器 | 最低版本 |
|-------|---------|
| Chrome | 80+ |
| Firefox | 78+ |
| Safari | 14+ |
| Edge | 80+ |
| iOS Safari | 14+ |
| Android Chrome | 80+ |

### 10.3 相关文档

- UI/UX 设计文档：`docs/ui-ux-design.md`
- 部署文档：`docs/deployment.md`
- 项目 README：`README.md`

---

**文档结束**

本架构文档覆盖了 BlogSystem 前端系统的完整技术设计，包括模块划分、数据流、API 设计、性能优化、错误处理和部署方案。开发时应严格遵循本文档的设计规范，确保代码质量和系统可维护性。
