# 第十课 前端技术入门之布局大法

## Inline元素和block元素
一般而言，HTML元素的显示方式分为两种，inline和block。

Block元素一般都从一个新的行开始。Block元素可以设置元素的width和height。
以下元素为block元素：
`<div> <p> <form> <figure> <h1> <li> <fieldset>`

Inline元素默认不换行，且默认宽度与内容宽度相同。多个inline元素从左到右排列在同一行中。Inline元素的默认高度取决于内部元素（一般就是文字）的高度。为inline元素设置width和height是没有效果的。同样，为inline元素设置垂直内边距，边框和外边距也不会影响元素的高度。修改inline元素高度的唯一方法是修改行高（line-height）。修改inline元素宽度的唯一方法是修改元素的水平内边距，边框或外边距。
以下元素为inline元素：
`<a> <img> <input> <span> <label> <textarea> <button>`

这就是为什么之前我们看到表单里的复选框(即`<input>`)都在一行的原因。

我们可以使用CSS属性display来改变元素默认的显示属性。
```css
input, label {
	display: block
}
```
display还可以为none，这样会使得元素彻底消失，连任何位置都不占用。注意`display: none`与`visibility: hidden`的区别。两者都会使元素不可见，但前者连位置都不占，后者还会保留元素的位置和大小（显示是空白）。

## Inline-block
显示模式inline-block与inline元素类似，多个inline-block元素从左到右排列在同一行中。所不同的是，使用了这种模式，我们就可以为元素设置width和height，以及垂直内边距，边框和外边距。Inline-block特别适合横向排列的一组框或者图标。
```css
div {
	display: inline-block
}
```
## 定位和浮动
CSS有3中基本的定位机制，分别为普通流，绝对定位和浮动。我们可以使用CSS属性position来改变定位机制。

### 普通流
默认的布局模式就是普通流。在普通流下，block元素从上到下垂直排列，并且都占用父元素的全部宽度。CSS表达式`position: static`就表示元素是处于普通流模式下。

### 相对定位
相对定位可以被认为是普通流的一种。当我们使用`position: relative`将元素修改为相对定位时，我们可以通过修改垂直或水平位置让元素“相对于”它原来的位置移动。但一般我们很少真正让它移动。我们使用相对定位，绝大部分原因时为了给绝对定位铺路搭桥。

### 绝对定位
绝对定位使得元素跳出普通流的布局。其它元素照常按照普通流来布局，就相当于绝对定位的元素不存在一样。绝对定位元素的初始位置相对于离它最近的已定位的（设置成相对定位就是“已定位”）父元素。如果所有文档都处于普通流下，则初始位置相对于HTML标签。一般在项目中，我们会把需要固定在屏幕某个角落的元素设置为绝对定位，再根据它的具体位置（相对于面板还是画布抑或是某个元素），找到适合的祖先，将其设置为相对定位。举个例子，在CSS的下拉菜单中，一级菜单项会设置成相对定位，整个二级菜单会设置成绝对定位。
由于绝对定位可以覆盖其它元素，因此可通过设置z-index属性来控制显示顺序。z-index的值大的元素，就可以覆盖z-index值较小的元素。
CSS表达式`position: absolute`表示绝对定位。

### 固定定位
固定定位是绝对定位的一种，区别在于固定定位的初始位置相对于视口（viewport），也就是浏览器窗口。
CSS表达式position: fixed表示固定定位。

### 浮动
普通流只能创建从上到下的布局。如果要创建现代三栏布局或者是类似桌面应用的侧边栏+主窗口的布局，则在CSS2下必须对元素进行浮动。
浮动的规则极度复杂。而且一个元素位置不对会破坏整个布局。因此在CSS3中，有更好的布局方式（Flexbox和CSS Grid）来生成现代布局。
除非你的Web App需要兼容老的浏览器（例如：IE），否则就不要浮动了。

## 现代布局之Flexbox
早期的CSS布局都是为了做网站，它的布局往往相对比较固定。因此，我们调整元素的定位模式，就可以实现。但是，当互联网发展成Web App的时候，它的布局就比网站要复杂很多。想象一下做一个Web版的IDE，它有一个垂直侧边栏，然后剩余空间被主窗口占据。甚至上下左右都有边栏，中间有一个自适应大小的主窗口，边栏还可以隐藏和显示。如此复杂的布局，使用定位技术已经很难解决了。因此CSS3引入了Flexbox。

下面我们生成一个侧边栏和主窗口，高度占满浏览器，侧边栏宽度固定，主窗口宽度自适应。
```javascript
import styled, { createGlobalStyle } from 'styled-components';
	
const GlobalStyle = createGlobalStyle`
		html, body {
		margin: 0;
		padding: 0;
	}
`;

const Ide = styled.div`
	display: flex;
	height: 100vh;
`;

const Sidebar = styled.div`
	border: solid 5px gray;
	width: 375px;
`;

const Main = styled.div`
	border: solid 5px gray;
	flex: 1;
`;

export default () => (
	<Ide>
		<Sidebar>Sidebar</Sidebar>
		<Main>main</Main>
		<GlobalStyle />
	</Ide>
);
```
Ide窗口是所有窗口的父窗口，我们称之为容器，我们将它的显示方式设置成了flex，这就使用了Flexbox。Flexbox的默认方向是横向，也就是说，`<Sidebar>`和`<Main>`这两个div，并列在一行里，且高度都与父窗口相同。高度单位vh是视口（浏览器窗口）高度的1/100。100vh就意味着占满浏览器窗口的高度。
`<Sidebar>`的宽度被设置成了375px，这是一个宽度固定的div。`<Main>`的flex被设置成了1，这表明，一行中剩余的宽度，全部被占据。
最后，将html和body的margin和padding都设置为0。这样高度就正好占据浏览器视口。否则由于浏览器默认margin和padding的存在，整个盒的大小（width和height设置的是内容的大小，盒的大小要加上两倍的padding, margin和border）将超过浏览器视口，这会导致滚动条的出现。

如果一个容器里有多个flex为1的子元素，则这些子元素平均分配剩余宽度。如果多个子元素的flex值不相等，则按flex值的比率来分配剩余宽度。比如，有三个子元素，flex值分别为1，2，3；则三个子元素占据剩余全部宽度，三个子元素的宽度比为1：2：3。

我们把上面的main窗口再平均分成2个窗口，这次是横向切分。
```javascript
import styled, { createGlobalStyle } from 'styled-components';
	
const GlobalStyle = createGlobalStyle`
		html, body {
		margin: 0;
		padding: 0;
	}
`;

const Ide = styled.div`
	display: flex;
	height: 100vh;
`;

const Sidebar = styled.div`
	border: solid 5px gray;
	width: 375px;
`;

const Main = styled.div`
	flex: 1;
	display: flex;
	flex-direction: column;
`;

const Main0 = styled.div`
	border: solid 5px gray;
	flex: 1;
`;

const Main1 = styled.div`
	border: solid 5px gray;
	flex: 1;
`;

export default () => (
	<Ide>
		<Sidebar>Sidebar</Sidebar>
		<Main>
			<Main0>Main0</Main0>
			<Main1>Main1</Main1>
		</Main>
		<GlobalStyle />
	</Ide>
);
```
我们可以看到，原来的`<Main>`窗口里面多了`<Main0>`和`<Main1>`两个子窗口。我们仍然需要把`<Main>`窗口的显示模式设置成flex，因为显示模式是不会继承给子窗口的。此外，我们还为`<Main>`窗口设置了flex-direction: column，这意味着，它里面的子窗口，是排成一列的，且它们的宽度与父窗口一致。`<Main0>`和`<Main1>`两个子窗口，它们的flex值都是1，这样就表示，它们的高度相等且共同占满父窗口（也就是各占50%）。

显示效果如下，当我们调整浏览器宽度和高度的时候，`<Main0>`和`<Main1>`的宽高度会自适应。



有了Flexbox，做类似IDE这样的切分窗口的Web App布局已经不在话下了，且我们并不需要知道浏览器的宽度和高度值。它几乎可以为我们生成任意复杂的可伸缩布局。

## 现代布局之CSS Grid
CSS Grid支持把N个div拼成一个类似表格的布局。它不仅能支持任意行列高度，还可以支持跨列，跨行和自定义行列间距。CSS Grid使用得较少，一般在使用Flexbox布局都很繁琐的情况下，才会考虑使用CSS Grid。因此本课暂不赘述，大家可以自行阅读参考资料。

## 居中
居中曾是CSS的难题。在CSS2时代，为了实现居中，出现过各种奇技淫巧，包括在CSS里计算出坐标位置，拿表格的单元格进行布局等等。幸好CSS3及时出现，一举解决了这个难题。如今我们不再需要使用各种“技巧”，也能够实现居中。

### Flexbox居中
我们在可以使用Flexbox的情况下，居中就变得轻松了许多。
```javascript
import styled, { createGlobalStyle } from 'styled-components';
	
const GlobalStyle = createGlobalStyle`
	html, body {
		margin: 0;
		padding: 0;
	}
`;

const Parent = styled.div`
	display: flex;
	height: 100vh;
	background-color: lightgray;
`;

const Child = styled.div`
	width: 300px;
	height: 57px;
	background-color: gray;
	margin: auto;
`;

export default () => (
	<Parent>
		<Child>
			Main Window
		</Child>
		<GlobalStyle />
	</Parent>
);
```
如果父元素被设置成了`display: flex`，那么只需要让子元素的margin为auto，即可实现上下左右都居中。如果只需要水平居中，那么可以把上下的margin设置为0，margin: 0 auto。如果只需要垂直居中，则可以把左右的margin设置为0，margin: auto 0。还有一种居中的方法，就是为父元素设置`justify-content: center`，这样就可以实现水平居中。如果再为父元素加上`flex-direction: column`，就可以实现垂直居中。如果要垂直和水平都居中，可以为父元素同时设上`justify-content: center`和`align-items: center`。

### 文字居中
我们可以注意到，在`<Child>`标签中的文字“Main Window”，并不居中。如果我们需要使得文字居中在div内部，会有一些不同。

### 文字水平居中
```javascript
const Child = styled.div`
	text-align: center;
`;
```
CSS语句`text-align: center`可以实现文字的水平居中。

文字垂直居中
```javascript
const Child = styled.div`
	height: 57px;
	line-height: 57px;
`;
```
将line-height设置成与height一样，则可以实现内部文字垂直居中。这种方法的缺点是，我们必须知道div的高度值。关于行高line-height的详细定义和使用，大家可以去阅读参考资料。如果我们需要让文字在水平和垂直都居中，则把上面的CSS属性全部都设上即可。

还有一种让文字居中的方法，就是使用Flexbox。
```javascript
const Child = styled.div`
	display: flex;
	justify-content: center;
	align-items: center;
`;
```
我们把Child也变成Flexbox，那么就可以使用justify-content和align-items来对内部的文字（子元素）进行两个方向上的居中了。

### 绝对定位居中
如果某个div是绝对定位的，那么上面的方法就失效了。因为绝对定位的元素已经脱离了文档流。我们使用下面的方法进行居中。
```javascript
const Title = styled.div`
	left: 0;
	right: 0;
`;
```
这样可以进行水平居中。垂直居中当然就是把top和bottom都设置成0啦。都居中则把4个都设置成0。

## 参考资料
1. [A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
1. [A Complete Guide to Grid](https://css-tricks.com/snippets/css/complete-guide-grid/)
1. [Centering in CSS: A Complete Guide](https://css-tricks.com/centering-css-complete-guide/)
1. [Centering Things](https://www.w3.org/Style/Examples/007/center.en.html)
