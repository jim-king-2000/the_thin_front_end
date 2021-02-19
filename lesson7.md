# 第七课 前端技术入门之React（二）

## 条件渲染
在很多时候，我们需要根据某个变量来决定内容是否显示。这时，我们需要使用条件渲染。这类似于计算机语言中的`if`。但是，在React的花括号中只能放表达式，而`if`是语句，因此`if`将无法放入花括号中。但是，幸好我们有`&&`。
```javascript
class Root extends React.Component {
  constructor(props) {
    super(props);
    this.state = { show: false };
    this.onClick = this.onClick.bind(this);
  }

  onClick() {
    this.setState({ show: !this.state.show });
  }

  render() {
    return (
      <div>
        <p onClick={this.onClick}>toggle</p>
        {this.state.show && <p>真正的内容</p>}
      </div>
    );
  }
}
```
当标签中出现了falsy值的时候，React什么也不显示。因此，当`this.state.show`的值为`false`的时候，整个花括号里面的内容就如同消失了一样。但是，一旦`this.state.show`的值被设置成了`true`，这时整个表达式的值是`&&`符号的右边的操作符的值，也就是`<p>真正的内容</p>`标签。
如果我们需要创建一个没有显示的组件，我们可以直接`return null`。

## 多组件渲染
我们经常会遇到要渲染一组对象的场景，比如需要渲染一组人名的复选框，或是渲染一组可点击的选项。React支持直接渲染数组。但一般这样的数组，需要经过变换处理才可以被显示。同理，for语句是不可以放到花括号中的。这时，我们就可以用`Array.map()`。
```javascript
const Root = ({ value }) => (
  <form>
    <fieldset>
      <legend>请选择学生</legend>
      {value.map((student, index) => 
        <label>
          <input
            type='checkbox'
            key={index}
            value={student} />
          {student}
        </label>
      )}
    </fieldset>
  </form>
);

const students = ['李大力', '王可爱', '赵小龙'];
ReactDOM.render(<Root value={students} />, document.getElementById('root'));
```
`Array.map()`将字符串数组映射成了复选框数组，这样就可以进行渲染了。
注意到我们为复选框增加了一个key属性。这时因为React需要追踪DOM的变化，只有变换了的DOM才会被渲染。这种方式极大地提高了渲染效率。但是，它就需要知道在被渲染的一组组件里，每一个组件还是不是原来那个组件。这就是key属性的作用。在真正的项目中，我们一般都会把存放在数据库里的id（唯一标识符）传给key。但在上面的例子中，我们仅仅简单的使用了数组的下标来作为key。
HTML的标签是没有key属性的。这也提醒着我们，它并不是HTML，尽管看起来非常相似。

## 表单
表单与其它元素有所不同。它天生就带有一些内部状态。在大多数情况下，我们会把表单的状态存放在React状态中。我们称之为“受控组件（Controlled Component）”。
```javascript
class Root extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      name: '',
      age: ''
    };
    this.onChange = this.onChange.bind(this);
  }

  onChange(e) {
    this.setState({ [e.target.name]: e.target.value });
  }

  render() {
    return (
      <form>
        <label>
          姓名：
          <input
            type='text'
            name='name'
            value={this.state.name}
            onChange={this.onChange} />
        </label>
        <label>
          年龄：
          <input
            type='number'
            name='age'
            value={this.state.age}
            onChange={this.onChange} />
        </label>
        <label>{this.state.name}, {this.state.age}岁</label>
      </form>
    );
  }
}
```
在上面的代码中，由两个文本框。它们的内容分别绑定了`this.state.name`和`this.state.age`。当我们在任意一个文本框中编辑字符串的时候，`onChange`函数会被触发。在`onChange`函数里，`e.target`就是文本框input，`e.target.name`就是被编辑的文本框的属性name的值，我们在“姓名”框中编辑，`e.target.name`就是'name'，在“年龄”框中编辑，`e.target.name`就是'age'。 而`e.target.value`就是我们输入编辑框的内容。这就意味着，我们编辑“姓名”框，就会修改`this.state.name`并且触发渲染；我们编辑“年龄”框，就会修改`this.state.age`并且也触发渲染。

## 事件处理
React的事件处理与DOM非常相似，只有三点不同。那就是：
* React事件绑定属性的命名采用驼峰式写法，DOM是小写。
* React事件需要传入一个函数，DOM传入一个字符串。
* React事件不可以返回`false`来阻止默认行为，必须明确使用`e.preventDefault()`。

因此，在React中，绑定一个Button的单击方法，我们这么写：
```javascript
<Button onClick={handler} />
```
如果我们使用class的话，由于Javascript语法的特殊性，`this.handler`只是一个普通函数，并没有绑定`this`。因此我们只能在构造函数中自行绑定，否则在`this.handler`函数中访问`this`将得到`undefined`。
```javascript
class Page extends Component {
  constructor(props) {
    super(props);
    this.onClick = this.onClick.bind(this);
  }

  onClick() {
    this.setState({ ... });
  }

  render() {
    return <Button onClick={this.onClick} />;
  }
}
```
如果懒得写bind，可以使用Javascript的新语法“public class fields syntax”，不过它目前尚处于实验性阶段。
```javascript
class Page extends Component {
  constructor(props) {
    super(props);
  }

  onClick = () => {
    this.setState({ ... });
  }

  render() {
    return <Button onClick={this.onClick} />;
  }
}
```
传递给回调函数的参数是一个叫Event的对象。它有以下标准属性：
* type: 当前Event对象的类型。
* timeStamp: Event生成的日期和时间。
* target: 触发Event的元素。

## React组件生命周期



上图是React组件的生命周期图。我们需要注意到的是`componentWillMount()`和`componentWillUnmount()`两个函数。如果我们需要在组件加载之前做一些初始化，或卸载完成后做一些清除，可以重写这两个函数。

在React中也可以使用Ajax技术。我们可以在`componentWillMount()`中发起异步API调用，然后把数据用`setState()`设置到状态里，这样就可以触发重新渲染。

## 高阶组件（HOC, High-Order Component）
高阶组件并不是一项React技术。它只是一个模式。我们可以自己写一个函数，传入一个组件，再返回修饰后的组件。这在设计模式中叫做Decorator模式。
```javascript
export default withRouter(Page);
```
这里，Page本身就是一个React组件。但是，我们使用一个叫做withRouter的HOC将它修饰了一下，然后作为新组建返回。这个withRouter的作用就是为Page注入一个叫query的props，它的值就是当前URL的query string。

写一个HOC也很容易，它就是一个函数，只不过参数和返回值都是组件而已。
```javascript
export default Page => class extends Component { ... };
```
这里，我们定义了一个箭头表达式HOC。它的参数是Page，返回值是一个类组件（匿名类）。如果想返回一个函数组件，则可以定义一个返回箭头表达式的箭头表达式。

## 结尾
React是前端划时代的飞跃。它提供了以下一些以前的前端框架做不到或者做的不好的功能。
* 使用Virtual DOM技术，且只渲染DOM更改的部分，极大地提高了前端运行效率。
* 提供自定义组件，极大地提高了前端代码复用率。
* 规定只使用“单向数据流”，解决了前端数据传递的混乱。
* 重JS。基本上不需要写HTML了，整个工程绝大部分都是JS。如果再使用CSS-in-JS，则所有的前端工程全部都是JS。如果后端再使用Node.js，那么可以实现JS同构全栈化。

我们在demo code中选择在浏览器端解析JSX，这是一种偷懒的做法。在真正的项目中，则不会这么用，会将JSX解析成标准JS并且压缩打包（打包就是把所有的js文件合成一个，这样加载起来比较快）。然而从零开始创建一个真正的React项目十分繁琐。幸好，我们有Next.js。

## 参考资料
1. React官方文档： https://react.docschina.org/docs/hello-world.html
