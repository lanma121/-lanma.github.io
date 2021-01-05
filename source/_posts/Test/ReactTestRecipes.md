
---
title:  React 测试技巧实例
date: 2021-01-05 17:49:28
tags: react, test
description: " react 测试模式实例 "
---

-   [创建/清理](https://react.docschina.org/docs/testing-recipes.html#setup--teardown)
对于每个测试，即使测试失败，也需要执行清理。否则，测试可能会导致“泄漏”，并且一个测试可能会影响另一个测试的行为。比如，我们通常希望将 React 树渲染给附加到  `document`的 DOM 元素。以便它可以接收 DOM 事件。当测试结束时，我们需要“清理”并从  `document`  中卸载树。
-   [`act()`](https://react.docschina.org/docs/testing-recipes.html#act)
在编写 UI 测试时，可以将渲染、用户事件或数据获取等任务视为与用户界面交互的“单元”。React 提供了一个名为  `act()`  的 helper，它确保在进行任何断言之前，与这些“单元”相关的所有更新都已处理并应用于 DOM：
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
