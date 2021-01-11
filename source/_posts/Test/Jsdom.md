
---
title: My New Post
date: 2020-11-30 10:49:28
tags:
description: " 大家好，this is description "
---

## what?
[`jsdom`](https://github.com/jsdom/jsdom) 是一个运行在 Node.js 内的许多web标准(特别是WHATWG DOM和HTML标准)的纯javascript实现。该项目的目标是模拟足够多的web浏览器子集（不能真实渲染），以便对测试和抓取真实的web应用程序有用，不具备如[布局和导航](https://github.com/jsdom/jsdom#unimplemented-parts-of-the-web-platform)的功能。

因为它的运行比为每个测试启动浏览器的方式效率更高。适用于大多数基于 Web 的组件测试，并且它与你编写的测试运行在同一个进程中，所以你能够编写代码来检查和断言渲染的 DOM。

如果您正在编写一个主要测试浏览器特定行为的库，并且需要布局或真实输入等原生浏览器行为，那么你可以使用像 [mocha](https://mochajs.org/) 这样的框架。
