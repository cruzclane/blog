---
title: 为什么使用 Zola 搭建 Blog
date: 2024-08-04
---

## 什么是 Zola?

> Zola is a static site generator (SSG), similar to [Hugo](https://gohugo.io/), [Pelican](https://blog.getpelican.com/), and [Jekyll](https://jekyllrb.com/) (for a comprehensive list of SSGs, please see [Jamstack](https://jamstack.org/generators)). It is written in [Rust](https://www.rust-lang.org/) and uses the [Tera](https://keats.github.io/tera/) template engine, which is similar to [Jinja2](https://jinja.palletsprojects.com/en/2.10.x/), [Django templates](https://docs.djangoproject.com/en/2.2/topics/templates/), [Liquid](https://shopify.github.io/liquid/), and [Twig](https://twig.symfony.com/).
>
> - [zola 官方文档](https://www.getzola.org/documentation/getting-started/overview/)

Zola 是一款静态网站生成框架, 如果你在 [GitHub 检索 static-site-generator](https://github.com/topics/static-site-generator), 发现 Zola 排名竟然在第13位.

## 为什么是 Zola?

1. 依赖简单, 得益于 Rust 语言特性, 安装环境只需要一个可执行文件.

2. 文件结构简单, 以及命令简单, 这也是为什么不选择 Hugo 的原因.

   ```bash
    .
    ├── content
    ├── static
    ├── templates
    └── config.toml
    3 directories, 1 file
   ```
   
3. Zola 提供丰富易用的主题 theme, 保证你可以实现开箱即用.

## 推荐谁来使用 Zola?

- 掌握基础的 `HTML` `CSS`.
- 喜欢极简风格, 喜欢动手, 你一定可以搭建一个属于自己的 Blog.

## 我的使用体验?

所有内容上传 GitHub 仓库, 使用 Cloudflare Pages 部署, 只需`git commit` 就可以更新网站.

我使用 Typora 编辑 content 文件夹的 md 文件, `zola serve` 可以实现实时预览, 甚至不需要手动保存, 省时省心省力, 写作成本大大降低.

## 谁在用 Zola 搭建 Blog?

- [迁移博客和Wiki到 Zola - owen](https://www.owenyoung.com/blog/migrate-to-zola/#overview) 
- [I spent one week with Zola](https://www.kytta.dev/blog/one-week-with-zola/)