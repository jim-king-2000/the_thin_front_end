# 第二课 前端开发生态系统简史

## 静态页面时代
这个时代只能编写静态页面，Javascript还不流行。幸好这个时代国内的朋友基本上没有经历过。

## 混沌时代
这个时代已经可以编写动态页面了，Javascript大行其道。人们使用结合了字体和粗体的标签来创建所需要的视觉效果。表格在当时成为了一种布局工具而不是显示数据的方式。这个时代出现了著名的网页三剑客，即Dreamweaver, Fireworks和Flash。他们可以使用简单的鼠标操作构建出复杂的表格布局。但遗憾的是，这些工具并没有使得事情变得简单。嵌套的表格和“分隔线GIF”把代码弄得非常混乱。而且这些布局极其脆弱，一不小心删除了错误的标签，整个布局就会崩溃。此外，Javascript代码也混在HTML中。这使得整个项目混乱不堪，毫无工程化可言。

## 后端MVC时代
这个时代已经有了相当程度的工程化，前端页面已经清晰的分成了HTML, CSS和Javascript三个部分。服务器端的程序也已经分化成了Model，View和Controller三个部分。这个时代的网页(HTML)主要由后端模板与数据拼接而成。在这个时代，每一次的网页切换，每点一次控件，都要做一次服务端渲染。这个时代已经可以用多种语言编写前端，曾经流行的库主要有Java Struts, PHP, ASP.NET MVC, Node.js Express等。这种技术主要使用reload页面来处理数据的变化，Javascript只是配角，用来处理一些简单的页面变化和客户端验证。数据上传也只有提交表单的方式。网页虽然是动态的，但是离App的体验还很远。

## AJAX时代
这个时代几乎是与后端MVC同时代的，两者共生了很长的时间。一开始，人们使用后端MVC生成初始网页，然后用Javascript来处理页面的各种控件操作。后来人们发现，连后端MVC都不需要了，直接用一个空白的页面，然后用Javascript加载一个网页的模板，再用Javascript加载页面数据。jQuery是这个时代当之无愧的明星。异步请求和客户端渲染也使得网页的操作体验与桌面应用越来越接近。

## SPA时代
AJAX成熟之后，人们发现，在不同的URL之间切换的时候，也不用访问服务器和刷新页面，页面之前的切换从“后端路由”变成了“前端路由”。一切都可以在AJAX的控制下悄无声息的进行。因此，单页应用（Single-Page Application）大行其道。单页应用有着和桌面应用极其相似的用户体验，在各页之间切换迅速流畅且无闪烁（因为不需要刷新页面）。在这个时代，Javascript变得极度复杂。在工程化要求的催生下，出现了各种SPA前端框架，比较著名的有Backbone，EmberJS，KnockoutJS和AngularJS等。

## Virtual DOM时代
这个时代是一个突破的时代。React是这个时代最为耀眼的明星。在这个时代，前端已经做到了组件化和同构（同一套代码做服务端渲染和客户端渲染）。React生态圈结合ES6(Javascript的新版本)，Node.js，HTML5/CSS3，CSS-in-JS，共同推进了程序员的全栈化。研发团队也渐渐进化成专业的前端工程师编写/维护前端组件库，全栈工程师使用组件库编写前后端所有业务逻辑的分工。前后端工程师不再需要像以前那样，共同审阅产品经理的设计说明书，共同制定前后端接口。这些工作现在都由全栈工程师一人完成，沟通成本大大降低。而组件化使得前端中间件的复用性达到甚至超过了后端的中间件复用性，进一步提高了研发效率。在整个生态圈和后端技术，运维技术的共同支撑下，现代的前端体验已经跟App不相上下，且还能跨平台，易于发布和运维。这就是为什么很多手机端的App其实就是在Android和iOS上各做一个嵌着Chrome的“壳”，全部的操作都是移动网页在背后提供服务。