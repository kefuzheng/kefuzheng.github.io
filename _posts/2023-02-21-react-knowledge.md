---
title: React 知识点
key: 2023-02-21
tags: react
---

### 1. 阻止默认行为和事件冒泡
##### 1. preventDefault
preventDefault它是事件对象(Event)的一个方法，作用是取消一个目标元素的默认行为。既然是说默认行为，当然是元素必须有默认行为才能被取消，如果元素本身就没有默认行为，调用当然就无效了。当Event对象的cancelable为false时，表示没有默认行为，这时即使有默认行为，调用 preventDefault也是不会起作用的。
```js
<a href="http://www.cnf2e.com/" id="testA" >cnf2e</a>

var a = document.getElementById("testA");
a.onclick =function(event){
   //阻止默认行为
   event.preventDefault();
}
```

##### 2. stopPropagation
stopPropagation也是事件对象(Event)的一个方法，作用是阻止目标元素的冒泡事件，但是会不阻止默认行为。什么是冒泡事件？如 在一个按钮是绑定一个”click”事件，那么”click”事件会依次在它的父级元素中被触发 。stopPropagation就是阻止目标元素的事件冒泡到父级元素
```js
<div id='div'  onclick='alert("div");'>

   <ul  onclick='alert("ul");'>

      <li onclick='alert("li");'>test</li>

   </ul>

</div>
// 上面的代码，我们单击test时，会依次触发alert(“li”),alert(“ul”),alert(“div”)，这就是事件冒泡。

<div id='div'  onclick='alert("div");' >

   <ul  onclick='alert("ul");'>

      <li id='ul-a' onclick='alert("li");event.stopPropagation()'>a</li>

   </ul>

</div>
// 这时单击”test”，只会提示alert(‘li’);
```

### 2. 创建组件
##### 1. 类组件
```typescript
import React from 'react';

class MyComponent extends React.Component {
  render() {
    return <h1>Hello {this.props.name}!</h1>;
  }
}

function App() {
  return (
    <div>
      <MyComponent name="John" />
      <MyComponent name="Mike" />
    </div>
  );
}
```

##### 2. 函数组件
```typescript
function MyComponent(props) {
  return <h1>Hello {props.name}!</h1>;
}

function App() {
  return (
    <div>
      <MyComponent name="John" />
      <MyComponent name="Mike" />
    </div>
  );
}
```

### 3. 生命周期
在谈 React 生命周期时，主要谈的是 类式组件的生命周期，因为函数式组件是没有生命周期函数的。   
组件从 被创建 到 被销毁 的过程称为组件的生命周期。   

##### 1. 生命周期状态与阶段
状态：   
- Mounting（挂载时）
- Updating（更新时）
- Unmounting（卸载时）

阶段：   
- Render： 用于计算当前的状态/更新信息，会根据产生的任务的优先级，安排任务的调度（schedule）
- Pre-commit： commit 之前，可以获取当前 DOM 的快照（snap）
- Commit： 把所有更新都 commit 到 DOM 树上

![Life period](/assets/images/react_life_period.png)   

##### 2. 组件挂载时
当组件实例被创建并插入 DOM 中时，其生命周期调用顺序如下：   
- constructor()
- static getDerivedStateFromProps()
- render()： class 组件中唯一必须实现的方法
- componentDidMount()

##### 3. 组件更新时
当组件的 props 或 state 发生变化时会触发更新。组件更新的生命周期调用顺序如下：   
- static getDerivedStateFromProps()
- shouldComponentUpdate()
- render()
- getSnapshotBeforeUpdate()
- componentDidUpdate()

##### 4. 组件卸载时
当组件从 DOM 中移除时会调用方法：componentWillUnmount()

### 4. set width in percentage in react-table
获得widget的宽度，通过计算的方式设置width   
```typescript
const getColumns = () => {
    return [
      {
        Header: 'Name',
        accessor: 'name',
        id: 'name',
        width: Math.round((window.innerWidth - 55) * 0.2),
        disableFilters: true,
      },
      {
        Header: 'Age',
        accessor: 'age',
        id: 'age',
        width: Math.round((window.innerWidth - 55) * 0.2),
        disableFilters: true,
      },
      {
        Header: 'Visits',
        accessor: 'visits',
        id: 'visits',
        width: Math.round((window.innerWidth - 55) * 0.2),
        disableFilters: true,
      },
      {
        Header: 'Last Visit',
        accessor: 'last_visit',
        id: 'last_visit',
        width: Math.round((window.innerWidth - 55) * 0.2),
        disableFilters: true,
      },
      {
        Header: 'Status',
        accessor: 'status',
        id: 'status',
        width: Math.round((window.innerWidth - 55) * 0.2),
        disableFilters: false,
      },
    ];
  };
```

----

[谈谈 React V16.4 的生命周期](https://juejin.cn/post/7203239282210324537)  
[深入理解Java双冒号(::)运算符的使用](https://blog.csdn.net/zhoufanyang_china/article/details/87798829)  
[Java Lambda 表达式](https://www.runoob.com/java/java8-lambda-expressions.html)  
[Java 8 Optional 类](https://www.runoob.com/java/java8-optional-class.html)  
