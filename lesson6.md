# 第六课 前端技术入门之React（一）

## React简介
React时Facebook开源的前端框架。React是业界的一次颠覆性的技术革新。它使用了Virtual DOM，使得前端运行效率大大提高。它还能够实现前端组件化，极大的提高了前端代码的复用水平。React再配合CSS-in-JS，使得实现Web App不再需要编写/维护HTML文件和CSS文件，一切尽在JS中。

## 最简单的React程序
一个最简单的React程序，只有寥寥几行。
```javascript
const Root = () => <h1>Hello, world!</h1>;
ReactDOM.render(<Root />, document.getElementById('root'));
```
程序中的Root，就是大名鼎鼎的自定义组件。在程序中，可以把自定义组件当标签使用。自定义组件就是前端组件化的基础。写好了一个自定义组件，就可以在别的React工程里使用它。程序中的第二行，就是把Root组件渲染到一个id为root的元素（一般是div）上。
我们发现使用自定义组件的语法，`<Root />`，特别像HTML。但其实这并不是HTML，它叫做JSX。它是Javascript的语法扩展，它将会被转译成真正的Javascript代码，从而被浏览器执行。

## 架设React服务器
1. 编写index.html。
```html
<!DOCTYPE HTML>
<html>
<head>
  <script src='https://cdn.bootcss.com/react/16.7.0/umd/react.production.min.js'></script>
  <script src='https://cdn.bootcss.com/react-dom/16.7.0-alpha.2/umd/react-dom.production.min.js'></script>
  <script src='https://cdn.bootcss.com/babel-standalone/6.26.0/babel.min.js'></script>
  <script type='text/babel' src='/static/index.js'></script>
</head>
<body>
  <div id='root'></div>
</body>
</html>
```
2. 在static子目录下创建index.js，内容如下：
```javascript
const Root = () => <h1>Hello, world!</h1>;
ReactDOM.render(<Root />, document.getElementById('root'));
```
3. 运行服务器：npm run dev
4. 访问 http://localhost:3000/static/index.html
5. 按F5刷新页面，观察页面是否有闪烁。

React项目需要react和react-dom两个库。我们目前是用CDN（Content Delivery Network，内容分发网络）的方式引入这两个库的。可以把CDN理解成一个分布在世界各地的缓存，当你去访问资源的时候，CDN会自动将请求转发到离你最近的那一个缓存去。
我们还加载了一个叫做babel-standalone的库，目前我们用它在浏览器中解析React。在工程中我们不会这么用，因为这影响运行效率。在工程中我们会使用Node.js + Next.js，将React变成普通的Javascript后再让浏览器加载。

## React元素
几乎所有的HTML标签，都可以在JSX中使用。在JSX中，它们有了新的名字，叫做React Element。
以下都是React元素。
```javascript
const sentence = <p>This is React.</p>;
const sidebar = (
  <div>
    <h1>Title</h1>
    <p>Content</p>
  </div>);
```
在多行React元素的外面，我们可以使用小括号将其包住。

我们可以在某一个div标签上直接渲染元素。这里的`ReactDOM.render()`是在浏览器端运行的，因此它属于客户端渲染。当我们使用F5来reload页面的时候，我们会发现明显的闪烁。这是因为初始的HTML是个空div，渲染出来就是空白。只有当浏览器加载并执行完js，div里才会有内容，这时再次渲染，我们才能看见真正的内容。第一次渲染为空白就是造成闪烁的根本原因。
```javascript
ReactDOM.render(sidebar, document.getElementById('root'));
```
我们需要注意的是，这虽然很像HTML，但它不是HTML，它是Javascript。经过渲染之后才会变成真正的HTML。

## React组件
React允许自定义标签。这种自定义的标签，我们叫它React Component。React组件有两种表现形式。第一种，函数表示。
```javascript
function Root() {
  return <p>React Component</p>;
}
```

函数表示也可以写成箭头表达式
```javascript
const Root = () => <p>React Component</p>;
```
第二种，类表示。
```javascript
class Root extends React.Component {
  constructor(props) {
    super(props);
  }

  render() {
    return <p>React Component</p>;
  }
}
```
以上三种写法都是等价的。无论哪种形式，都必须且只能返回一个React根元素（子元素数量不限）。区别是一个在函数中直接返回，另一个在类的render方法中返回。

React元素可以由React组件形成。
```javascript
const element = <Root />;
```
定义好React组件之后，把组件名放到标签里，就形成了React元素，然后就可以进行渲染了。React元素和组件就这样嵌套着存在，在组件里也可以使用别的组件形成的元素。
React组件的名称必须以大写字母开头。因为React默认会把小写字母开头的标签转换为HTML的标签，而只有大写字母开头的标签，才会被转换成一个类。

注意：
* React组件只能返回一个React根元素。
* React组件名称必须以大写字母开头。

## React属性
与HTML类似，我们可以在标签中定义属性。
```javascript
const e0 = <a href='url' target='blank' />;
const e1 = <Root value={2} />;
```
与HTML不同的是，如果属性的值不是字符串，则必须用花括号包住。在JSZ中，花括号的意思是引入一段Javascript表达式。因此，可以在花括号中调函数，函数的返回值会被赋值给属性。

我们可以在React组件中处理外部传入的属性。在函数组件中，函数的参数就是属性。
```javascript
const Root = props => (
  <div>
    <p>React Component</p>
    <p>Value: {props.value}</p>
  </div>
);
```
我们可以使用destructing语法直接拿到props里面的值。
```javascript
const Root = ({ value }) => (
  <div>
    <p>React Component</p>
    <p>Value: {value}</p>
  </div>
);
```
类组件则必须使用`this.props`来获取属性的值。
```javascript
class Root extends React.Component {
  constructor(props) {
    super(props);
  }

  render() {
    return (
      <div>
        <p>React Component</p>
        <p>Value: {this.props.value}</p>
      </div>
    );
  }
}
```
注意：
* 属性值是只读的，是不能被改变的。
* JSX的属性名是以小驼峰命名法命名的。因此，原有的2单词的属性，如tabindex, rowspan, colspan等，一律要改为tabIndex, rowSpan和colSpan。
* 在HTML里我们可以为标签添加class属性，但在JSX里，必须写成className。
* 属性名children表示该组件的子组件。

## React状态
React的属性是只读的。而且拥有属性的组件只会被渲染一次。然而在现实中，我们常常遇到的情况是，刚开始的时候，渲染出某个初始状态，过了一会儿，由于用户的操作或者事件的发生，导致状态改变，需要重新渲染。这时，我们需要使用React State。只有类组件才可以使用React State。
```javascript
class Root extends React.Component {
  constructor(props) {
    super(props);
    this.state = { clicked: 0 };
    this.onClick = this.onClick.bind(this);
  }

  onClick() {
    this.setState({ clicked: ++this.state.clicked });
  }

  render() {
    return (
      <p onClick={this.onClick}>Clicked: {this.state.clicked}</p>
    );
  }
}
```
我们在构造函数里直接为this.state赋值，直接赋值是不会出发重新渲染的，只有在构造函数内才能这样使用。在渲染的时候，我们使用this.state.clicked把状态的值读出来。然后我们给`<p>`的onClick属性设置了一个回调函数，每次我们去点击文字的时候，该回调会被触发。我们在回调函数中使用this.setState()改变了组件的状态，这会触发重新渲染。于是，更新后的状态会被渲染到`<p>`上。
由于Javascript语言的特殊性，this.onClick本身是一个普通函数，如果将它直接传给`<p>`的onClick属性，那么在该函数里访问this会得到undefined。为了解决这个问题，我们只能在构造函数里为this.onClick绑上this。

React的状态更新是合并的。如果当前状态有两个子对象，如`{ a: 'a', b: 'b' }`，当调用`this.setState({ a: 'c' })`后，状态会变成`{ a: 'c', b: 'b' }`。未被更改的状态会得以保留。因此我们在程序中，只更新变化的状态即可。

在React项目中，父组件的状态和属性可以传给子对象的属性，子对象的状态和属性又可以传给孙对象的属性。数据只能从父组件向子组件流动。我们称之为“单向数据流”。

需要注意的是，state的值是不可以被修改的。因此，在state里面使用数组需要特别注意。由于`Array.push()`和`Array.pop()`等函数会修改数组的内容，因此在state里面是不可以使用的。
```javascript
this.state.students.push('Bob');
// 这样写是错误的，它不仅不会触发渲染，还会破坏内部状态。
```
因此，我们必须要生成新的数组。
```javascript
this.setState({ students: [...this.state.students, 'Bob'] });
```
上面的代码生成了一个新的数组，这个数组前面的元素就是this.state.students，然后最后一个元素是'Bob'。

## Fragments
React组件只能返回一个根元素。如果有多个元素要返回，我们一般会使用`<div>`包裹一下。但使用`<div>`会增加一个DOM节点。有时候，这个增加的节点是会有问题的。比如，`<ul>`里天生就可以有多个`<li>`，如果我们返回多个`<li>`的时候，用`<div>`包裹一下，就破坏整个语义了。
这时，React Fragments粉墨登场。
```javascript
return (
  <>
    <li>Coffee</li>
    <li>Tea</li>
    <li>Milk</li>
  </>
);
```
空标签`<></>`就是React Fragments。这样，我们的自定义标签就可以放到`<ul>`标签里面了。
