- useContext
- useReducer
- useCallback
- useMemo
- useRef
- useImperativeMethods
- useMutationEffect
- useLayoutEffect





你还在为该使用无状态组件（Function）还是有状态组件（Class）而烦恼吗？——拥有了hooks，你再也不需要写Class了，你的所有组件都将是Function。

你还在为搞不清使用哪个生命周期钩子函数而日夜难眠吗？——拥有了Hooks，生命周期钩子函数可以先丢一边了。

你在还在为组件中的this指向而晕头转向吗？——既然Class都丢掉了，哪里还有this？你的人生第一次不再需要面对this。

**想要复用一个有状态的组件太麻烦了！**



#### useState

渲染属性（Render Props）和高阶组件（Higher-Order Components）。

渲染属性指的是使用一个值为函数的prop来传递需要动态渲染的nodes或组件。

高阶组件这个概念就更好理解了，说白了就是一个函数接受一个组件作为参数，经过一系列加工后，最后返回一个新的组件。



##### 声明一个状态变量

```
import {useState} from 'react';
function Example(){
	const [count, setCount] = useState(0);
}
```

useState是react自带的一个hook函数，它的作用就是用来声明状态变量。 useState这个函数接收的参数是我们的状态初始值（initial state），它返回了一个数组，这个数组的第 [0]项是当前当前的状态值，第 [1]项是可以改变状态值的方法函数。



##### 读取状态值

```
<p> You clicked {count} times </p>
```



##### 更新状态

```
  <button onClick={() => setCount(count + 1)}>
    Click me
  </button>
```



##### 为什么可以记住之前的状态？

是react帮我们记住的。



##### 假如一个组件有多个状态值，怎么办？

useState无论调用多少次，相互之间是独立的。



##### 如何保证状态值的相互独立？

react是根据useState出现的顺序来定的





#### useEffect

我们之前都把这些副作用的函数写在生命周期函数钩子里，比如componentDidMount，componentDidUpdate和componentWillUnmount。而现在的useEffect就相当与这些声明周期函数钩子的集合体。它以一抵三。









