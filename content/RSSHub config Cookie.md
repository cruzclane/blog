---
title: 如何为 RSSHub 配置 Cookie
date: 2025-11-03
type:
tags:
---

由于国内平台（知乎、bilibili、豆瓣）严格的反爬机制，RSSHub 必须配置**有效**的 Cookie 才可以正常使用

本文记录配置过程，仅供参考
### 1. 获取 Cookie

- 哔哩哔哩 （登录账号，F12 打开 DevTool > Network）
  - 需要获取的参数 `uid` 和 `SESSDATA`
  - ![bilibili cookie for rsshub 20251103_1513.png](https://pub-f32dde7eb8b242fa96072c7908c17ffa.r2.dev/blog/202511/16/bilibili%20cookie%20for%20rsshub%2020251103_1513.png?GO35H1r8oO)
- 知乎（登录账号，F12 打开 DevTool > Network）
  - 需要获取的参数 `z_c0` 、`d_c0` 和 `__zse_ck`
  - ![zhihu cookie for rsshub 20251103_1520.png](https://pub-f32dde7eb8b242fa96072c7908c17ffa.r2.dev/blog/202511/16/zhihu%20cookie%20for%20rsshub%2020251103_1520.png?n73T1lrfiK)

### 2. 修改 `docker-compose.yml`

```yml
environment:
  # 自行修改`{}`里的内容
  BILIBILI_COOKIE_{UID}: 'SESSDATA={value}'
  ZHIHU_COOKIES: 'z_c0={value_1}; d_c0={value_2}; __zse_ck={value_3}' 
  # 双引号内的 Cookie 不要为了排版，手动换行，YAML 会解析失败
```

### 3. 重启 Docker

```sh
docker-compose up -d
```

### 注意：

> **验证 Cookie 是否有效**：访问  `/zhihu/check-cookie`  路由，返回  `code: 0`  表示有效，否则需要重新抓取 cookie。

测试发现无论是否配置 Cookie 成功，始终返回 NotFound

**Reference**

- [知乎路由又一次 403 · Issue #20402 · DIYgod/RSSHub](https://github.com/DIYgod/RSSHub/issues/20402#issuecomment-3474256184)
