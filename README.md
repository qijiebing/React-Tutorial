# React教程

## 目录
  1. [介绍](#介绍)
  2. [Hello World](#hello-world)
  3. [学习JSX](#学习jsx)

## 介绍
本文译自[React官方文档](https://facebook.github.io/react/docs/hello-world.html)

![React](https://github.com/alivebao/React-Tutorial/raw/master/img/logo.jpg)

## hello-world

### Hello World
开始学习React最简单的方式是参看CodePen上的[Hello World实例](http://codepen.io/gaearon/pen/ZpvBNJ?editors=0010)。
您不需要安装任何东西，只要在浏览器中打开该链接并随我们学习该例子。如果想要使用本地开发环境，查看[安装页面](https://facebook.github.io/react/docs/installation.html)。
最简单的React实例如下：
```javascript
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```
这段代码在页面上渲染了一个Hello World的header。
接下来将逐步向您介绍如何使用React。我们将了解React程序的构建块-元素和组件。
掌握他们后，您将可以通过多块小段的可复用代码创建出复杂的应用。

### 预备知识

React是一个JavaScript库，您需要明白JavaScript的基本语法。
在例子中，我们也会使用一些ES6的语言特性。由于其相对较新，我们会保守的使用它。
但您最好熟悉[箭头函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，[类](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes)，[模板字符串](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/template_strings)，```let```和```const```声明。
您可以通过[Babel REPL](http://babeljs.io/repl/)查看ES6的编译结果。

## 学习jsx

首先看看以下代码段：
```javascript
const element = <h1>Hello, world!</h1>;
```
这个有趣的标签表达式既不是一个字符串，也不是一个HTML标签。
它叫做JSX，是JavaScript的扩展。我们推荐在React中使用JSX描述UI。
JSX也许会使你想起某种模板语言，它可以充分利用JavaScript的特性。
JSX创建React元素。我们将再下一章节学习如何将其渲染至DOM。接下来，我们将学习JSX的基本用法。

### 在JSX中嵌入表达式
在JSX中，您可以通过 *{}* 的方式嵌入任何JavaScript表达式。
如 *2+2*, *user.firstName*, 和 *formatName(user)* 等均为有效的表达式：
```javascript
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```
[在CodePen中尝试](http://codepen.io/gaearon/pen/PGEjdG?editors=0010)

从可读性的角度考虑，我们将JSX分成了多行。但这并不是强制性的。
当需要分行时，我们推荐使用圆括号 *()* 将其括起，从而避免JS的[自动插入机制ASI](http://stackoverflow.com/questions/2846283/what-are-the-rules-for-javascripts-automatic-semicolon-insertion-asi)

### JSX也是表达式
通过编译后，JSX表达式变成了常规的JavaScript对象。
这意味着您可以使用JSX中使用 *if* 语句和 *for* 循环等:
```javascript
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

### 使用JSX指定标签属性
您可以使用引号 *""* 将字符串指定为属性：
```javascript
const element = <div tabIndex="0"></div>;
```
也可以使用括号 *{}* 嵌入JavaScript表达式作为属性：
```javascript
const element = <img src={user.avatarUrl}></img>;
```
但是，__当通过JavaScript表达式未属性赋值时，不要使用双引号。__
否则，JSX会将其 __视为一个字符串而非JavaScript表达式__。
对于字符串，我们可以直接使用 *""* 将其作为属性值；对于表达式，我们可以通过 *{}* 将其作为属性值。
但是 __不要将其混用__ 。

### 使用JSX指定子元素
当某tag不含值时，可以直接用 */>* 将其关闭：
```javascript
const element = <img src={user.avatarUrl} />;
```
JSX的标记可以包含子元素：
```javascript
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```
__注意，由于相对HTML而言，JSX更加类似于JavaScript, React DOM使用驼峰命名代替HTML中的属性名。__
__例如，JSX中使用 *className* 而非 *class* , 使用 *tabIndex* 而非 *tabindex* __

### JSX能够防注入攻击
在JSX中直接嵌入用户输入是安全的：
```javascript
const title = response.potentiallyMaliciousInput;
// This is safe:
const element = <h1>{title}</h1>;
```
在执行渲染前，React DOM会默认对任何嵌入的值进行编码。
这可以避免应用被注入，可以避免XSS攻击。

### JSX表示对象
Babel将JSX编译为 *React.createElement()* 调用。
以下两种写法是一致的：
```javascript
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```
```javascript
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```
*React.createElement()* 会执行一些语法方面的检查，但其核心功能是创建一个如下的对象：
```javascript
const element = {
  ...
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world'
  }
  ...
};
```
这些对象被称为"React元素"。我们可以将其视为我们想要在屏幕上表现的元素的描述。React读取这些对象并利用他们构建DOM并保持更新。
我们将在下一部分学习将React元素渲染成DOM。

Tip：
__为了ES6和JSX都能在编辑器中高亮显示，我们推荐您将您的编辑器设置为"Babel"语法方案。__