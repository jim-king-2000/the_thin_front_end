# 第十二课 前端技术入门之MobX

## 背景介绍
通过前面的课程，我们了解到了页面状态。我们曾经使用React State来控制对话框的显示和隐藏，这就是页面状态。但对于一个复杂的页面来说，页面状态繁多，且相互关联，同一个页面状态还有可能控制多个元素。这时，仅仅使用React State就已经不够了。需要单独使用一个框架来实现。Redux曾经是非常流行的状态管理框架，但Redux使用起来相对比较繁琐。后来出现的MobX大大简化了使用的难度，因此逐渐在业界得以流行。

## 页面状态管理
页面的状态控制着页面数据的流动和元素的呈现。绝大多数的页面状态管理流程，都可以抽象成下图的模式。
```mermaid
graph TD;
  Actions --> States;
  States --> Computed Values;
  Computed Values --> Reactions;
  Reactions --> Actions;
```
Actions就是事件的回调函数，它可以由用户输入（如点击按钮，修改文本等）触发，也可以由系统事件（如定时器消息，服务器反推等）触发。在Actions里面，我们可以直接修改状态，也可以调用API，获取服务器的数据，再根据数据修改状态。

States就是一组相互正交（互相没有关联）且最小化的原始数据组合。States几乎可以是任何类型，字符串，数组，对象都可以。States的变化可以触发后续的步骤自动更新。

Computed Values是由States经过处理而得到的值。MobX会自动更新它。

Reactions与Computed Values类似，可以被States或Computed Values的变化触发。但它不会产生一个值，而是会执行一个动作，比如重新渲染UI。

## Hello MobX
1. 安装MobX库：`npm i --save @babel/plugin-proposal-class-properties @babel/plugin-proposal-decorators mobx mobx-react`
2. 修改.babelrc文件：
```json
{
  "presets": [
    "next/babel"
  ],
  "plugins": [
    ["@babel/plugin-proposal-decorators", { "legacy": true }],
    ["@babel/plugin-proposal-class-properties", { "loose": true }],
    [
      "babel-plugin-styled-components",
      {
        "ssr": true,
        "displayName": true,
        "preprocess": false
      }
    ]
  ]
}
```
3. 修改index.js文件：
```javascript
import { action, observable, computed } from 'mobx';
import { Provider, inject, observer } from 'mobx-react';
import React, { Component } from 'react';

class Store {
  @observable show = true;
  @action toggleShow = () => this.show = !this.show;
  @computed get Show() {
    return this.show;
  }
  @computed get ButtonChar() {
    return this.show ? '∧' : '∨';
  }
}

const store = new Store();

@inject('store')
@observer
class TestMobX extends Component {
  constructor() {
    super();
    this.onclick = this.onclick.bind(this);
  }

  onclick() {
    this.props.store.toggleShow();
  }

  render() {
    return (
      <div>
        <button type="button" onClick={this.onclick}>
          {this.props.store.ButtonChar}
        </button>
        <p>{this.props.store.Show && 'Hello Next.js@7 with MobX.'}</p>
      </div>
    )
  }
}

export default = () => (
  <Provider store={store}>
    <TestMobX />
  </Provider>
);
```
.babelrc中新增的babel配置语句是用来支持Javascript的decorator语法的。这样我们就可以使用`@inject('store')`和`@observer`等类似的语法了。

Store里只有一个可观察的变量，show。可观察用decorator `@obserable`来修饰。在MobX里，“可观察”表示只要直接或者间接使用了该变量，当该变量变化的时候，就会自动触发行为。`@computed`一般用来修饰一个属性函数，该函数的返回值一般是从可观察变量计算而来。同样，依赖了`@computed`属性，当可观察变量变化的时候，也会自动触发行为。

@observer是mobx-react库里的。一般只用来修饰React组件。当可观察属性变化的时候，被`@observer`修饰过的组件，会重新被渲染。在本例中，按钮上的字符和`<p>`组件的显示，都会根据`store.show`的变化而变化。

`<provider>`和`@inject`是用来做属性注入的。使用了属性注入，就不用把属性一层层传进子组件去了。如果组件嵌套不深，则可以自行手动传递属性。

## Autorun
我们可以用autorun来注册自动调用函数。
```javascript
class Store {
  constructor() {
    autorun(() => console.log(this.show));
  }
  @observable show = true;
}
```
这样一来，当show变化的时候，就会把它的值打印到console里。

一般我们使用autorun来处理一个可观察变量依赖另一个可观察变量的情况。当一个可观察变量既会被用户/系统修改，又会被其它可观察变量修改，则无法使用`@computed`。

## Async Computed Value
`@computed`修饰的是get关键字。在现有的Javascript语法中，get函数不可以是async的。如果要使用异步的计算属性，我们需要使用一个叫做'computed-async-mobx'的库。本课对此不再赘述，如有需要可以自行阅读参考资料。

## 注意事项
通常我们都会需要把数组变成“可观察”的，但可观察数组并不是真正的Javascript Array。我们有时候会选择把整个可观察数组传递给子组件，而子组件却依赖于真正的Array。这有可能会对数组的可观察性造成问题，导致数组变化无法触发行为。因此，在把可观察数组传递给子组件之前，创建一份浅拷贝（使用`Array.slice()`），是一个最佳实践。
```javascript
render() {
  const cars = this.props.store.cars.slice();
  return <CarList cars={cars} />;
}
```
## 参考资料
1. [MobX中文文档](https://cn.mobx.js.org/)
1. [MobX英文文档](https://mobx.js.org/)
1. [Ten minute introduction to MobX and React](https://mobx.js.org/getting-started.html)
1. [computed-async-mobx](https://github.com/danielearwicker/computed-async-mobx/)
