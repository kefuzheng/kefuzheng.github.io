---
title: React Hooks
key: 2023-02-21
tags: react
---

### 1. Hook
钩子（hook）就是 React 函数组件的副效应解决方案，用来为函数组件引入副效应

##### 1. Hook使用原则
只能在函数最外层调用 Hook。不要在循环、条件判断或者子函数中调用。   
只能在 React 的函数组件中调用 Hook。不要在其他Typescript 函数中调用。

### 2. useState
useState()是改变状态的开关，将状态添加到函数组件需要4个步骤:启用状态、初始化、读取和更新。   
每当 React 重新渲染组件时，都会执行useState(initialState)   
useState(initialState)的第一个参数是初始状态。返回的数组有两项:当前状态和状态更新函数   
使用 setState(newState)来更新状态值。  
如果需要根据先前的状态更新状态，可以使用回调函数setState(prevState => newState)。   

```typescript
import React, { useState } from 'react';

function Bulbs() {
  const [on, setOn] = useState(false);

  const lightSwitch = () => setOn(on => !on);

  return (
    <>
      <div className={on ? 'bulb-on' : 'bulb-off'} />
      <button onClick={lightSwitch}>开/关</button>
    </>
  );
}
```
##### 1. 工作原理
1. 当运行函数组件时，会生成一个组件节点，在组件节点生成的过程中，会创建真实的DOM节点，和一个状态链表
2. 第一次运行函数组件时（调用App函数），发现第一次使用useState
3. 检测状态链表中是否含有下标为0，状态值为默认值false的这一项
4. 如果没有就新建一个这样的状态放进状态表格中，如果有就直接获取，
5. 第二次发现使用useState就重复第 3、 4步
6. 当该函数组件重新渲染时，会再次寻找useState，根据该出现的次数，对应状态表格中的数据进行查找

##### 2. 使用细节
- useState 尽量写在函数组件的函数体开始部分，便于阅读
- useState 不要写在循环或者判断语句里（原因参考基本原理中的根据出现次数找下标对应状态值）
- useState 返回的第二个值（setXXX）引用是不变的，目的是为了节约内存空间
- 使用useState()来管理一个简单的状态
- 使用第二个返回值函数（setXXX）改变数据，如果新的数据和旧的数据是完全一样的（Objec.is比较），那么组件不会重新渲染
- 使用第二个返回值函数改变数据，新值会直接替换掉旧值，而不会合并
- 实现强制刷新组件：类组件：this.forceUpdate()；函数组件：给setN函数传入一个空对象
- 如果某些组件之间没有必然的联系，那么尽量写成多个状态的形式，不要合并成一个对象
- 和类组件的改变状态一样，函数组件改变状态时也有可能是异步的（dom事件中改变状态），那么此时就会将多次改变状态合并在一起最终变化，提高效率。此时是不能信任新的值，而是应该使用回调函数的形式{setState(prevState => newState)}

##### 3. 状态的延迟初始化
每当 React 重新渲染组件时，都会执行useState(initialState)。 如果初始状态是原始值（数字，布尔值等），则不会有性能问题。   
当初始状态需要昂贵的性能方面的操作时，可以通过为useState(computeInitialState)提供一个函数来使用状态的延迟初始化
```typescript
import React, { useState } from 'react';

function MyComponent({ bigJsonData }) {
  const [value, setValue] = useState(function getInitialState() {
    const object = JSON.parse(bigJsonData); 
    return object.initialValue;
  });
}
```
getInitialState()仅在初始渲染时执行一次，以获得初始状态。在以后的组件渲染中，不会再调用getInitialState()，从而跳过昂贵的操作

##### 4. 复杂状态管理useReducer
useReducer 是 React 中的一个 Hook。    
reducer 是一个函数，它接收两个参数：当前的 state 和 action，返回一个新的 state。   
useReducer 返回一个数组，包含了当前的 state 和 dispatch 函数。dispatch 函数用于触发 action，从而更新 state
```typescript
import React, { useReducer } from 'react';

const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </>
  );
}
```

### 3. useMemo
useMemo 是 React 中的一个 Hook，它可以用来缓存计算结果，避免重复计算。useMemo 接收两个参数，第一个参数是一个函数，这个函数返回需要缓存的值；第二个参数是一个数组，这个数组中包含了所有需要监视的变量。只有当这些变量发生变化时，useMemo 才会重新计算缓存的值。   
如果第二个参数是一个空数组，就表明副效应参数没有任何依赖项。因此，副效应函数这时只会在组件加载进入 DOM 后执行一次，后面组件重新渲染，就不会再次执行。这很合理，由于副效应不依赖任何变量，所以那些变量无论怎么变，副效应函数的执行结果都不会改变，所以运行一次就够了。   
useMemo则可以看作是componentWillUpdate函数   
```typescript
import React, { useMemo } from 'react';

function MyComponent({ data }) {
  const result = useMemo(() => {
    // 计算结果
    return data.map(item => item + 1);
  }, [data]);

  return (
    <div>
      {result.map(item => (
        <div>{item}</div>
      ))}
    </div>
  );
}
```

### 4. useCallback
useCallback 是 React 中的一个 Hook，它可以用来缓存函数，避免重复创建。useCallback 接收两个参数，第一个参数是一个函数，这个函数是需要缓存的函数；第二个参数是一个数组，这个数组中包含了所有需要监视的变量。只有当这些变量发生变化时，useCallback 才会重新创建函数
```typescript
import React, { useCallback } from 'react';

function MyComponent({ onClick }) {
  const handleClick = useCallback(() => {
    onClick();
  }, [onClick]);

  return (
    <button onClick={handleClick}>Click me</button>
  );
}
```

##### 1. 与useMemo的区别
它们的作用不同。useMemo 用于缓存计算结果，避免重复计算；而 useEffect 用于处理副作用，比如访问 DOM、发起网络请求等等。

##### 2. 使用场景
useMemo 的使用场景：需要缓存计算结果，避免重复计算。   
useEffect 的使用场景：需要处理副作用，比如访问 DOM、发起网络请求等等。

### 5. useEffect
useEffect可以让你在函数组件中使用生命周期函数。useEffect 会在每次渲染后执行，包括第一次渲染。你可以使用 useEffect 来处理一些副作用，比如：请求数据、添加事件监听、手动修改 DOM 等等

```typescript
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // 相当于componentDidUpdate:
  useEffect(() => {
    // 使用浏览器的 API 更新页面标题
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
useEffect()的参数是一个函数，它就是所要完成的副效应（改变网页标题）。组件加载以后，React 就会执行这个函数。  
useEffect()的作用就是指定一个副效应函数，组件每渲染一次，该函数就自动执行一次。组件首次在网页 DOM 加载后，副效应函数也会执行。

##### 1. 第二个参数
有时候，我们不希望useEffect()每次渲染都执行，这时可以使用它的第二个参数，使用一个数组指定副效应函数的依赖项，只有依赖项发生变化，才会重新渲染   
useEffect()的第二个参数是一个数组，指定了第一个参数（副效应函数）的依赖项（props.name）。只有该变量发生变化时，副效应函数才会执行。   
如果第二个参数是一个空数组，就表明副效应参数没有任何依赖项。因此，副效应函数这时只会在组件加载进入 DOM 后执行一次，后面组件重新渲染，就不会再次执行。这很合理，由于副效应不依赖任何变量，所以那些变量无论怎么变，副效应函数的执行结果都不会改变，所以运行一次就够了。

##### 2. useEffect和生命周期
在 React 的函数组件中，useEffect的作用其实也对标了类组件中的生命周期，它的四种使用格式也与生命周期的四种钩子函数形成着对应关系：   
- componentDidMount 对应 useEffect(() => {}, [])
- componentDidUpdate 对应 useEffect(() => {})
- componentWillUnmount 对应 useEffect(() => { return () => {} })
- shouldComponentUpdate 对应 useEffect(() => {}, [props])

##### 3. 使用细节
- 副作用函数的运行节点：在页面完成真实的UI渲染之后, 因此它的执行是异步的，并且不会阻塞浏览器
- 每个函数组件中，可以有多个useEffect函数

##### 4. useEffect的返回值
副效应是随着组件加载而发生的，那么组件卸载时，可能需要清理这些副效应。   
useEffect()允许返回一个函数，在组件卸载时，执行该函数，清理副效应。如果不需要清理副效应，useEffect()就不用返回任何值。
```typescript
useEffect(() => {
  const subscription = props.source.subscribe();
  return () => {
    subscription.unsubscribe();
  };
}, [props.source]);
```
上面例子中，useEffect()在组件加载时订阅了一个事件，并且返回一个清理函数，在组件卸载时取消订阅。   
实际使用中，由于副效应函数默认是每次渲染都会执行，所以清理函数不仅会在组件卸载时执行一次，每次副效应函数重新执行之前，也会执行一次，用来清理上一次渲染的副效应。

### 6. useContext
使用 useContext 可以避免在组件树中通过 props 一层层传递数据，使得组件之间的数据传递更加简单和方便。   
使用 useContext 需要先创建一个 context 对象，然后在需要使用该 context 的组件中调用 useContext 方法来获取该 context 的值   
```typescript
/* 
1.需要引入createContext,useContext 
2.通过createContext来创建句柄 
3.Context.Provider来确定共享范围 
4.通过value来分发内容 
5.在子组件中通过useContext（Context句柄）来获取数据 
*/
import React, { useContext } from 'react';

const MyContext = React.createContext();

function MyComponent() {
  const value = useContext(MyContext);
  return <div>{value}</div>;
}

function App() {
  return (
    <MyContext.Provider value={'Hello World!'}>
      <MyComponent />
    </MyContext.Provider>
  );
}
```

### 6. useRef
`const myRef = useRef(initialValue);`   
- useRef 返回一个可变的 ref 对象，且只有一个current属性，初始值为传入的参数( initialValue )；
- 返回的 ref 对象在组件的整个生命周期内保持不变；
- 当更新 current 值时并不会 re-render ，而 useState 更新值时会触发页面渲染；
- 更新 useRef 是 side effect (副作用)，所以一般写在 useEffect 或 event handler 里;
- useRef 类似于类组件的 this。

##### 1. useRef来获取上一次的值
```typescript
function usePrevious(value) {
  const ref = useRef();
  useEffect(() => {
    ref.current = value;
  });
  return ref.current;
}
```
这个函数再渲染过程中总是返回上一次的值，因为 ref.current 变化不会触发组件的重新渲染，所以需要等到下次的渲染才能显示到页面上。

##### 2. useRef来保存不需要变化的值
因为useRef的返回值在组件的每次redner之后都是同一个，所以它可以用来保存一些在组件整个生命周期都不需要变化的值

### 7. forwardRef
forwardRef的作用是将父组件传递给子组件的 ref 属性转发到子组件内部的某个 DOM 节点或子组件上.   
使用 forwardRef 可以让函数组件接收一个 ref 参数，然后将其转发给其子组件   

useImperativeHandle 是一个自定义暴露给父组件的实例值的 Hook，当使用 ref 时，可以针对一个 React 组件定义它要暴露的任何属性。   
通常和 forwardRef 一起使用，用于减少父组件获取的 DOM 元素属性，只暴露给父组件需要用到的 DOM 方法。   
使用 useImperativeHandle 解决了使用 forwardRef 获取子函数式组件 DOM 时，获取到的 DOM 属性暴露的太多了的问题。在子函数式组件中定义父组件需要进行 DOM 操作，减少获取 DOM 暴露的属性过多   
```typescript
mport React, { useRef, forwardRef, useImperativeHandle } from 'react'

const JMInput = forwardRef ((props, ref) => {

  // 作用: 减少父组件获取的DOM元素属性,只暴露给父组件需要用到的DOM方法
  // 参数1: 父组件传递的ref属性
  // 参数2: 返回一个对象,父组件通过ref.current调用对象中方法
  useImperativeHandle (ref, () => ({
    handlerData: () => {
      // handler data
    },
  }))

  return <input type="text"/>
});

export default function ImperativeHandleDemo () {
  const inputRef = useRef ()

  // useImperativeHandle 主要作用:用于减少父组件中通过forward+useRef获取子组件DOM元素暴露的属性过多
  // 为什么使用: 因为使用forward+useRef获取子函数式组件DOM时,获取到的dom属性暴露的太多了
  // 解决: 使用uesImperativeHandle解决,在子函数式组件中定义父组件需要进行DOM操作,减少获取DOM暴露的属性过多
  return (
    <div>
      <button onClick={() => (inputRef.current as any).handlerData()}>聚焦</button>
      <JMInput ref={inputRef} />
    </div>
  )
};
```

### 8. 常见问题
##### 1. 在componentDidUpdate中使用setState
componentDidUpdate 是 React 组件生命周期中的一个方法，当组件更新后立即被调用，首次渲染不会执行此方法。   
在 componentDidUpdate 中可以对 DOM 进行操作，如果你对更新前后的 props 进行了比较，也可以选择在此处进行网络请求。   
你也可以在 componentDidUpdate 中直接调用 setState()，但请注意它必须被包裹在一个条件语句中。   
```typescript
class ExampleComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  componentDidUpdate(prevProps, prevState) {
    if (prevState.count !== this.state.count) {
      console.log('count state has changed.')
      // do something
    }
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

##### 2. 如何解决：'Error: Maximum update depth exceeded'
至于无限循环错误，通常是因为我们改变了生命周期方法内的组件属性或状态(例如：componentDidMount，componentDidUpdate用于基于类的组件，useEffect钩子用于功能组件)   
请确保在组件的生命周期内没有任何地方可以直接更改道具或状态(无条件)。 例如，直接在componentDidUpdate内部使用setState将导致无限循环。

##### 3. useEffect 钩子监听不到数组变化分析
useEffect 监听的是变量在内存中的地址引用，如果变量引用没有发生变化，他是监听不到的，副作用代码并不会触发执行。  
通过新建数组来改变地址：`setList([...list])`

----

[轻松学会 React 钩子：以 useEffect() 为例](https://www.ruanyifeng.com/blog/2020/09/react-hooks-useeffect-tutorial.html)  
[React的useContext的使用](https://juejin.cn/post/6896353934525497357)  
[React Hooks系列之useRef](https://www.jianshu.com/p/c1240516c44c)  
[react中父组件调用子组件的方法](https://blog.csdn.net/qq_36990322/article/details/109858890)  
