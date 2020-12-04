---
title: React Hook - useEffect
date: 2020-11-30 14:15:43
descript: React Hook, useEffect, 副作用
tags:
---

# React Hook - useEffect

## What?
用以替代render 之后执行副作用的`componentDidMount` 和 `componentDidUpdate`生命周期函数。
React 会记录传入useEffect的副作用函数，在DOM更新后调用它。 

## Why?
 1. 为了在函数式组件执行[副作用](./SideEffect)
 2. 减少类组件生命周期对相同逻辑的调用
 3. 把相互关联的副作用放到一起，比如事件订阅和取消，直接返回一个取消方法，React 会在取消的时候调用它。
 4. 可以把多个副作用分割到不同的effets中，避免混杂多个不相关的逻辑
 5. 每次调用都会清理上一次的render, 以减少bug, [看官方实例](https://reactjs.org/docs/hooks-effect.html#explanation-why-effects-run-on-each-update)

## How?
```js
let  freinds = {};
const  ChatAPI = {
	subscribeToFriendStatus: (id, callback) => {
		freinds[id] = { interval:  null, times:  0, };
		freinds[id].interval = setInterval(() => {
		callback(freinds[id].times % 2 === 0 ? true: false);
		freinds[id].times++;
		}, 3000);
	},
	unsubscribeFromFriendStatus: (id, callback) => {
		clearInterval(freinds[id].interval);
		delete  freinds[id];
	}
}

export  function  FriendStatusWithCounter(props) {
	const [count, setCount] = useState(0);
	// Effect Without Cleanup
	useEffect(() => {
		console.log("只有count 改变才走："+count);
		document.title = `You clicked ${count} times`;
	}, [count]); // 仅count 被改变执行effect

	const [isOnline, setIsOnline] = useState(null);
	// Effect With Cleanup
	useEffect(() => {
		console.log("props.friend.id 改变才走："+props.friend.id);
		function  handleStatusChange(isOnline) {
			setIsOnline(isOnline);
		}
		// 添加订阅
		ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
		// 取消订阅
		return () => {
			ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
		};
	}, [props.friend.id]);

  

	return (
		<div>
			<h1>  {  isOnline ? 'Online' : 'Offline'  }</h1>
			<p>You clicked {count} times</p>
			<button  onClick={() =>  setCount(count + 1)}>Click me</button>
		</div>
	);

}
```

## When

## Notes
 1. 每次调用useEffect都会给effect传入一个新的函数，以同步函数式组件内部的变量（state, props）, 如果不调用则永远保留上一次的值。
 2. useEffect 可以以数组的方式传入第二个参数
     
        如果传入的变量没有改变，跳过effect 执行；
        如果传入空数组[],则仅在装载阶段执行一次；
        如果不传入，每次render都会执行；
 3. 
 4. 
 5. 

## 怎样优化依赖频繁改变调用effect
1. 用[functional update form of  `setState`](https://reactjs.org/docs/hooks-reference.html#functional-updates)
```js
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      setCount(c => c + 1); // ✅ 用function的形式更新state,第一个参数是上一次的state
    }, 1000);
    return () => clearInterval(id);
  }, []); // ✅ []只在初始化使调用一次,
  return <h1>{count}</h1>;
}
```
2. 用[useRef](./UseRef)保留可变值
```js
export  function  Counter() {
	const [count, setCount] = useState(0);
	const  countRef = useRef(count);
	
	useEffect(() => {
		const  id = setInterval(() => {
			countRef.current++;
			// ✅ useRef每次render都返回相同的对象，可以保留可变值到整个生命周期
			setCount(countRef.current); 
		}, 1000);
		return () =>  clearInterval(id);
	}, []); // ✅ []只在初始化使调用一次,
	return  <h1>{count}</h1>;
}
```
3. 


## 与`componentDidMount` or `componentDidUpdate`区别
useEffect 不会阻止浏览器更新屏幕，如果需要同步处理（比如计算layout）,可以用[`useLayoutEffect`](./UseLayoutEffect)
 
 

