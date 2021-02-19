# 第三课 前端技术入门之HTML

## HTML基本结构
一个最简单的HTML如下所示：

```html
	<!DOCTYPE HTML>
	<html>
	  <head></head>
	  <body>
	    <p>Hello HTML</p>
	  </body>
	</html>
```

我们可以看到，一个HTML文档有两部分组成。第一部分是文档声明`<!DOCTYPE HTML>`，表示这是一个HTML文档。第二部分是根元素，`<html></html>`，所有的内容都在其中。

## HTML标签
HTML的内容以标签的形式来呈现。HTML的标签都是预先规定好的，不可以任意创造。我们先来介绍标签的规则。

HTML标签规则:
* 标签必须以开始标签起始，以结束标签终止。（开始标签前面加上斜杠就是结束标签）
* 开始标签和结束标签之间的内容称为元素的内容。
* 空元素可以在开始标签中直接关闭，比如`<img src='URL' />`。
* 大多数标签可以拥有属性，上面src就是img标签的属性。属性名一般都是预先定义好的，但属性值可以由我们来填写。属性值必须用单引号或者双引号围住。

常用标签介绍：
* `<!-- abcd -->`: HTML注释。
* `<html>`：根元素标签，所有的标签都必须在它里面。它只能包含两个标签，一个是`<head>`，另一个是`<body>`。
* `<head>`：头标签。在里面可以定义CSS和Javascript等不可见内容。
* `<body>`：身体标签。在里面可以定义文字，图像，表格等可见内容。
* `<title>`：放在`<head>`标签中。定义标题，通常显示在浏览器的Tab上。
* `<link>`：放在`<head>`标签中。定义CSS。
* `<script>`：放在`<head>`标签中。定义Javascript脚本。
* `<a>`：放在`<body>`标签中。定义链接。
* `<img>`：放在`<body>`标签中。定义图像。
* `<h1>` - `<h6>`：放在`<body>`标签中。定义1级到6级标题。
* `<p>`：放在`<body>`标签中。定义段落。
* `<table>`：放在`<body>`标签中。定义表格。
* `<ul>`, `<ol>`：放在`<body>`标签中。定义列表。
* `<div>`：放在`<body>`标签中。定义一组标签组。

## HTML表单
在HTML中，处理用户输入的界面叫表单。表单内的元素就是各种控件，如文本框，密码框，单选框，多选框等。表单的标签叫`<form>`，各控件就放在`<form></form>`中。如下就是一个简单的表单：

```html
	<form>
	  <input type="text" name="email" size="40" maxlength="50" />
	  <input type="password" />
	  <input type="checkbox" checked="checked" />
	  <input type="radio" checked="checked" />
	  <input type="submit" value="Send" />
	  <input type="reset" />
	  <input type="hidden" />
	  <select>
	    <option>苹果</option>
	    <option selected="selected">香蕉</option>
	    <option>樱桃</option>
	  </select>
	  <textarea name="comment" rows="60" cols="20"></textarea>
	</form>
```

现在还无法处理表单的交互。不用着急，在后续的课程中，我们会介绍如何使用React来处理表单逻辑。

## 架设HTML服务器
我们使用Node.js + Next.js来架设一个最简单的HTML服务器（Node.js必须事先安装好）。步骤如下：
1. 新建子目录并且进入该子目录：mkdir test&&cd test
2. 创建package.json：npm -y init
3. 安装Next.js: npm i -y next react react-dom
4. 打开package.json，将scripts修改成下面的内容：
	
```json
	"scripts": {
	  "dev": "next",
	  "build": "next build",
	  "start": "next start"
	},
```
	
5. 新建static和pages子目录：mkdir static pages
6. 在static子目录下创建index.html文件

```html
	<!DOCTYPE HTML>
	<html>
	  <head></head>
	  <body>
	    <p>Hello HTML</p>
	  </body>
	</html>
```
	
7. 启动服务器：npm run dev
8. 打开浏览器，访问 http://localhost:3000/static/index.html
9. 再把index.html的内容换成表单试试。

## 参考资料
1. [W3schools](http://www.w3school.com.cn/html/index.asp)
2. [HTML & CSS: Design and Build Websites](https://item.jd.com/11186682.html)
