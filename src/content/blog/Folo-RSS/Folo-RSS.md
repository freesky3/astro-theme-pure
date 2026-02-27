---
title: 在Folo中用RSS订阅一切
publishDate: 2026-02-22 21:02:00
description: '信息流减法'
tags:
  - RSS
heroImage: { src: './RSS.png', color: '#7E787D' }
language: '中文'
---

对于bilibili的教程：[Bilibili用户RSS订阅源获取教程：解决无法订阅与失效问题的终极指南 - 云原生实践](https://www.oryoy.com/news/bilibili-yong-hu-rss-ding-yue-yuan-huo-qu-jiao-cheng-jie-jue-wu-fa-ding-yue-yu-shi-xiao-wen-ti-de-zh.html)

使用`https://rsshub.app/bilibili/user/video/{uid}`订阅用户投稿



对于youtube：使用`rsshub://youtube/user/{uid}`, uid为@后面的

或者`https://www.youtube.com/feeds/videos.xml?channel_id={查看网页URL}`

`https://www.youtube.com/feeds/videos.xml?playlist_id=PLCgD3ws8aVdolCexlz8f3U-RROA0s5jWA`



#### 1. 订阅“合集” (Collection)

**公式**：`http://你的服务器IP:1200/bilibili/user/collection/UID/SID`

* **UID**：UP 主的数字 ID。
* **SID**：你在地址栏看到的 `collectiondetail?sid=` 后面的数字。

#### 2. 订阅“视频列表” (Series)

**公式**：`http://你的服务器IP:1200/bilibili/user/series/UID/SID`

* **UID**：UP 主的数字 ID。
* **SID**：你在地址栏看到的 `seriesdetail?sid=` 后面的数字。

特定视频集合如：`http://你的服务器IP:1200/bilibili/video/page/BV1LD6BBPEjt`



RSS参数教程：https://docs.rsshub.app/zh/guide/parameters



---

对于微信公众号：

```
docker run -d \
  --name wewe-rss \
  -p 4000:4000 \
  -e DATABASE_TYPE=sqlite \
  -e AUTH_CODE={设置} \
  -v $(pwd)/data:/app/data \
  cooderl/wewe-rss-sqlite:latest
```

之后防火墙放行4000端口

登录之后，输入文章链接，可以自动生成对应公众号的RSS Feed，最后通过.opml文件导出

---

对于知乎：

A. 订阅知乎专栏

* **路由格式**：`/zhihu/zhuanlan/:id`
* **如何获取 ID**：打开专栏网页，URL 末尾的部分即为 ID。例如专栏 `https://zhuanlan.zhihu.com/p/12345`，ID 就是 `12345`。
* **你的 Feed 链接**：`http://你的服务器IP:1200/zhihu/zhuanlan/12345`

B. 订阅特定用户的动态

* **路由格式**：`/zhihu/people/activities/:id`
* **如何获取 ID**：用户个人主页 URL 中 `people/` 后面的部分。例如：`https://www.zhihu.com/people/excited-vczh`，ID 为 `excited-vczh`。
* **你的 Feed 链接**：`http://你的服务器IP:1200/zhihu/people/activities/excited-vczh`

C. 订阅某个问题的回答

* **路由格式**：`/zhihu/question/:questionId`
* **你的 Feed 链接**：`http://你的服务器IP:1200/zhihu/question/12345678`

D. 知乎热榜

* **路由格式**：`/zhihu/hotlist`
* **你的 Feed 链接**：`http://你的服务器IP:1200/zhihu/hotlist`



反反爬虫：

```
vim /opt/rsshub/docker-compose.yml
```

用vim编辑，加入知乎cookie：`\- ZHIHU_COOKIES="_xsrf=1Hkej...这里是你那串极长的内容...; BEC=63a63..."`



之后`docker compose up -d`让 Docker 重新读取配置并重启容器
