# 第八课 前端技术入门之Next.js

## Next.js简介
从零开始创建React工程十分繁琐。但用React实现页面服务端渲染，更繁琐。这时，Next.js横空出世，为我们提供了从创建工程，到ES转译（浏览器不能解析JSX，也不能解析太新的甚至是未标准化的JS语法，必须用babel把这些语法翻译成较老的JS，这个过程叫做转译，transpile，它是translate和compile各取一半拼起来的），到服务端渲染等一揽子功能。

此外，Next.js还提供了服务端渲染（前后端同构），serverless等高级功能。它可以说是当今前端最新技术的组合。使用Next.js开发前端，可以极大的提高开发效率。

## 静态文件服务器
我们在前面的课程中已经见过了Next.js的静态服务器。凡是放入目录static中的文件，都可以用URL：“/static/”来访问。这样，我们就可以把logo，图片，字体等文件放入其中。

## Hello Next.js
创建一个Next.js的工程十分简单。步骤如下：
1. 在"pages"子目录里新建index.js文件，内容如下：
```javascript
export default () => <p>Hello Next.js</p>;
```
2. 启动服务器：npm run dev
3. 访问： http://localhost:3000

## 服务端渲染
按F5刷新页面，页面纹丝不动。这是因为，Next.js默认就是服务端渲染，当浏览器请求HTML的时候，它的内容就是全量页面。这时所有的内容就都已经渲染出来了。当JS加载且运行完毕之后，还会进行一次客户端渲染。由于客户端渲染的结果跟服务端渲染是一样的，而React只会重新渲染变化的DOM，因此客户端渲染什么都不做。由于没有任何一次渲染会渲染空白页，因此页面永远不会闪烁。这时，页面的渲染效果已经跟App别无二致了。

在React之前的时代，服务端渲染通常是某一套代码，比如Java，Python或者C#，使用HTML模板拼出页面；客户端渲染又是另一套代码，使用Javascript和jQuery去处理DOM的更新逻辑。到了React时代，服务端渲染和客户端渲染使用的是同一套代码。这叫做同构（isomorphic)。这大大简化了前端的开发。只是我们时刻要注意，我们前面写的组件，既在服务端运行（React元素在服务端被渲染成HTML），又在客户端运行。

如果我们需要某段代码只在服务端或者客户端运行，我们可以使用`process.browser`来做区分。它在服务端为`false`，在客户端为`true`。

## 文件路由
Next.js的前端路由方式有点类似PHP。当我们需要为/about增加一个页面的时候，我们只要在pages下面增加一个About.js文件即可。在页面之间，我们使用Link组件来做跳转。
```javascript
// index.js
import Link from 'next/link';

export default () => (
  <div>
    <Link href='/about'><a>About</a></Link>
    <p>Hello Next.js</p>
  </div>
);

// about.js
import Link from 'next/link';

export default () => (
  <div>
    <Link href='/'><a>Home</a></Link>
    <p>Hello About.</p>
  </div>
);
```
如果我们直接使用`<a>`来做链接，那么就会访问一次服务器。而用Link做跳转，是不会访问服务器的，属于前端路由。由于是一个文件对应一个路径，因此又叫“文件路由”。

## 数据获取
在真实的项目中，网页中的内容往往根据后端API返回的数据来决定。在Next.js中，我们使用getInitialProps函数来获取数据。
```javascript
// index.js
import Link from 'next/link';
import fetch from 'isomorphic-unfetch';

const Index = ({ names }) => (
  <div>
    <Link href='/about'><a>About</a></Link>
    <form>
      <fieldset>
        <legend>请选择</legend>
        {names.map(name => 
          <label><input type='checkbox' value={name} />{name}</label>
        )}
      </fieldset>
    </form>
  </div>
);

Index.getInitialProps = async () => {
  const res = await fetch('http://localhost:3000/static/names.json');
  const names = await res.json();
  return { names: names.names };
};

export default Index;
```
我们看到，我们在`getInitialProps`函数中，调用了`fetch`来获取服务器端的数据。这里使用的`fetch`来自于一个叫做"isomorphic-unfetch"的库，它既能在服务端使用，又能在客户端使用。获取数据之后，我们返回了一个对象。这个被返回的对象，会作为Index组件的属性，从而被使用。在Index组件中，我们根据数据，渲染了一组checkbox。我们可以频繁按F5来刷新页面，即使页面带数据加载，在刷新的时候仍然纹丝不动，这就是服务端渲染的威力。

我们还需要在static子目录下创建一个名叫names.json的数据文件，内容如下：
```json
{
  "names": [
    "王大力",
    "赵小雷",
    "李亭亭"
  ]
}
```
如果是类组件，则`getInitialProps`函数可以被写成类的静态函数。
```javascript
export default class extends Component {
  constructor(props) {
    super(props);
  }

  static async getInitialProps() {
    ...
  }
}
```
注意：`getInitialProps`既有可能在服务端被调用，又有可能在客户端被调用。因此该函数依赖的函数，也应该在客户端和服务端都能运行。且该函数只能出现在pages子目录下的组件中，其它目录下的组件的`getInitialProps`不会被自动调用。

## 参考资料
1. [官网教程](https://nextjs.org/)
