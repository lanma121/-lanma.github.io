
---
title: 端对端测试
date: 2020-01-11 10:49:28
tags: test
description: " 端对端测试(end to end test)"
---

端对端测试对于测试更长的工作流程非常有用，特别是当它们对于你的业务（例如付款或注册）特别重要时。对于这些测试，你可能会希望测试真实浏览器如何渲染整个应用、从真实的 API 端获取数据、使用 session 和 cookies 以及在不同的链接间导航等功能。你可能还希望不仅在 DOM 状态上进行断言，而同时也在后端数据上进行校验（例如，验证更新是否已经在数据库中持久化）。

在这种场景下，你可以使用像  [Cypress](https://www.cypress.io/)  这类框架或者如  [puppeteer](https://github.com/GoogleChrome/puppeteer)  这样的库，这样你就可以在多个路由之间导航切换，并且不仅能够在浏览器中对副作用进行断言也能够在后端这么做。
