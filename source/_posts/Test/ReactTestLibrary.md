
---
title: React test library
date: 2021-01-05 16:49:28
tags: test
description: " react test library"
---

[React 测试库](https://testing-library.com/react)构建于[`DOM Testing Library`](https://testing-library.com/docs/dom-testing-library/intro)并扩展了react-dom和react-dom / test-utils，它能有效地避免引入组件的**具体实现**（人为的包装组件中部分代码，进行测试）而破坏测试用例的健壮性，从而让重构工作变得轻而易举，它建议你去测试代码具体的每一部分，而是通过引起组件的变化因素（props, event...） 测试最终的结果。虽然它不能让你省略子元素来浅（shallowly）渲染一个组件，但像 Jest 这样的测试运行器可以通过  [mocking](https://react.docschina.org/docs/testing-recipes.html#mocking-modules)  让你做到。

测试原则：**您的测试越类似于您的软件的使用方式，它们就越能给您信心。**

>一般是终端用户和调用你组件的开发人员使用你的组件。这可以使你的测试避免引入组件的[实现细节](https://kentcdodds.com/blog/testing-implementation-details)，从而有利于重构代码而不破坏测试。
用Enzyme也可以遵循这些准则，但是由于Enzyme提供了所有额外的有助于测试实现详细信息的API，因此很难实施。

## [How to use React Testing Library Tutorial](https://www.robinwieruch.de/react-testing-library)
示例： http://blog.itpub.net/31559758/viewspace-2682344/




