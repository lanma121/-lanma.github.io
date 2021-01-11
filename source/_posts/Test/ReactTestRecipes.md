
---
title:  React 测试技巧实例
date: 2021-01-05 17:49:28
tags: react, test
description: " react 测试模式实例 "
---

## Test Utilities 库
-  `isElement()`
    当  `element`  是任何一种 React 元素时，返回  `true`。

- [`isElementOfType(element, componentClass)`](https://react.docschina.org/docs/test-utils.html#iselementoftype)
  当  `element`  是一种 React 元素，并且它的类型是参数  `componentClass`  的类型时，返回  `true`。

- [`isDOMComponent(instance)`](https://react.docschina.org/docs/test-utils.html#isdomcomponent)
 当  `instance`  是一个 DOM 组件（比如  `<div>`  或  `<span>`）时，返回  `true`。

- [`isCompositeComponent(instance)`](https://react.docschina.org/docs/test-utils.html#iscompositecomponent)
当  `instance`  是一个用户自定义的组件，比如一个类或者一个函数时，返回  `true`。

- [`isCompositeComponentWithType(instance, componentClass)`](https://react.docschina.org/docs/test-utils.html#iscompositecomponentwithtype)
 当  `instance`  是一个组件，并且它的类型是参数  `componentClass`  的类型时，返回  `true`。

----------
- [`findAllInRenderedTree(tree,test)`](https://react.docschina.org/docs/test-utils.html#findallinrenderedtree)
遍历所有在参数  `tree`  中的组件，记录所有  `test(component)`  为  `true`  的组件。单独调用此方法不是很有用，但是它常常被作为底层 API 被其他测试方法使用。

- [`scryRenderedDOMComponentsWithClass(tree,className)`](https://react.docschina.org/docs/test-utils.html#scryrendereddomcomponentswithclass)
查找渲染树中组件的所有 DOM 元素，这些组件是 css 类名与参数  `className`  匹配的 DOM 组件。

- [findRenderedDOMComponentWithClass(tree, className)](https://react.docschina.org/docs/test-utils.html#findrendereddomcomponentwithclass)
用法与  [`scryRenderedDOMComponentsWithClass()`](https://react.docschina.org/docs/test-utils.html#scryrendereddomcomponentswithclass)  保持一致，但期望仅返回一个结果。不符合预期的情况下会抛出异常。

- [scryRenderedDOMComponentsWithTag(tree,tagName)](https://react.docschina.org/docs/test-utils.html#scryrendereddomcomponentswithtag)
查找渲染树中组件的所有的 DOM 元素，这些组件是标记名与参数  `tagName`  匹配的 DOM 组件。

- [`findRenderedDOMComponentWithTag(tree,tagName)`](https://react.docschina.org/docs/test-utils.html#findrendereddomcomponentwithtag)
用法与  [`scryRenderedDOMComponentsWithTag()`](https://react.docschina.org/docs/test-utils.html#scryrendereddomcomponentswithtag)  保持一致，但期望仅返回一个结果。不符合预期的情况下会抛出异常。

-  [scryRenderedComponentsWithType(tree,componentClass)](https://react.docschina.org/docs/test-utils.html#scryrenderedcomponentswithtype)
查找组件类型等于  `componentClass`  组件的所有实例。

- [`findRenderedComponentWithType(tree,  componentClass)`](https://react.docschina.org/docs/test-utils.html#findrenderedcomponentwithtype)
用法与  [`scryRenderedComponentsWithType()`](https://react.docschina.org/docs/test-utils.html#scryrenderedcomponentswithtype)  保持一致，但期望仅返回一个结果。不符合预期的情况下会抛出异常。

- [`renderIntoDocument(element)`](https://react.docschina.org/docs/test-utils.html#renderintodocument)`renderIntoDocument()`
渲染 React 元素到 document 中的某个单独的 DOM 节点上。**这个函数需要一个 DOM 对象。**  它实际相当于：
	```js
	const domContainer = document.createElement('div');
	ReactDOM.render(element, domContainer);
	```
> 注意：
> 
> 你需要在引入  `React`  **之前**确保  `window`  存在，`window.document`  和  `window.document.createElement`  能在全局环境中获取到。不然 React 会认为它没有权限去操作 DOM，以及像  `setState`  这样的方法将不可用。

- Simulate.{eventName}( element,[eventData])
使用可选的 `eventData` 事件数据来模拟在 DOM 节点上触发事件。
	```js
	// <input ref={(node) => this.textInput = node} />
	const node = this.textInput;
	node.value = 'giraffe';
	ReactTestUtils.Simulate.change(node);
	ReactTestUtils.Simulate.keyDown(node, {key: "Enter", keyCode: 13, which: 13});
	```

-   [创建/清理](https://react.docschina.org/docs/testing-recipes.html#setup--teardown)
对于每个测试，即使测试失败，也需要执行清理。否则，测试可能会导致“泄漏”，并且一个测试可能会影响另一个测试的行为。比如，我们通常希望将 React 树渲染给附加到  `document`的 DOM 元素。以便它可以接收 DOM 事件。当测试结束时，我们需要“清理”并从  `document`  中卸载树。
-   [`act()`](https://react.docschina.org/docs/testing-recipes.html#act)
act 方法能在断言之前确保因渲染、用户事件或数据获取等任务而引起的所有更新都已处理并应用于 DOM。

	由react 外部调用栈引起的函数式组件变化，会显示 `act(...)`  	warning（`类组件由于被大量使用，会对遗留的代码展示太多的warning， 所以被react team 忽略掉了`）。
这个warning 的发生是为了防止一些事情导致组件产生不期望的变化。如果需要变化，我们用act去包装，明确告诉react。这样能保证我们测试的准确性。

	在react 调用栈内部跑的code引起的变化，React 会自动处理这个warning，像点击事件直接更新state。但是不能处理react  调用栈外部引起的组件变化，比如：
	1. 异步代码完成后的更新
	2. 使用`jest`的Timers 方法
	3. 自定义 Hooks
	4. 使用 `useImperativeHandle`
	参考：[Fix the "not wrapped in act(...)" warning (kentcdodds.com)](https://kentcdodds.com/blog/fix-the-not-wrapped-in-act-warning)

-   [渲染](https://react.docschina.org/docs/testing-recipes.html#rendering)
测试组件对于给定的 prop 渲染是否正确。此时应考虑实现基于 prop 渲染消息的简单组件。
-   [数据获取](https://react.docschina.org/docs/testing-recipes.html#data-fetching)
-   [mock 模块](https://react.docschina.org/docs/testing-recipes.html#mocking-modules)
-   [事件](https://react.docschina.org/docs/testing-recipes.html#events)
-   [计时器](https://react.docschina.org/docs/testing-recipes.html#timers)
-   [快照测试](https://react.docschina.org/docs/testing-recipes.html#snapshot-testing)
-   [多渲染器](https://react.docschina.org/docs/testing-recipes.html#multiple-renderers)
-   [缺少什么?](https://react.docschina.org/docs/testing-recipes.html#something-missing)

----------

```jsx
// user.js
import  React, { useState, useEffect } from  "react";
import  GoogleMap  from  "./map";
export  default  function  User(props) {
	const [user, setUser] = useState(null);
	const [state, setState] = useState(false);

	async  function  fetchUserData(id) {
		const  response = await  fetch("/" + id);
		setUser(await  response.json());
	}
	useEffect(() => {
		fetchUserData(props.id);
	}, [props.id]);
	useEffect(() => {
		const  timeoutID = setTimeout(() => {
			props.onSelect(null);
		}, 5000);
		return () => {
			clearTimeout(timeoutID);
		};
	}, [props.onSelect]);

	return (
		<div>
		{
			props.name
			? <h1>你好，{props.name}！</h1>:
			<span>嘿，陌生人</span>
		}
		{
			props.id && user ? (
			<details>
				<summary>{user.name}</summary>
				<strong>{user.age}</strong> 岁
				<br  />
				住在 {user.address}
			</details>
			) : null
		}

		<div  id="script-loader">
			<GoogleMap  id="example-map"  center={props.center}  />
		</div>

		<button
			onClick={() => {
				setState(previousState  => !previousState);
				props.onChange(!state);
			}}
			data-testid="toggle"
		>
			{state === true ? "Turn off" : "Turn on"}
		</button>
		{
			[1, 2, 3, 4].map(choice  => (
				<button
				key={choice}
				data-testid={choice}
				onClick={() =>  props.onSelect(choice)}
				>
				{choice}
				</button>
			))
		}
	</div>);
}
```

```jsx
// user.test.js
import  React  from  "react";
import { render, unmountComponentAtNode } from  "react-dom";
import { act } from  "react-dom/test-utils";
import  pretty  from  "pretty";
import  User  from  "./user";
 
let  container = null;
beforeEach(() => {
	// 创建一个 DOM 元素作为渲染目标
	container = document.createElement("div");
	document.body.appendChild(container);
});

afterEach(() => {
	// 退出时进行清理
	unmountComponentAtNode(container);
	container.remove();
	container = null;
});

it("渲染 - 名称", () => {
	act(() => {
		render(<User  />, container);
	});
	expect(container.textContent).toBe("嘿，陌生人");
	 
	act(() => {
		render(<User  name="Jenny"  />, container);
	});
	expect(container.textContent).toBe("你好，Jenny！");
});

it("mock 数据获取 ", async () => {
	const  fakeUser = {
		name:  "Joni Baez",
		age:  "32",
		address:  "123, Charming Avenue"
	};

	jest.spyOn(global, "fetch").mockImplementation(() =>
		Promise.resolve({
			json: () =>  Promise.resolve(fakeUser)
		})
	);
	// 使用异步的 act 应用执行成功的 promise
	await  act(async () => {
		render(<User  id="123"  />, container);
	});
	expect(container.querySelector("summary").textContent).toBe(fakeUser.name);

	expect(container.querySelector("strong").textContent).toBe(fakeUser.age);

	expect(container.textContent).toContain(fakeUser.address);

	// 清理 mock 以确保测试完全隔离
	global.fetch.mockRestore();
});

it("mock 模块", () => {
	jest.mock("./map", () => {
		return  function  DummyMap(props) {
			return (
				<div  data-testid="map">
					{props.center.lat}:{props.center.long}
				</div>
			);
		};
	});

	const  center = { lat:  0, long:  0 };
	act(() => {
		render(<User center={center}/>,container);
	});
	expect(container.querySelector('[data-testid="map"]').textContent).toEqual("0:0");
});

it("Events", () => {
	const  onChange = jest.fn();
	act(() => {
		render(<User  onChange={onChange}  />, container);
	});
	// 获取按钮元素，并触发点击事件
	const  button = document.querySelector("[data-testid=toggle]");
	expect(button.innerHTML).toBe("Turn on");
	
	// 创建的每个事件中传递 { bubbles: true } 才能到达 React 监听器，因为 React 会自动将事件委托给 root。
	act(() => {
		button.dispatchEvent(new  MouseEvent("click", { bubbles:  true }));
	});

	expect(onChange).toHaveBeenCalledTimes(1);
	expect(button.innerHTML).toBe("Turn off");
	
	act(() => {
		for (let  i = 0; i < 5; i++) {
			button.dispatchEvent(new  MouseEvent("click", { bubbles:  true }));
		}
	});
	
	expect(onChange).toHaveBeenCalledTimes(6);
	expect(button.innerHTML).toBe("Turn on");

});

it("计时器 - 超时后应选择 null", () => {
	const  onSelect = jest.fn();
	act(() => {
		render(<User  onSelect={onSelect}  />, container);
	});

	// 提前 100 毫秒执行
	act(() => {
		jest.advanceTimersByTime(100);
	});
	expect(onSelect).not.toHaveBeenCalled();

	// 然后提前 5 秒执行
	act(() => {
		jest.advanceTimersByTime(5000);
	});
	expect(onSelect).toHaveBeenCalledWith(null);
});

it("计时器 - 移除时应进行清理", () => {
	const  onSelect = jest.fn();
	act(() => {
		render(<User  onSelect={onSelect}  />, container);
	});
	act(() => {
		jest.advanceTimersByTime(100);
	});
	expect(onSelect).not.toHaveBeenCalled();
	
	// 卸载应用程序
	act(() => {
		render(null, container);
	});
	act(() => {
		jest.advanceTimersByTime(5000);
	});
	expect(onSelect).not.toHaveBeenCalled();
});

it("计时器 - 应接受选择", () => {
	const  onSelect = jest.fn();
	act(() => {
		render(<User  onSelect={onSelect}  />, container);
	});
	act(() => {
		container
		.querySelector("[data-testid='2']")
		.dispatchEvent(new  MouseEvent("click", { bubbles:  true }));
	});
	expect(onSelect).toHaveBeenCalledWith(2);
});

it("快照测试", () => {
	act(() => {
		render(<User  />, container);
	});
	expect(
		pretty(container.innerHTML)
	).toMatchInlineSnapshot(); /* ... 由 jest 自动填充 ... */
	 
	act(() => {
		render(<User  name="Jenny"  />, container);
	});
	expect(
		pretty(container.innerHTML)
	).toMatchInlineSnapshot(); /* ... 由 jest 自动填充 ... */
});
```
