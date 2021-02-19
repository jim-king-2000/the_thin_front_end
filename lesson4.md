# 第四课 前端技术入门之CSS

## CSS的作用
在互联网时代的早期，HTML里充斥了控制样式的标签。这使得HTML变得杂乱无章，且语义混乱。后来，标准在语义上做出了切分。一切跟内容相关的，如文字，图像，表格等，都使用HTML来表示。而内容的展示样式，则使用CSS（Cascading Style Sheets，层叠样式表）来表示。

## 在HTML文件中链接CSS文件
我们使用`<link>`标签来链接外部CSS，它位于`<head>`标签内。虽然也可以把CSS写在HTML里面，但一般我们不这样使用。
```html
<!DOCTYPE HTML>
<html>
  <head>
    <link rel='stylesheet' type='text/css' href='/static/index.css' />
  </head>
  <body>
    <p>Hello HTML</p>
  </body>
</html>
```
## CSS的基本语法
最简单的CSS文件内容只有三行。
```css
p {
  background-color: yellow;
}
```
这里的p，我们称之为选择器。它的意思是，花括号中的CSS规则，适用于HTML文档内的所有`<p>`标签。花括号中可以有多条规则，每一条规则必须以分号结尾。规则的内容分为属性和值，之间用冒号分隔。这里我们只定义了一条规则，就是把背景色改为黄色。
CSS对大小写不敏感。但我们一般都使用小写。

## 为服务器增加CSS
1. 打开前一课的index.html文件，在`<head>`标签里加入一行：
`<link rel='stylesheet' type='text/css' href='/static/index.css' />`
2. 在static目录下新建文件index.css，内容如下：
```css
p {
  background-color: yellow;
}
```
3. 启动服务器：npm run dev
4. 访问或刷新 http://localhost:3000/static/index.html

## CSS选择器
CSS的选择器有很多种，以下是最常用的一些选择器。
首先，选择器可以有多个项，他们共享同一组规则，这叫分组选择器。
```css
p，h1, h2, td, dd {
  background-color: yellow;
}
```
分组用逗号隔开。上面的CSS就把标签`<p>, <h1>, <h2>, <td>, <dd>`的背景色都改为黄色。

使用空格把标签隔开，则称之为后代选择器。
```css
div p {
  background-color: yellow;
}
```
上面的规则，仅适用于`<div>`标签里面的`<p>`标签。如果我们的HTML如下所示：
```html
<body>
  <p>Hello HTML</p>
  <div>
    <p>yellow background</p>
  </div>
</body>
```
那么，只有`<p>yellow background</p>`的背景色是黄色的。而`<p>Hello HTML</p>`则不会被上面的CSS规则匹配到，因为它不是`<div>`标签的子标签。

注意：分组选择器与后代选择器的差别仅仅在于是否有逗号，但两者的语义截然不同。虽然这不符合设计原则（设计原则要求，对于截然不同的两种事物，它们看上去也应该截然不同），但现实往往就是如此。

分组选择器和后代选择器都属于可以匹配一大片的选择器。如果我们要精确匹配某一个标签，我们可以使用ID选择器。我们为HTML的某一个标签增加id属性。
```html
<body>
  <p id='thisp'>Hello HTML</p>
  <div>
    <p>yellow background</p>
  </div>
</body>
```
然后用#跟id名来做匹配。
```css
#thisp {
  background-color: yellow;
}
```
需要注意的是，在同一个HTML文件里，id名不可以重复。

ID选择器只能匹配一个标签。如果要匹配一类标签，可以使用类选择器。
```html
<body>
  <p class='sidebar'>Hello HTML</p>
  <div>
    <p class='sidebar'>yellow background</p>
  </div>
</body>
```
```css
.sidebar {
  background-color: yellow;
}
```
类选择器使用.后跟类名来实现（跟面向对象语言调用类成员函数的语法很像）。在HTML里，同名的class可以有任意多个。因此，在很长一段时间内，类选择器得到了极为广泛的使用，甚至一度被滥用。导致在前端项目中出现了成百上千个类名。且当来源不同的css合并到一个项目的时候，会出现类名的冲突，出现所谓的“CSS全局污染”问题。所幸，我们有更好的技术来解决这个问题（见第七课）。

注意：CSS虽然对大小写不敏感，但是对id和class的名称却是大小写敏感的。

最后，通用选择器*，匹配所有的标签。
```css
* {
  margin: 0;
  padding: 0;
}
```
此外还有属性选择器，相邻同胞选择器，子选择器等，由于不太常用，因此我们就不介绍了。

## CSS伪类
有时，我们需要根据文档状态来应用样式。这个时候就需要用到伪类。比如我们现在要在鼠标悬停在文字上的时候，修改文字的背景色。
```html
<body>
  <p>Hello HTML</p>
  <div>
    <p>yellow background</p>
  </div>
</body>
```
```css
p {
  background-color: yellow;
}

p:hover {
  background-color: blue;
}
```
当我们将鼠标悬停在`<p>`标签上的时候，文字的背景色就变成了蓝色，否则就是黄色。

常用的伪类有：
* `:hover`，鼠标悬停。
* `:active`，元素被激活。
* `:focus`，拥有输入焦点。
* `:link`，未访问的链接。
* `:visited`，已访问的链接。
* `:first-child`，该元素的第一个子元素。

## 选择器的组合
这些选择器可以组合起来，例如：
```css
div p.sidebar:hover {
  background-color: blue;
}
```
它匹配鼠标悬停的`<p>`元素，且该元素在`<div>`标签内，class名为sidebar。

## CSS盒模型(Box Model)


一个标准的CSS盒模型，由外边距（margin），边框（border），内边距（padding）和元素（element）共同组成。我们可以使用CSS为它们设置宽度(长度单位一般为像素，即px)。
```css
p {
  margin: 7px;
  border: 3px solid red;
  padding: 5px;
  width: 120px;
  height: 47px;
}
```
注意：我们在CSS中设置的width和height，指的是元素的宽度和高度。但由于外边距，边框和内边距的存在，整个盒的大小是要比元素大小更大的。是的，这有违直觉。但业界已经将错就错很多年了。
注意：元素的背景图片应用于元素和内边距的区域（上图青色区域）。但元素的背景色应用于边框，内边距和元素，边框会盖住背景色。
外边距总是透明的，一般用来控制盒的间隔。
内外边距的距离如果只设置了一个数字，则上下左右的距离都应用这个数字。如果被设置了两个数字，则第一个数字对应上下，第二个数字对应左右。如果设置了四个数字，则分别对应上，右，下，左（顺时针方向）。多个值之间用空格隔开。
```css
p {
  padding: 3px 5px;
  margin: 1px 2px 3px 4px;
}
```
这样`<p>`元素的上下内边距是3px，左右内边距是5px，上外边距是1px，右外边距是2px，下外边距是3px，左外边距是4px。
由于各大浏览器总是会不知所谓的设置各不相同的默认内外边距，因此为了保证页面的显示效果一致，我们通常在CSS的开头强行将所有元素的内外边距清除(值为0的话，就不需要写单位了)。
```css
* {
  margin: 0;
  padding: 0;
}
```
## CSS常用规则
1. 字体属性：
    1. `font-family: Georgia, Serif;`	规定字体。
    1. `font-size: 45px;`	规定字体大小。
    1. `font-style: italic;`	规定字体样式。
    1. `font-weight: bold;`	规定字体粗细。

1. 文本属性：
    1. `color: #FFFFFF;`	设置文本的前景色，井号后是颜色值，代表RGB三个颜色分量，每个分量两位数字。
    1. `line-height: 40px;`	设置行高。
    1. `text-align: center;`	设置文本水平居中方式。
    1. `word-wrap: break-word;`	允许折行。

1. 定位属性：
    1. top: 30px;	设置元素上边界到父元素上边界的偏移。
    1. left: 30px;	同上，但为左偏移。
    1. right: 30px;	同上，但为右偏移。
    1. bottom: 30px;	同上，但为下偏移。
    1. overflow: scroll;	规定当内容溢出元素时的行为。
    1. visibility: hidden;	规定元素可见性。
    1. z-index: 3;	规定重叠元素的显示顺序。

1. 颜色属性
  1. `opacity: 0.5;`	设置div元素的不透明级别，1表示不透明，0表示全透明。

后面等我们需要用到哪些属性的时候，我们再具体讲解。

## 参考资料
1. [W3schools](http://www.w3school.com.cn/css/index.asp)
1. [Stylin' with CSS: A Designer's Guide Third Edition](https://e.jd.com/30410359.html)
1. [CSS Secrets](https://e.jd.com/30322961.html)
