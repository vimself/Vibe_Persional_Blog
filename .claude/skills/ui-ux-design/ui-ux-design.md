# BlogSystem UI/UX 设计文档

**项目名称**：BlogSystem -- 个人博客系统
**文档版本**：v1.0
**最后更新**：2026-05-19
**技术栈**：HTML5 + CSS3 + JavaScript (ES6+) | CSS Variables + Flexbox/Grid

---

## 一、设计原则

### 1.1 内容优先原则

博客系统的核心价值在于文字内容本身。所有设计决策必须服务于阅读体验。

- 内容区域占据页面视觉中心，宽度控制在合理阅读范围内
- 正文字号不小于 16px，确保舒适阅读
- 行高不低于 1.6，避免文字拥挤
- 段落间距大于行间距，明确区分段落边界
- 图片和代码块作为内容的辅助元素，不应喧宾夺主

### 1.2 简约美学原则

遵循 Anthropic 的设计哲学：干净、克制、有目的性。

- 每个界面元素的存在必须有明确的功能目的
- 装饰性元素严格控制，仅在必要时使用
- 留白是设计的一部分，不是"空"
- 视觉元素之间保持一致的间距和对齐
- 避免过度设计：不使用不必要的阴影、渐变、动画

### 1.3 用户友好原则

- 导航结构清晰，用户随时知道自己在哪里
- 交互元素有明确的可点击/可交互暗示
- 加载状态有清晰的反馈
- 所有操作可逆（返回、回退）
- 错误状态有友好的提示信息

### 1.4 无干扰原则

- 不包含评论系统
- 不包含打赏/赞助功能
- 不包含弹窗、浮层广告
- 不包含不必要的社交分享按钮
- 不使用自动播放的媒体内容
- 不使用侵入性的 cookie 提示（仅在法律要求时使用最小化提示）

---

## 二、色彩系统

### 2.1 色彩变量定义（CSS Custom Properties）

```css
:root {
  /* === 中性灰色系（主色调） === */
  --color-gray-50:   #FAFAFA;
  --color-gray-100:  #F5F5F5;
  --color-gray-200:  #E5E5E5;
  --color-gray-300:  #D4D4D4;
  --color-gray-400:  #A3A3A3;
  --color-gray-500:  #737373;
  --color-gray-600:  #525252;
  --color-gray-700:  #404040;
  --color-gray-800:  #262626;
  --color-gray-900:  #171717;
  --color-gray-950:  #0A0A0A;

  /* === 强调色（柔和蓝紫色） === */
  --color-accent-50:  #F0F0FF;
  --color-accent-100: #E0E0FF;
  --color-accent-200: #C4C4FF;
  --color-accent-300: #A0A0FF;
  --color-accent-400: #7C7CFF;
  --color-accent-500: #6366F1;   /* 主强调色 */
  --color-accent-600: #5254D6;
  --color-accent-700: #4344B8;
  --color-accent-800: #36369A;
  --color-accent-900: #2C2C7D;

  /* === 语义色 === */
  --color-success:   #22C55E;
  --color-warning:   #EAB308;
  --color-error:     #EF4444;
  --color-info:      #3B82F6;

  /* === 功能色映射 === */
  --color-bg-primary:    #FFFFFF;
  --color-bg-secondary:  var(--color-gray-50);
  --color-bg-tertiary:   var(--color-gray-100);
  --color-bg-code:       var(--color-gray-50);

  --color-text-primary:   var(--color-gray-900);
  --color-text-secondary: var(--color-gray-600);
  --color-text-tertiary:  var(--color-gray-400);
  --color-text-inverse:   #FFFFFF;

  --color-border-primary:   var(--color-gray-200);
  --color-border-secondary: var(--color-gray-100);

  --color-link:          var(--color-accent-500);
  --color-link-hover:    var(--color-accent-600);
  --color-link-visited:  var(--color-accent-700);
}
```

### 2.2 暗色模式变量

```css
[data-theme="dark"] {
  --color-bg-primary:    var(--color-gray-950);
  --color-bg-secondary:  var(--color-gray-900);
  --color-bg-tertiary:   var(--color-gray-800);
  --color-bg-code:       var(--color-gray-800);

  --color-text-primary:   var(--color-gray-50);
  --color-text-secondary: var(--color-gray-400);
  --color-text-tertiary:  var(--color-gray-500);

  --color-border-primary:   var(--color-gray-700);
  --color-border-secondary: var(--color-gray-800);

  --color-link:          var(--color-accent-400);
  --color-link-hover:    var(--color-accent-300);
  --color-link-visited:  var(--color-accent-200);
}
```

### 2.3 色彩对比度要求

| 元素组合 | 最低对比度 | 目标对比度 | WCAG 等级 |
|---------|-----------|-----------|----------|
| 正文文字 / 背景 | 7:1 | 8:1+ | AAA |
| 次要文字 / 背景 | 4.5:1 | 6:1+ | AA |
| 链接文字 / 背景 | 4.5:1 | 6:1+ | AA |
| 占位符文字 / 背景 | 3:1 | 4.5:1 | A |
| 图标 / 背景 | 3:1 | 4.5:1 | A |
| 边框 / 背景 | 3:1 | 3:1+ | A |

---

## 三、字体系统

### 3.1 字体栈定义

```css
:root {
  /* 标题字体 */
  --font-heading: 'Inter', 'SF Pro Display', -apple-system, BlinkMacSystemFont,
                  'Segoe UI', Roboto, 'Noto Sans SC', 'PingFang SC',
                  'Microsoft YaHei', sans-serif;

  /* 正文字体 */
  --font-body: 'Inter', 'SF Pro Text', -apple-system, BlinkMacSystemFont,
               'Segoe UI', Roboto, 'Noto Sans SC', 'PingFang SC',
               'Microsoft YaHei', sans-serif;

  /* 代码字体 */
  --font-mono: 'JetBrains Mono', 'Fira Code', 'SF Mono', 'Cascadia Code',
               'Consolas', 'Liberation Mono', monospace;
}
```

### 3.2 字体加载策略

```html
<!-- Google Fonts 预加载 -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
```

字体加载采用 `font-display: swap` 策略，确保文字始终可见，避免 FOIT（Flash of Invisible Text）。

### 3.3 字号规范

| 用途 | CSS 变量 | 像素值 | rem 值 | 行高 | 字重 |
|-----|---------|-------|-------|-----|-----|
| H1 标题 | `--text-h1` | 36px | 2.25rem | 1.2 | 700 |
| H2 标题 | `--text-h2` | 28px | 1.75rem | 1.25 | 700 |
| H3 标题 | `--text-h3` | 22px | 1.375rem | 1.3 | 600 |
| H4 标题 | `--text-h4` | 18px | 1.125rem | 1.35 | 600 |
| H5 标题 | `--text-h5` | 16px | 1rem | 1.4 | 600 |
| H6 标题 | `--text-h6` | 14px | 0.875rem | 1.4 | 600 |
| 正文大 | `--text-lg` | 18px | 1.125rem | 1.7 | 400 |
| 正文 | `--text-base` | 16px | 1rem | 1.7 | 400 |
| 正文小 | `--text-sm` | 14px | 0.875rem | 1.6 | 400 |
| 辅助文字 | `--text-xs` | 12px | 0.75rem | 1.5 | 400 |
| 代码 | `--text-code` | 14px | 0.875rem | 1.6 | 400 |

### 3.4 字体变量定义

```css
:root {
  --text-h1:    2.25rem;
  --text-h2:    1.75rem;
  --text-h3:    1.375rem;
  --text-h4:    1.125rem;
  --text-h5:    1rem;
  --text-h6:    0.875rem;
  --text-lg:    1.125rem;
  --text-base:  1rem;
  --text-sm:    0.875rem;
  --text-xs:    0.75rem;

  --leading-tight:   1.2;
  --leading-snug:    1.35;
  --leading-normal:  1.5;
  --leading-relaxed: 1.625;
  --leading-loose:   1.75;

  --weight-regular:  400;
  --weight-medium:   500;
  --weight-semibold: 600;
  --weight-bold:     700;
}
```

---

## 四、间距系统

### 4.1 基于 8px 网格的间距规范

```css
:root {
  --space-0:   0;
  --space-1:   4px;     /* 0.5x 基础单位 - 微间距 */
  --space-2:   8px;     /* 1x 基础单位 */
  --space-3:   12px;    /* 1.5x 基础单位 */
  --space-4:   16px;    /* 2x 基础单位 */
  --space-5:   20px;    /* 2.5x 基础单位 */
  --space-6:   24px;    /* 3x 基础单位 */
  --space-8:   32px;    /* 4x 基础单位 */
  --space-10:  40px;    /* 5x 基础单位 */
  --space-12:  48px;    /* 6x 基础单位 */
  --space-16:  64px;    /* 8x 基础单位 */
  --space-20:  80px;    /* 10x 基础单位 */
  --space-24:  96px;    /* 12x 基础单位 */
  --space-32:  128px;   /* 16x 基础单位 */
}
```

### 4.2 间距使用场景

| 间距 Token | 值 | 使用场景 |
|-----------|-----|---------|
| `--space-1` | 4px | 图标与文字间距、行内元素间距 |
| `--space-2` | 8px | 紧凑元素间距、标签内边距 |
| `--space-3` | 12px | 卡片内边距、按钮内边距 |
| `--space-4` | 16px | 列表项间距、表单元素间距 |
| `--space-5` | 20px | 段落间距 |
| `--space-6` | 24px | 卡片之间间距、区块内边距 |
| `--space-8` | 32px | 内容区块间距 |
| `--space-10` | 40px | 区域分隔 |
| `--space-12` | 48px | 页面区块间距 |
| `--space-16` | 64px | 主要区域分隔 |
| `--space-20` | 80px | 页面顶部/底部留白 |
| `--space-24` | 96px | 英雄区域留白 |

### 4.3 内容宽度约束

```css
:root {
  --content-max-width:     720px;   /* 正文最大宽度 */
  --content-wide-width:    960px;   /* 宽内容区（含侧边） */
  --content-full-width:    1200px;  /* 全宽内容区 */
  --page-padding-x:        var(--space-6);  /* 页面水平内边距 */
}
```

---

## 五、布局设计

### 5.1 整体布局结构

```
+----------------------------------------------------------+
|                        HEADER                             |
|  [Logo/站点名]              [导航链接]    [主题切换]       |
+----------------------------------------------------------+
|                                                          |
|                      MAIN CONTENT                        |
|                                                          |
|  +----------------------------------------------------+  |
|  |                 博客列表 / 文章详情                  |  |
|  +----------------------------------------------------+  |
|                                                          |
+----------------------------------------------------------+
|                         FOOTER                           |
|              [版权信息]  [回到顶部]                       |
+----------------------------------------------------------+
```

### 5.2 首页/博客列表布局

```
+----------------------------------------------------------+
|                                                          |
|   +-- Hero Section -----------------------------------+  |
|   |                                                   |  |
|   |   站点标题                                        |  |
|   |   站点描述/副标题                                 |  |
|   |                                                   |  |
|   +---------------------------------------------------+  |
|                                                          |
|   +-- 标签筛选栏 ------------------------------------+  |
|   |  [全部] [标签1] [标签2] [标签3] ...               |  |
|   +---------------------------------------------------+  |
|                                                          |
|   +-- 文章卡片列表 ----------------------------------+  |
|   |                                                   |  |
|   |  +-- 卡片 1 -----------------------------------+  |  |
|   |  |  日期                    [标签] [标签]       |  |  |
|   |  |  文章标题                                    |  |  |
|   |  |  文章摘要描述文字...                         |  |  |
|   |  |  阅读全文 ->                                 |  |  |
|   |  +----------------------------------------------+  |  |
|   |                                                   |  |
|   |  +-- 卡片 2 -----------------------------------+  |  |
|   |  |  ...                                        |  |  |
|   |  +----------------------------------------------+  |  |
|   |                                                   |  |
|   +---------------------------------------------------+  |
|                                                          |
|   +-- 分页 ------------------------------------------+   |
|   |         < 1  [2]  3  ...  10  >                  |   |
|   +---------------------------------------------------+  |
|                                                          |
+----------------------------------------------------------+
```

### 5.3 文章详情页布局

```
+----------------------------------------------------------+
|                                                          |
|   +-- 文章头部 --------------------------------------+   |
|   |                                                   |   |
|   |  <- 返回列表                                      |   |
|   |                                                   |   |
|   |  文章标题（H1）                                   |   |
|   |  发布日期  |  阅读时间  |  [标签] [标签]          |   |
|   |                                                   |   |
|   +---------------------------------------------------+   |
|                                                          |
|   +-- 文章正文 --------------------------------------+   |
|   |                                                   |   |
|   |  段落文字...                                      |   |
|   |                                                   |   |
|   |  ## 二级标题                                      |   |
|   |                                                   |   |
|   |  段落文字...                                      |   |
|   |                                                   |   |
|   |  ```                                             |   |
|   |  代码块                                          |   |
|   |  ```                                             |   |
|   |                                                   |   |
|   |  > 引用块                                        |   |
|   |                                                   |   |
|   |  - 列表项                                        |   |
|   |  - 列表项                                        |   |
|   |                                                   |   |
|   +---------------------------------------------------+   |
|                                                          |
|   +-- 文章底部 --------------------------------------+   |
|   |                                                   |   |
|   |  <- 上一篇: 文章标题   |   下一篇: 文章标题 ->   |   |
|   |                                                   |   |
|   +---------------------------------------------------+   |
|                                                          |
+----------------------------------------------------------+
```

### 5.4 响应式断点

```css
:root {
  /* 断点定义 */
  --breakpoint-sm:  640px;
  --breakpoint-md:  768px;
  --breakpoint-lg:  1024px;
  --breakpoint-xl:  1280px;
  --breakpoint-2xl: 1440px;
}

/* 移动端：< 768px */
@media (max-width: 767px) {
  :root {
    --page-padding-x: var(--space-4);
    --content-max-width: 100%;
  }
}

/* 平板端：768px - 1024px */
@media (min-width: 768px) and (max-width: 1023px) {
  :root {
    --page-padding-x: var(--space-6);
    --content-max-width: 640px;
  }
}

/* 桌面端：1024px - 1439px */
@media (min-width: 1024px) and (max-width: 1439px) {
  :root {
    --page-padding-x: var(--space-8);
    --content-max-width: 720px;
  }
}

/* 大屏端：>= 1440px */
@media (min-width: 1440px) {
  :root {
    --page-padding-x: var(--space-12);
    --content-max-width: 720px;
  }
}
```

### 5.5 响应式布局变化

| 区域 | 移动端 (<768px) | 平板端 (768-1024px) | 桌面端 (>1024px) |
|-----|----------------|---------------------|-----------------|
| 导航 | 汉堡菜单，侧边抽屉 | 水平导航，精简项 | 完整水平导航 |
| 内容区 | 单列，全宽 | 单列，居中约束 | 单列，居中约束 |
| 卡片列表 | 垂直堆叠 | 垂直堆叠 | 垂直堆叠 |
| 文章正文 | 100% 宽度 | max-width: 640px | max-width: 720px |
| 侧边目录 | 隐藏 | 隐藏 | 可选显示 |
| 字号 | 基础字号不变 | 基础字号不变 | H1 可放大至 40px |

---

## 六、组件设计

### 6.1 头部导航组件 (Header)

**结构**：
```
Header
├── Logo / 站点名称（左侧）
├── 导航链接（右侧，桌面端水平排列）
│   ├── 首页
│   ├── 归档（可选）
│   └── 关于（可选）
└── 主题切换按钮（右侧）
```

**样式规范**：
- 高度：`64px`（固定）
- 背景：`var(--color-bg-primary)` + `backdrop-filter: blur(12px)`（毛玻璃效果）
- 底部边框：`1px solid var(--color-border-secondary)`
- 内边距：`0 var(--page-padding-x)`
- 定位：`position: sticky; top: 0; z-index: 100`
- Logo 字号：`20px`，字重 `600`
- 导航链接字号：`14px`，字重 `500`，颜色 `var(--color-text-secondary)`
- 导航链接 hover：颜色变为 `var(--color-text-primary)`
- 当前页面链接：底部 `2px` 指示条，颜色 `var(--color-accent-500)`

**移动端适配**：
- 导航链接折叠为汉堡菜单图标（`☰`）
- 点击后从右侧滑入侧边抽屉（宽度 `280px`）
- 抽屉背景遮罩 `rgba(0,0,0,0.3)`
- 抽屉内链接垂直排列，间距 `--space-4`

### 6.2 博客卡片组件 (PostCard)

**结构**：
```
PostCard
├── PostCard__Meta
│   ├── 发布日期
│   └── 阅读时间（可选）
├── PostCard__Title (H2)
├── PostCard__Description (摘要)
├── PostCard__Tags
│   ├── Tag
│   └── Tag
└── PostCard__Link ("阅读全文")
```

**样式规范**：
- 容器：无背景，无边框，无阴影
- 底部边框：`1px solid var(--color-border-secondary)`
- 内边距：`var(--space-6) 0`
- 日期字号：`var(--text-xs)`，颜色 `var(--color-text-tertiary)`
- 标题字号：`var(--text-h3)`，字重 `600`，颜色 `var(--color-text-primary)`
- 摘要字号：`var(--text-base)`，颜色 `var(--color-text-secondary)`，最多显示 2 行（`-webkit-line-clamp: 2`）
- "阅读全文" 字号：`var(--text-sm)`，颜色 `var(--color-accent-500)`，带 `->` 箭头图标

**交互效果**：
- hover 时标题颜色渐变为 `var(--color-accent-500)`
- hover 时 "阅读全文" 箭头向右平移 `4px`
- 过渡时间：`transition: all 0.2s ease`
- 整个卡片可点击（不是仅标题）

### 6.3 文章内容组件 (ArticleContent)

这是 Markdown 渲染后的文章正文区域，需要覆盖所有 Markdown 元素的样式。

**容器**：
- `max-width: var(--content-max-width)`
- `margin: 0 auto`
- `padding: var(--space-12) var(--page-padding-x)`

**标题样式**：

```css
.article-content h1 {
  font-size: var(--text-h1);
  font-weight: var(--weight-bold);
  line-height: var(--leading-tight);
  margin-top: var(--space-12);
  margin-bottom: var(--space-6);
  color: var(--color-text-primary);
  letter-spacing: -0.02em;
}

.article-content h2 {
  font-size: var(--text-h2);
  font-weight: var(--weight-bold);
  line-height: var(--leading-tight);
  margin-top: var(--space-10);
  margin-bottom: var(--space-4);
  color: var(--color-text-primary);
  letter-spacing: -0.01em;
  padding-bottom: var(--space-2);
  border-bottom: 1px solid var(--color-border-secondary);
}

.article-content h3 {
  font-size: var(--text-h3);
  font-weight: var(--weight-semibold);
  line-height: var(--leading-snug);
  margin-top: var(--space-8);
  margin-bottom: var(--space-3);
  color: var(--color-text-primary);
}
```

H4-H6 依此规律递减，不再添加底部边框。

**段落**：
```css
.article-content p {
  font-size: var(--text-base);
  line-height: 1.75;
  margin-bottom: var(--space-5);
  color: var(--color-text-primary);
}
```

**链接**：
```css
.article-content a {
  color: var(--color-link);
  text-decoration: underline;
  text-underline-offset: 3px;
  text-decoration-thickness: 1px;
  transition: color 0.15s ease;
}

.article-content a:hover {
  color: var(--color-link-hover);
}
```

**代码块**：
```css
.article-content pre {
  background-color: var(--color-bg-code);
  border: 1px solid var(--color-border-primary);
  border-radius: 8px;
  padding: var(--space-4) var(--space-5);
  overflow-x: auto;
  margin-bottom: var(--space-6);
  font-family: var(--font-mono);
  font-size: var(--text-sm);
  line-height: 1.6;
}

.article-content code {
  font-family: var(--font-mono);
  font-size: 0.9em;
  background-color: var(--color-bg-code);
  padding: 2px 6px;
  border-radius: 4px;
}

.article-content pre code {
  background: none;
  padding: 0;
  font-size: var(--text-sm);
}
```

**引用块**：
```css
.article-content blockquote {
  border-left: 3px solid var(--color-accent-500);
  padding-left: var(--space-5);
  margin: var(--space-6) 0;
  color: var(--color-text-secondary);
  font-style: italic;
}

.article-content blockquote p:last-child {
  margin-bottom: 0;
}
```

**列表**：
```css
.article-content ul,
.article-content ol {
  padding-left: var(--space-6);
  margin-bottom: var(--space-5);
}

.article-content li {
  margin-bottom: var(--space-2);
  line-height: 1.75;
}

.article-content li::marker {
  color: var(--color-text-tertiary);
}
```

**图片**：
```css
.article-content img {
  max-width: 100%;
  height: auto;
  border-radius: 8px;
  margin: var(--space-6) 0;
}

.article-content figure {
  margin: var(--space-8) 0;
}

.article-content figcaption {
  text-align: center;
  font-size: var(--text-sm);
  color: var(--color-text-tertiary);
  margin-top: var(--space-2);
}
```

**表格**：
```css
.article-content table {
  width: 100%;
  border-collapse: collapse;
  margin: var(--space-6) 0;
  font-size: var(--text-sm);
}

.article-content th {
  text-align: left;
  padding: var(--space-3) var(--space-4);
  border-bottom: 2px solid var(--color-border-primary);
  font-weight: var(--weight-semibold);
  color: var(--color-text-primary);
}

.article-content td {
  padding: var(--space-3) var(--space-4);
  border-bottom: 1px solid var(--color-border-secondary);
  color: var(--color-text-secondary);
}
```

**分割线**：
```css
.article-content hr {
  border: none;
  height: 1px;
  background-color: var(--color-border-primary);
  margin: var(--space-10) 0;
}
```

### 6.4 标签组件 (Tag)

**样式**：
- 显示模式：`inline-flex`
- 字号：`var(--text-xs)`
- 字重：`var(--weight-medium)`
- 内边距：`2px 10px`
- 圆角：`100px`（胶囊形）
- 背景：`var(--color-bg-tertiary)`
- 文字颜色：`var(--color-text-secondary)`
- 边框：无

**交互**：
- hover 时背景变为 `var(--color-accent-50)`
- hover 时文字颜色变为 `var(--color-accent-600)`
- 点击后进入该标签的筛选视图
- 选中状态：背景 `var(--color-accent-500)`，文字白色

### 6.5 分页组件 (Pagination)

**结构**：
```
<nav class="pagination">
  <button class="pagination__prev" disabled>< 上一页</button>
  <div class="pagination__pages">
    <button class="pagination__page pagination__page--active">1</button>
    <button class="pagination__page">2</button>
    <button class="pagination__page">3</button>
    <span class="pagination__ellipsis">...</span>
    <button class="pagination__page">10</button>
  </div>
  <button class="pagination__next">下一页 ></button>
</nav>
```

**样式**：
- 容器居中显示，`margin-top: var(--space-12)`
- 页码按钮：`36px x 36px`，圆角 `8px`
- 当前页码：背景 `var(--color-accent-500)`，文字白色
- 非当前页码：背景透明，文字 `var(--color-text-secondary)`
- hover：背景 `var(--color-bg-tertiary)`
- 禁用状态：`opacity: 0.4; cursor: not-allowed`
- 省略号：`var(--color-text-tertiary)`

### 6.6 返回顶部组件 (BackToTop)

**触发条件**：页面滚动超过 `500px` 后显示

**样式**：
- 固定定位：`position: fixed; bottom: 32px; right: 32px`
- 尺寸：`44px x 44px`
- 背景：`var(--color-bg-primary)`
- 边框：`1px solid var(--color-border-primary)`
- 圆角：`50%`
- 图标：向上箭头 `↑`，字号 `18px`，颜色 `var(--color-text-secondary)`
- 阴影：`0 2px 8px rgba(0,0,0,0.08)`

**交互**：
- 显示/隐藏：`opacity` + `transform: translateY()` 过渡，`0.3s ease`
- hover：背景 `var(--color-bg-tertiary)`，边框颜色 `var(--color-border-primary)` 变深
- 点击：平滑滚动至顶部 `window.scrollTo({ top: 0, behavior: 'smooth' })`

### 6.7 文章导航组件 (PostNavigation)

位于文章底部，用于上/下篇文章导航。

**样式**：
- 容器顶部边框：`1px solid var(--color-border-secondary)`
- 内边距：`var(--space-8) 0`
- 两栏布局（Flexbox），`justify-content: space-between`
- 标签文字：`var(--text-xs)`，颜色 `var(--color-text-tertiary)`，如 "上一篇" / "下一篇"
- 文章标题：`var(--text-base)`，字重 `var(--weight-medium)`，颜色 `var(--color-text-primary)`
- hover 时标题颜色变为 `var(--color-accent-500)`

### 6.8 文章元信息组件 (PostMeta)

显示在文章标题下方。

**结构**：
```
<div class="post-meta">
  <time datetime="2026-05-19">2026年5月19日</time>
  <span class="post-meta__separator">·</span>
  <span class="post-meta__reading-time">约 5 分钟</span>
  <div class="post-meta__tags">
    <span class="tag">标签1</span>
    <span class="tag">标签2</span>
  </div>
</div>
```

**样式**：
- 字号：`var(--text-sm)`
- 颜色：`var(--color-text-tertiary)`
- 分隔符：`margin: 0 var(--space-2)`
- 标签区域：`margin-top: var(--space-3)`

---

## 七、交互设计

### 7.1 页面过渡动画

**页面切换**（SPA 路由切换）：
```css
/* 淡入 */
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(8px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.page-enter {
  animation: fadeIn 0.3s ease forwards;
}
```

- 过渡时长：`300ms`
- 缓动函数：`ease`（`cubic-bezier(0.25, 0.1, 0.25, 1)`）
- 效果：轻微上移 + 透明度渐变
- 避免使用 `slide` 等大幅度动画

### 7.2 悬停效果

**通用悬停规则**：
- 所有可交互元素必须有 hover 状态
- 过渡时间统一：`transition: all 0.15s ease` 或 `0.2s ease`
- 颜色变化使用 CSS `transition`，不使用 JS 动画
- 不使用 `transform: scale()` 放大效果（保持克制）

**具体 hover 效果**：

| 元素 | hover 效果 |
|-----|-----------|
| 链接 | 颜色变深，下划线保持 |
| 卡片标题 | 颜色变为 `var(--color-accent-500)` |
| 按钮 | 背景色变深 5% |
| 标签 | 背景变为 `var(--color-accent-50)` |
| 导航项 | 文字颜色变为 `var(--color-text-primary)` |

### 7.3 点击反馈

- 按钮点击：`transform: scale(0.98)` + `transition: 0.1s`
- 链接点击：无额外动画，依赖浏览器默认行为
- 移动端 tap：使用 `-webkit-tap-highlight-color: transparent` 移除默认高亮

### 7.4 滚动行为

**平滑滚动**：
```css
html {
  scroll-behavior: smooth;
}
```

**导航栏滚动行为**：
- 导航栏始终固定在顶部（`position: sticky`）
- 滚动时导航栏添加底部阴影：`box-shadow: 0 1px 3px rgba(0,0,0,0.05)`
- 可选：向下滚动时隐藏导航栏，向上滚动时显示（通过 JS 监听 scroll 方向）

**文章内锚点滚动**：
- 点击目录锚点时，使用 `scrollIntoView({ behavior: 'smooth', block: 'start' })`
- 滚动偏移量：`scroll-margin-top: 80px`（考虑固定导航栏高度）

### 7.5 加载状态

**文章列表加载**：
- 显示骨架屏（Skeleton）占位
- 骨架屏高度模拟卡片布局
- 使用 `@keyframes` 渐变动画
- 加载完成后骨架屏淡出，内容淡入

**文章内容加载**：
- 显示简短的加载提示文字："正在加载..."
- 使用 `var(--color-text-tertiary)` 颜色
- 加载完成后内容区域淡入

### 7.6 错误状态

**404 页面**：
- 居中显示
- 大号数字 "404"
- 提示文字："页面未找到"
- 返回首页链接

**文章加载失败**：
- 提示文字："文章加载失败，请稍后重试"
- 重试按钮
- 返回列表链接

---

## 八、视觉层次

### 8.1 标题层级

| 层级 | 字号 | 字重 | 行高 | 上间距 | 下间距 | 特殊样式 |
|-----|------|-----|------|-------|-------|---------|
| H1 | 36px | 700 | 1.2 | 48px | 24px | 仅用于文章标题 |
| H2 | 28px | 700 | 1.25 | 40px | 16px | 底部 1px 分隔线 |
| H3 | 22px | 600 | 1.3 | 32px | 12px | 无特殊样式 |
| H4 | 18px | 600 | 1.35 | 24px | 8px | 无特殊样式 |
| H5 | 16px | 600 | 1.4 | 20px | 8px | 颜色偏灰 |
| H6 | 14px | 600 | 1.4 | 16px | 8px | 全大写 + 字间距 |

### 8.2 内容分区

- **一级分区**：通过 `--space-16` (64px) 或 `--space-20` (80px) 的间距分隔
- **二级分区**：通过 `--space-8` (32px) 或 `--space-10` (40px) 的间距分隔
- **三级分区**：通过 `--space-4` (16px) 或 `--space-6` (24px) 的间距分隔
- 可使用 `hr` 分割线（1px `var(--color-border-primary)`）辅助分隔

### 8.3 视觉权重分配

```
高权重 ──────────────────────────────────── 低权重

 H1标题    H2标题    H3标题    正文     辅助文字   占位符
  700       700       600      400       400        400
 #171717   #171717   #171717  #171717   #525252    #A3A3A3
 36px      28px      22px     16px      14px       12px
```

### 8.4 Z-Index 层级规范

```css
:root {
  --z-base:      0;
  --z-dropdown:  10;
  --z-sticky:    20;
  --z-header:    100;
  --z-overlay:   200;
  --z-modal:     300;
  --z-toast:     400;
}
```

---

## 九、Markdown Frontmatter 规范

博客文章的 Markdown 文件需要在头部包含 frontmatter 元数据：

```yaml
---
title: "文章标题"           # 必填 - 显示为 H1
date: 2026-05-19            # 必填 - 发布日期，ISO 格式
description: "文章摘要..."   # 可选 - 卡片列表中显示的摘要
tags: [标签1, 标签2]        # 可选 - 标签数组
draft: false                # 可选 - 是否为草稿，默认 false
---
```

**字段处理规则**：
- `title`：若缺失，使用文件名作为标题
- `date`：若缺失，使用文件的最后修改时间
- `description`：若缺失，截取正文前 150 个字符
- `tags`：若缺失，不显示标签
- `draft: true` 的文章不在列表中显示

---

## 十、CSS 架构

### 10.1 文件组织

```
css/
├── variables.css    # 所有 CSS 自定义属性（颜色、字号、间距、断点等）
├── reset.css        # CSS Reset / Normalize
├── base.css         # 基础元素样式（body, a, p, h1-h6 等）
├── components.css   # 组件样式（Header, PostCard, Tag 等）
├── article.css      # 文章内容渲染样式
├── layout.css       # 布局样式
├── utilities.css    # 工具类（可选）
├── animations.css   # 动画定义
└── responsive.css   # 响应式媒体查询
```

### 10.2 命名规范

采用 BEM（Block Element Modifier）命名规范：

```css
/* Block */
.post-card { }

/* Element */
.post-card__title { }
.post-card__meta { }
.post-card__description { }

/* Modifier */
.post-card--featured { }
.tag--active { }
.pagination__page--current { }
```

---

## 十一、可访问性 (Accessibility)

### 11.1 基本要求

- 所有交互元素可通过键盘 Tab 聚焦
- 聚焦样式可见：`outline: 2px solid var(--color-accent-500); outline-offset: 2px`
- 图片必须有 `alt` 属性
- 链接必须有明确的文字描述（避免 "点击这里"）
- 表单控件必须有关联的 `label`
- 使用语义化 HTML 标签（`<header>`, `<nav>`, `<main>`, `<article>`, `<footer>`）

### 11.2 颜色对比

- 正文文字与背景的对比度不低于 7:1（WCAG AAA）
- 次要文字与背景的对比度不低于 4.5:1（WCAG AA）
- 不仅依赖颜色传达信息（如错误状态同时使用图标和文字）

### 11.3 减少动画

```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

---

## 十二、性能指标

| 指标 | 目标值 |
|-----|-------|
| First Contentful Paint (FCP) | < 1.0s |
| Largest Contentful Paint (LCP) | < 2.0s |
| Cumulative Layout Shift (CLS) | < 0.1 |
| First Input Delay (FID) | < 100ms |
| 首页总资源大小 | < 200KB（不含图片） |
| 字体文件大小 | < 100KB（单个字体子集） |

---

## 十三、暗色模式实现

通过 `data-theme` 属性切换：

```html
<html lang="zh-CN" data-theme="light">
```

切换逻辑：
```javascript
// 检测系统偏好
const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;

// 读取用户偏好
const savedTheme = localStorage.getItem('theme');
const theme = savedTheme || (prefersDark ? 'dark' : 'light');

document.documentElement.setAttribute('data-theme', theme);
```

切换按钮使用太阳/月亮图标 SVG，过渡动画 `0.3s ease`。

---

**文档结束**

本文档覆盖了 BlogSystem 项目从设计原则到具体实现规范的全部内容。开发时应以 `variables.css` 中定义的 CSS 自定义属性为基础，确保所有样式引用这些变量而非硬编码值，从而保证设计系统的一致性和可维护性。
