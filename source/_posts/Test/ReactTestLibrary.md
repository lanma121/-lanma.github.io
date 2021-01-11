
---
title: React test library
date: 2021-01-05 16:49:28
tags: test, react
description: " react test library"
---

**[React 测试库](https://testing-library.com/react)**构建于[`DOM Testing Library`](https://testing-library.com/docs/dom-testing-library/intro)并扩展了react-dom和react-dom / test-utils，是一组能让你不依赖 React 组件具体实现对他们进行测试的辅助工具。它让重构工作变得轻而易举，还会推动你拥抱有关无障碍的最佳实践。虽然它不能让你省略子元素来浅（shallowly）渲染一个组件，但像 Jest 这样的测试运行器可以通过  [mocking](https://react.docschina.org/docs/testing-recipes.html#mocking-modules)  让你做到。

测试原则：**您的测试越类似于您的软件的使用方式，它们就越能给您信心。**
1. 如果涉及渲染组件，则它应处理DOM节点而不是组件实例。（）
2. 该库鼓励测试以预期的方式使用组件。通常，以用户使用它的方式测试应用程序组件将非常有用。
3. 该库的实现和api应该简单而灵活。

>这可以使你的测试避免引入组件的[实现细节](https://kentcdodds.com/blog/testing-implementation-details)，从而有利于重构代码而不破坏测试。
用Enzyme也可以遵循这些准则，但是由于Enzyme提供了所有额外的有助于测试实现详细信息的API，因此很难实施。

## [How to use React Testing Library Tutorial](https://www.robinwieruch.de/react-testing-library)
示例： http://blog.itpub.net/31559758/viewspace-2682344/




