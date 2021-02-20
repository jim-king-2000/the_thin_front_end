# 第九课 前端技术入门之Styled-Components

## 简介
CSS的出现，使得前端语义分明。在最早期不使用CSS的年代，HTML一片混乱。标签既要描述内容，又要兼顾排版。当我们在HTML文档中看到`<table>`的时候，我们无法知晓这是一个真正的表格，还是被用来把元素排列整齐。在CSS出现之后，情况大为改观。HTML只负责提供内容，而内容的显示方式则全部由CSS来负责。这时，随着时代渐渐的发展，互联网开发已经逐渐从网站，过度到了Web App。这对CSS提出了更高的要求，不仅需要CSS支持更灵活的布局(后来通过CSS3实现支持)，而且需要CSS复用性好，简单易维护。因此，CSS Module应运而生。有了它，就可以在React的文件中直接import CSS。但使用它却需要大量使用class。同样一个按钮，如果有8种样式，则有8个class。这些class的名称全部暴露在全局，导致了全局名称污染。尽管后来CSS Module引入了namespace的概念，但仍然没有减少class的数量，且项目仍需维护CSS文件。不久以后，业界就从CSS Module过渡到了CSS-in-JS。顾名思义，就是把CSS放到了Javascript中。至此，我们不再使用class，也不再编辑CSS文件，一切尽在Javascript中。

Styled-Components是CSS-in-JS的框架之一。其强大的功能和简洁的语法使其迅速流行。

## 安装
1. 安装2个库：`npm install --save styled-components babel-plugin-styled-components`
1. 在根目录新建文件.babelrc，内容如下：
```json
{
  "presets": [
    "next/babel"
  ],
  "plugins": [
    [
      "babel-plugin-styled-components",
      {
        "ssr": false
      }
    ]
  ]
}
```
## 开始使用
我们使用styled.{标签}来使用styled-components。
```javascript
import Link from 'next/link';
import styled from 'styled-components';

const Title = styled.p`
  font-size: 75px;
  text-align: center;
  color: palevioletred;
`;

const Index = () => (
  <div>
    <Link href='/about'><a>About</a></Link>
    <Title>Hello, Styled-Components</Title>
  </div>
);

export default Index;
```
这里，我们使用styled.p来新建一个带有style的`<p>`标签。在styled.p的后面，跟了一个由反引号包裹的字符串。这个字符串的语法就是template string。整个语句的语法叫做tagged template。
在template string里面，就可以填写标准的CSS语句。支持所有的语法，包括伪类等。
```javascript
const Title = styled.p`
  font-size: 75px;
  text-align: center;
  color: palevioletred;
  &:hover {
    color: black;
  }
`;
```
伪类的语法与CSS的伪类基本一致。符号&代表被修饰的元素本身，因此在上例中，当鼠标位于Title标签上时，文字的颜色变成了黑色。

## 属性
我们可以把属性传递给styled-components。
```javascript
import Link from 'next/link';
import styled from 'styled-components';

const Title = styled.p`
  font-size: 75px;
  text-align: center;
  color: ${props => props.color || palevioletred};
`;

const Index = () => (
  <div>
    <Link href='/about'><a>About</a></Link>
    <Title color='green'>Hello, Styled-Components</Title>
  </div>
);

export default Index;
```
我们可以把箭头表达式传递给CSS的项。表达式用${}包住。箭头表达式的参数就是传递给标签的属性。箭头表达式的返回值就是CSS项的取值。

## 自定义标签
HTML的标准标签使用.点号进行修饰。如果需要修饰React的自定义标签，则使用括号。
```javascript
const StyledButton = styled(Button)`
  color: palevioletred;
`;
```
## 全局CSS
有时我们需要修改标签`<html>`和`<body>`的样式。这两个标签已经被包裹到了Next.js的内部，我们无法定义这两个标签。但我们可以使用createGlobalStyle来修改这两个标签的样式。
```javascript
import Link from 'next/link';
import styled, { createGlobalStyle } from 'styled-components';

const Title = styled.p`
  font-size: 75px;
  text-align: center;
  color: ${props => props.color || palevioletred};
`;

const GlobalStyle = createGlobalStyle`
  html, body {
    margin: 0;
    padding: 0;
    font-family: sans-serif;
    min-width: 1024px;
    min-heiht: 768px;
  }
`;

const Index = () => (
  <div>
    <Link href='/about'><a>About</a></Link>
    <Title color='green'>Hello, Styled-Components</Title>
    <GlobalStyle />
  </div>
);

export default Index;
```
函数createGlobalStyle内部的语法与CSS完全一致。注意CSS选择器语法中逗号与空格的区别。这里，我们写成html, body，表示这两个标签都应用与花括号中的样式。如果写成了html body，则表示只有`<html>`中的`<body>`标签才应用花括号中的样式（也就是说`<html>`标签并不应用花括号中的样式）。

## 服务端渲染
当我们刷新上面的页面的时候，我们会发现样式会发生明显的变化。这是因为styled-component默认是客户端渲染。想开启服务端渲染，需要增加以下几个步骤：

1. 将.babelrc修改成下面的内容：
```json
{
  "presets": [
    "next/babel"
  ],
  "plugins": [
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
1. 在pages新增_document.js文件，内容如下：
```javascript
import Document, { Head, Main, NextScript } from 'next/document';
import { ServerStyleSheet } from 'styled-components';

export default class MyDocument extends Document {
  static getInitialProps ({renderPage}) {
    const sheet = new ServerStyleSheet();
    const page = renderPage(App => props => sheet.collectStyles(<App {...props} />));
    const styleTags = sheet.getStyleElement();
    return { ...page, styleTags };
  }

  render () {
    return (
      <html>
        <Head>
          <link rel='shortcut icon' type='image/x-icon' href='/favicon.ico' />
          {this.props.styleTags}
        </Head>
        <body>
          <Main />
          <NextScript />
        </body>
      </html>
    );
  }
}
```
这样，刷新就不再闪烁了。

## 结尾
学到这里，我们才真正意义上可以开始构建完整的前端工程。使用最新的技术构建的前端工程，有以下一些特征：
* 纯JS，不需要再维护HTML和CSS文件。
* 组件化好，代码复用率高。
* 支持服务端渲染，页面用户体验极好，与桌面应用的渲染效果几乎相同。
* 前后端同构，服务端渲染和客户端渲染共享同一段代码。

但我们也发现，前端的技术日新月异，碎片化极为严重。不同的技术之间完全不能兼容。因此，前端上手的难点就在于技术点驳杂，需要学完HTML/CSS/React/Next.js/Styled-Components等一串东西，才可以真正构建一个完整的前端应用。然后如果我们换一个框架，比如换成使用Google的Angular或VUE2，则几乎没有一行代码是相同的。这就导致没有一个框架和技术是可以亘古不变的，或是可以沿袭的。今天流行的框架和技术，短则几年，长则十几年，必然要过时。一个前端工程师必须终生学习，才能跟上时代的步伐。一个成熟的互联网产品，也只有经历一遍又一遍的重写，才能跟随时代的潮流。我们能做的，只能是通过增长自己的经验，使得学习或重写的成本变得越来越小而已。

## 参考资料
1. [官方网站](https://www.styled-components.com/)
