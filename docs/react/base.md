## 1.与 ES5 相比，React 的 ES6 语法有何不同？

1. require 与 import

```javascript
// ES5
var React = require('react');
 
// ES6
import React from 'react';
```

2. export 与 exports

```javascript
// ES5
module.exports = Component;
 
// ES6
export default Component;
```

3. component 和 function

```javascript
// ES5
var MyComponent = React.createClass({
    render: function() {
        return
            <h3>Hello Edureka!</h3>;
    }
});
 
// ES6
class MyComponent extends React.Component {
    render() {
        return
            <h3>Hello Edureka!</h3>;
    }
}
```

4. props

```javascript
// ES5
var App = React.createClass({
    propTypes: { name: React.PropTypes.string },
    render: function() {
        return
            <h3>Hello, {this.props.name}!</h3>;
    }
});

// ES6
class App extends React.Component {
    render() {
        return
            <h3>Hello, {this.props.name}!</h3>;
    }
}
```

5. state

```javascript
// ES5
var App = React.createClass({
    getInitialState: function() {
        return { name: 'world' };
    },
    render: function() {
        return
            <h3>Hello, {this.state.name}!</h3>;
    }
});

// ES6
class App extends React.Component {
    constructor() {
        super();
        this.state = { name: 'world' };
    }
    render() {
        return
            <h3>Hello, {this.state.name}!</h3>;
    }
}
```

6. export 与 exports

```javascript
// ES5
module.exports = Component;
 
// ES6
export default Component;
```

## 2.如何更新组件的状态？

```js
class MyComponent extends React.Component {
    constructor() {
        super();
        this.state = {
            name: 'Maxx',
            id: '101'
        }
    }
    render()
        {
            setTimeout(()=>{this.setState({name:'Jaeha', id:'222'})},2000)
            return (              
                <div>
                    <h1>Hello {this.state.name}</h1>
                    <h2>Your Id is {this.state.id}</h2>
                </div>
            );
        }
    }
ReactDOM.render(
    <MyComponent/>, document.getElementById('content')
);
```

## 3.你对受控组件和非受控组件了解多少？

|  受控组件   | 非受控组件  |
|  ----  | ----  |
| 没有维持自己的状态  | 保持着自己的状态 |
| 数据由父组件控制  | 数据由 DOM 控制 |
| 通过 props 获取当前值，然后通过回调通知更改  | Refs 用于获取其当前值 |

## 4.什么是高阶组件（HOC）？

> 高阶组件是重用组件逻辑的高级方法，是一种源于 React 的组件模式。 HOC 是自定义组件，在它之内包含另一个组件。它们可以接受子组件提供的任何动态，但不会修改或复制其输入组件中的任何行为。你可以认为 HOC 是“纯（Pure）”组件。

## 5.你能用HOC做什么？

* 代码重用，逻辑和引导抽象

* 渲染劫持

* 状态抽象和控制

* Props 控制

## 6. 什么是纯组件？

> 纯（Pure） 组件是可以编写的最简单、最快的组件。它们可以替换任何只有 render() 的组件。这些组件增强了代码的简单性和应用的性能。

## 7.React 中 key 的重要性是什么？

> key 用于识别唯一的 Virtual DOM 元素及其驱动 UI 的相应数据。它们通过回收 DOM 中当前所有的元素来帮助 React 优化渲染。这些 key 必须是唯一的数字或字符串，React 只是重新排序元素而不是重新渲染它们。这可以提高应用程序的性能。

## 8.什么是 React Hooks？使用 React Hooks 好处是啥？

> Hooks是 React 16.8 中的新添加内容。它们允许在不编写类的情况下使用state和其他 React 特性。使用 Hooks，可以从组件中提取有状态逻辑，这样就可以独立地测试和重用它。Hooks 允许咱们在不改变组件层次结构的情况下重用有状态逻辑，这样在许多组件之间或与社区共享 Hooks 变得很容易。

#### 好处

**首先，Hooks 通常支持提取和重用跨多个组件通用的有状态逻辑，而无需承担高阶组件或渲染 props 的负担。Hooks 可以轻松地操作函数组件的状态，而不需要将它们转换为类组件。
Hooks 在类中不起作用，通过使用它们，咱们可以完全避免使用生命周期方法，例如 componentDidMount、componentDidUpdate、componentWillUnmount。
相反，使用像useEffect这样的内置钩子。**

Hooks 写法示例：

```javascript
import React, { useState } from "react";
import { Button} from 'antd'

const modifyText = (text) => {
    if(!text) throw Error('the params must request');
    return text + 1;
}

//ChangeText现在看起来更像一个纯函数了吧！
const ChangeText = ({text, changeText}) => {
    // 开发者编写的组件代码，只不过剔除了useState声明
    return(
        <div>
            <h1>{text}</h1>
            <Button type="primary" onClick={() => changeText(modifyText(text))}>Hooks</Button>
        </div>
    )
}

//React内核生成的“虚拟组件”，帮你维护组件状态
const PageRender = () => {
    const [text, changeText] = useState('React hooks is testing!!!');
    return (
        <ChangeText text={text} changeText={changeText}/>
    )
};

export default PageRender
```

## 9.

## 10.

## 11.

## 12.