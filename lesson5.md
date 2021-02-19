# 第五课 前端技术入门之DOM & BOM

## 什么是DOM
DOM（Document Object Model）是W3C（万维网联盟）定义的以对象形式访问HTML文档的标准，它是中立于平台和语言的接口。它允许程序和脚本动态的访问和更新文档的内容、结构和样式。

## 什么是BOM
BOM（Browser Object Model）是浏览器定义的对象，提供除了文档之外的其它功能的访问。

## DOM节点和属性
HTML文档中所有的内容都是DOM节点。整个文档是一个文档节点，每个HTML元素是元素节点，元素内部的文本是文本节点，元素的属性是属性节点，注释是注释节点。W3C提供了丰富的API来对节点进行各种各样的操作，如获取节点，增加子节点等。

我们可以使用Javascript来获取DOM元素节点。
```javascript
const Button = document.getElementById('toggleButton');
```
在这里，document就指代文档节点。API getElementById会获取id='toggleButton'的元素节点。

获取了元素节点之后，就可以访问/修改元素的属性了。
```javascript
<input type='button' value='toggle' id='toggleButton' />

const Button = document.getElementById('toggleButton');
console.log(Button.value);
```
由于在HTML文档中，按钮的属性value值是'toggle'。那么使用Button.value获取的属性值就是'toggle'。

如果需要修改元素的CSS，则可以使用style属性。

## DOM事件
W3C规定了一系列的DOM事件可供处理。当用户进入或离开页面时，会触发onload和onunload事件；当用户单击按钮时，会产生onclick事件；当用户将鼠标指针移动到元素上或从元素上离开时，会触发onmouseover和onmouseout事件；当我们修改文本框或者单/复选框的时候，会触发onchange事件。我们可以为这些事件设置回调函数。

触发了事件之后，浏览器会给回调函数一个参数，我们称之为Event对象。Event对象里规定了键盘和鼠标的各种状态以及事件发生的时间和类型等属性。但我们最关心的还是target属性，它指代了触发该事件的元素。访问该元素的属性，我们就可以获取用户的输入。
```javascript
window.onload = e => console.log(e);
```
用上面的程序，我们就可以在浏览器的console窗口观察到Event对象的所有属性。

即使我们为事件设置了回调函数，一些默认的行为（比如`<a>`，click之后就要跳转到新的url）仍然会得到触发。如果需要阻止触发默认动作，需要在回调函数中调用e.preventDefault()，或者让回调函数返回false。

## 实例：点击按钮，显示/隐藏文本。
我们现在演示一小段Web App，上面只有一个按钮和一段文本。点击按钮，文本消失。再次点击按钮，文本重新显示。

创建static/about.html，内容如下：
```html
<!DOCTYPE HTML>
<html>
  <head>
    <script src='/static/about.js'></script>
  </head>
  <body>
    <input type='button' value='toggle' id='toggleButton' />
  <p id='canvas'>Hello DOM</p>
  </body>
</html>
```
创建static/about.js，内容如下：
```javascript
window.onload = () => {
  let isShown = true;
  let Canvas = document.getElementById('canvas');
  let Button = document.getElementById('toggleButton');
  Button.onclick = () => {
    isShown = !isShown;
    Canvas.style.display = isShown ? 'block' : 'none';
  };
};
```
我们的主程序必须放在`window.onload`里面。否则HTML还未被解析，getElementById无法获取元素。我们为Button的onclick事件注册了一个回调函数，当我们点击按钮的时候，它就会被执行。在回调函数里，我们根据isShown变量来设置`<p>`元素的CSS属性。当CSS属性display为'none'的时候，元素就被隐藏。

## AJAX
如果我们在用户点击的时候发起远程API调用，再根据远程API返回的数据来更新DOM，这就是AJAX（Asynchronous Javascript And XML ）。虽然我们早已不再使用XML做数据格式（今天我们广泛使用的数据格式是JSON），但该名称还是沿用了下来。

由于Ajax发起的是异步调用，不会让页面僵死，因此Ajax技术可以提供优良的用户体验。这项技术一问世就迅速得到流行，一度统治业界长达十余年之久。其中最声名远播的框架当属jQuery，它简洁轻快，拥有丰富的插件，且兼容各种主流浏览器。如今我们已不再使用jQuery，但Ajax的思想却仍被保留了下来。当我们使用React去制作Web App的时候，我们仍然使用的是Ajax技术。

AJAX技术发展到了成熟的阶段，项目中的HTML文件就变得只剩下了一个空div。然后所有的内容都用AJAX来渲染。就连跳转到另一个URL，新页面也完全用AJAX来渲染。这使得页面几乎不需要重新加载，用户体验与本地App不相上下。但它的缺点也很明显，一旦发生reload，浏览器就渲染出一个空白页面，等加载完Javascript，浏览器再渲染出一个与当前URL相对应的模板HTML，然后再执行获取数据的Javascript，得到数据后，浏览器最终才渲染出所有的元素。因此，一次完整的页面渲染，需要3个甚至更多个http回合（一个回合200 ~ 300毫秒，三个回合至少要600 ~ 900毫秒），在视觉上就是一个明显的闪烁+缓慢渐进式加载。这就是客户端渲染的顽疾。只有服务端渲染可以解决这个问题，因为服务端渲染的HTML是一次性直出的（只有一个http回合，200 ~ 300毫秒即可开始渲染全量页面）。

## 同源策略与跨域
为了防止CSRF（Cross-Site Request Forgery，跨站请求伪造）攻击，浏览器有同源策略。两个页面的协议（就是http或https），端口和域名都相同，则两个页面具有相同的源。浏览器默认限制当前页面的Javascript，发起网络请求（XHR或Fetch）的时候只能访问同源URL。这意味着，搜狐网页上面的Javascript，默认是无法访问新浪的API的。注意，HTML里的`<script>`, `<img>`等标签的src属性，可以与页面不同源。

但对于一个中大型的网站来说，页面与API很可能分布在不同的域名。这时我们就需要做跨域访问。如果只是做跨域GET的话，我们可以使用JSONP（JSON with Padding）技术。如果跨域要POST， GET等，我们可以使用CORS（Cross-Origin Resource Sharing）技术。使用这两种技术，都是需要后端服务器配合的。跨域技术本文不再赘述，大家有兴趣可以自行阅读参考资料。

## 参考资料
1. [HTML DOM教程](http://www.w3school.com.cn/htmldom/index.asp)
1. [JSONP教程](https://www.w3cschool.cn/json/json-jsonp.html)
1. [浏览器的同源策略](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)
1. [HTTP访问控制（CORS）](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)
