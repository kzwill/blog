## react.js学习笔记

### 1、安装环境和创建项目

01）学习react前，需要安装node.js  下载稳定版安装

安装成功后，在命令行输入

node -v  和 npm -v 都会有版本信息，则说明安装成功。

![1565159912666](C:\Users\kangzw\AppData\Roaming\Typora\typora-user-images\1565159912666.png)

02）运行该命令。创建react项目

```
npx create-react-app my-app
```

**注意**：在第一次运行  npx create-react-app my-app 的时候，一般会报错，在执行create-react-app的时候，如果没有科学上网，一般情况下都不会太快，因为这个时候create-react-app指令默认调用npm，所以我们需要把npm的register永久给设置成cnpm，这样就可以一劳永逸了。

```
npm config set registry https://registry.npm.taobao.org
-- 配置后可通过下面方式来验证是否成功
npm config get registry
-- 或npm info express
```

创建好项目之后，运行

```
cd my-app
npm start
```

### 2、项目结构说明

##### 第一部分：项目入口：index.js

```javascript
//引入react的目的是为了识别特定的语法结构
import React from 'react';
//引入ReactDOM是为了将组件渲染到dom节点上
import ReactDOM from 'react-dom';
//引入App组件 大写字母开头都是组件，引入reacr后，才可以识别
import App from './App';
//将组件渲染至dom节点上
ReactDOM.render(<App />, document.getElementById('root'));
```

##### 第二部分：组件：App.js

```javascript
import React from 'react';
//定义一个react组件App
function App() {  //版本不同，此处显示不同，以当前版本形成的组件格式为准
  return (
    <div className="App">
        hello world！
    </div>
  );
}  
//将上面的组件导出，便于后面调用
export default App;
```

##### 第三部分：页面index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
    <title>React App</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>
```

至此，项目既可以按照自己的想法进行初步编辑了。

### 3、简单的jsx语法

在Appjs中存在jsx语法，例如：

```jsx
 return (
    <div className="App">
        hello world！
    </div>
  );
```

**注意：jsx要求 return 的内容必须要在一个标签内部，该标签内部可以有多个标签，但是jsx不能返回多个标签。**

​        正常情况下，此语法结构是错误的，div的标签不能直接用return返回，但是jsx支持这种语法格式，以及这种标签结构的语法格式。

```jsx
ReactDOM.render(<App />, document.getElementById('root'));
```

并且在div标签内部也可以加入**js表达式**（不是语句），格式如下

```
<div className="App">
    	{2 + 6}
        hello world！
</div>
```

### 4、将函数转换为类

你可以通过5个步骤将函数组件 `Clock` 转换为类

1. 创建一个名称扩展为 `React.Component` 的[ES6 类](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)
2. 创建一个叫做`render()`的空方法
3. 将函数体移动到 `render()` 方法中
4. 在 `render()` 方法中，使用 `this.props` 替换 `props`
5. 删除剩余的空函数声明

**函数：**

```jsx
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}
```

**类：**

```jsx
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

### 5、正确地使用状态

关于 `setState()` 这里有三件事情需要知道

- #### 不要直接更新状态

  <u>构造函数是唯一能够初始化 `this.state` 的地方</u>

  ```jsx
  // Wrong
  this.state.comment = 'Hello';
  // Correct
  this.setState({comment: 'Hello'});
  ```

- #### 状态更新可能是异步的

  React 可以将多个`setState()` 调用合并成一个调用来提高性能。

  因为 `this.props` 和 `this.state` 可能是异步更新的，你不应该依靠它们的值来计算下一个状态。

  ```jsx
  // Wrong
  this.setState({
    counter: this.state.counter + this.props.increment,
  });
  // Correct
  this.setState((prevState, props) => ({
    counter: prevState.counter + props.increment
  }));
  // Correct
  this.setState(function(prevState, props) {
    return {
      counter: prevState.counter + props.increment
    };
  });
  ```

  **注意：箭头函数是ES6的一种语法**，其基本的语法为：

  ```js
  (参数1, 参数2, …, 参数N) => { 函数声明 }
  //相当于：(参数1, 参数2, …, 参数N) =>{ return 表达式; }
  (参数1, 参数2, …, 参数N) => 表达式（单一）
  
  // 当只有一个参数时，圆括号是可选的：
  (单一参数) => {函数声明}
  单一参数 => {函数声明}
  
  // 没有参数的函数应该写成一对圆括号。
  () => {函数声明}
  ```

- #### 状态更新合并

当你调用 `setState()` 时，React 将你提供的对象合并到当前状态。

例如，你的状态可能包含一些独立的变量：

```jsx
  constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```

你可以调用 `setState()` 独立地更新它们：

```jsx
  componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```

这里的合并是浅合并，也就是说`this.setState({comments})`完整保留了`this.state.posts`，但完全替换了`this.state.comments`。





































































































































react 是一个组件，页面上的每一部分，每一个组件就是网页上的一部分（一个标签）