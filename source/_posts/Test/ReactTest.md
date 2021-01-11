
---
title: React Test
date: 2021-01-05 14:49:28
tags: test
description: " React test"
---

## 测试 React 组件的方法
-   **渲染组件树**  在一个简化的测试环境中渲染组件树并对它们的输出做断言检查。
-   **运行完整应用**  在一个真实的浏览器环境中运行整个应用（也被称为“端到端（end-to-end）”测试）。

## 怎样权衡测试工具
-   **迭代速度 vs 真实环境：**  一些工具在做出改动和看到结果之间提供了非常快速的反馈循环，但没有精确的模拟浏览器的行为。另一些工具，也许使用了真实的浏览器环境，但却降低了迭代速度，而且在持续集成服务器中不太可靠。
-   **mock 到什么程度：**  对组件来说，“单元测试”和“集成测试”之间的差别可能会很模糊。如果你在测试一个表单，用例是否应该也测试表单里的按钮呢？一个按钮组件又需不需要有他自己的测试套件？重构按钮组件是否应该影响表单的测试用例？

## React测试库

 1. [Test Utilities](https://react.docschina.org/docs/test-utils.html)
`ReactTestUtils` 可搭配你所选的测试框架(比如jest，可以使用jsdom)，轻松实现 React 组件测试
2. [Test Renderer](https://react.docschina.org/docs/test-renderer.html)
这个 package 提供了一个 React 渲染器，用于将 React 组件渲染成纯 JavaScript 对象，无需依赖 DOM 或原生移动环境。
这个 package 提供的主要功能是在不依赖浏览器或  [jsdom](https://github.com/tmpvar/jsdom)  的情况下，返回某个时间点由 React DOM 或者 React Native 平台渲染出的视图结构（类似与 DOM 树）快照。
3. [Enzyme](https://airbnb.io/enzyme/)
4. [React Testing Library](https://testing-library.com/react)，

## 测试什么 
 要回答这个问题， 谁是这个code的用户？终端用户和developers，终端用户会点击button，render不同的内容。developers会更新props，render 不同的内容。所以我们的测试入口就是 props 和 与用户交互的事件，出口就是render 的内容，这也是React Testing Library 的设计理念。这样就会避免[细节测试](https://kentcdodds.com/blog/testing-implementation-details)所引起的无效测试。
- False negative:  测试通过，render 不一定正
-  False positive:  测试不通过，render 可能正确。
1.  What part of your untested codebase would be really bad if it broke? (The checkout process)
2. 将使用说明转换成测试案例，写进自动化测试
