---
title: Zola 无法引入 style.css
date: 2025-08-25
tags:
---

> 在 Zola 中引入 CSS、JS、图片等静态资源时，结果浏览器报 404，资源无法加载。

<!-- more -->

### 原因分析

- Zola 的 `static/` 文件夹不是网站访问路径的前缀，而是**存放静态资源的源目录**。
- 构建或运行 `zola serve` 后，`static/` 里的文件会被原封不动地复制到网站根目录。
- 因此，访问路径**不包含 `static/`**。

---

### 正确做法

1. 文件放置：

```
your-zola-site/
├── content/
├── templates/
└── static/
     └── style.css
```

2. HTML 引入：

```html
<link rel="stylesheet" href="/style.css" />
```

> 注意：路径从网站根开始，不要写 `static/`。

3. 构建或运行：

```bash
zola serve
```

- 然后在浏览器访问 `http://127.0.0.1:1111/`，CSS 会生效。

---

### 小提示

- 任何放在 `static/` 的文件，都以 `/` 开头引用。
- 如果有子目录，也按网站根路径引用，例如 `/images/logo.png`。
