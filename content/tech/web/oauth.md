---
title: OAuth 流程
date: 2017-11-18T16:49:59+08:00
categories: web
---

## 前言

OAuth 允许第三方应用程序以用户的身份与服务提供者进行交互，
而在这个过程中应用程序并不需要知道用户在服务提供者那里保存的密码。

这其中涉及到用户，应用程序与服务提供者之间的多次信息交互。
最近读到一篇深入浅出的文章，很有代入感，遂记录于此。

## OAuth 中的三角关系

OAuth 涉及三个角色：

- 用户（user）
- 第三方应用程序（consumer）
- 服务提供者（service provider）

在下面的例子中，假设第三方应用程序是大众点评手机客户端，服务提供者是微信。
当你在一家环境优雅的餐厅大快朵颐之后，在手机上写下点评，并希望发布到微信朋友圈，
这时候将涉及以下一系列 OAuth 流程：

## OAuth 流程

![](/images/tech/oauth-process.png)

## 回顾

服务提供者返回 secret 以用来防止伪造请求，第三方应用程序使用 secret 对每个请求进行签名，
这样服务提供者就可以验证请求的合法性。

在 Step 3 中，对于第三方应用程序重定向的网站，用户必须检查其正确性，
因为如果第三方应用程序搞一个和服务提供者长得一样的网站，而用户又输入了用户名和密码，那就完蛋了。

在 Step 4 中，服务提供者将 request token 标记为 `good-to-go` ，
这样，当第三方应用程序后续请求 access token 时，就会被允许，
当然前提是这个请求是合法的（被正确的 secret 签名）。


## 参考
[Introduction to OAuth (in Plain English)](https://blog.varonis.com/introduction-to-oauth/)