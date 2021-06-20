+++
title = "给恐龙们解释一下当今的CSS - Modern CSS（译文）"
date = 2018-02-11T19:03:51+08:00
categories = ["前端 & 全栈开发"]
tags = ["css"]
toc = true
draft = false
+++

现代 CSS 的进化史 - HTML，CSS，Javascript 是前端开发三大技术，之前讲过了 Javascript，CSS 也是同样野蛮，同样让人费解的技术。

<!--more-->

💬 恐龙 ，远古时代的动物，指的应该是像我这样，因为 Internet 的出现，20 年前就知道，接触过 CSS，但还好没有"入坑"但快要“入土”的程序员。

---

原文：<i class="fas fa-external-link-alt"></i>&nbsp; [Modern CSS Explained For Dinosaurs](https://medium.com/actualize-network/modern-css-explained-for-dinosaurs-5226febe3525)

![0](/images/modern-css/0.png#center)

_对于 web 开发人员，CSS 被认为是其中一个最容易也是最难学习的语言_。上手肯定是比较容易的 - 你可以定义 CSS 的属性和其对应的值，让后把它们应用到某些 HTML 元素，然后，没有然后啦。但是，在一个大型的项目中，当你想把一大坨 CSS 代码组织好，一切就会变得纠缠不清和十分复杂。当你想通过 CSS 只改变一个页面中的某个元素的样式，结果任何一行 CSS 的变动往往也会导致其它页面、其它元素的样式发生意想不到的改变。

CSS 样式具有复杂的内在继承性，为了对付这种复杂性，各种最佳实践应运而生，但问题是，大家对这些最佳实践没有明显的统一认识，而且很多最佳实践是互相抵触的。如果你是第一次学习 CSS，这肯定会让你摸不着头脑。

本文的目的是回顾各种 CSS 的编写方法和工具如何演化到今天（2018 年）的样子。通过了解过去，让我们更容易理解各种 CSS 的编写方法以及如何让它们为我们工作。好，让我们开始吧！

## 使用 CSS 实现简单样式

让我们从一个很简单的网页开始，只有简单的 <span class="kwd">index.hml</span> 和 <span class="kwd">index.css</span> 文件，index.html 是这样的：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <title>Modern CSS</title>
        <link rel="stylesheet" href="index.css" />
    </head>
    <body>
        <header>This is the header.</header>
        <main>
            <h1>This is the main content.</h1>
            <p>...</p>
        </main>
        <nav>
            <h4>This is the navigation section.</h4>
            <p>...</p>
        </nav>
        <aside>
            <h4>This is an aside section.</h4>
            <p>...</p>
        </aside>
        <footer>This is the footer.</footer>
    </body>
</html>
```

目前我们的 HTML 中没有使用 classes 或 ids，只用了 HTML 的 tags。在完全没有 CSS 样式的情况下，这个网页看起来是这样的（[在线演示版](https://codepen.io/peterxjang/pen/oEbERP?editors=1100)）：

![1](/images/modern-css/1.png#center)

一点问题都没有，只是不好看，让我们加入 CSS 首先来改变字体，下面是 index.css：

```css
/* BASIC TYPOGRAPHY                       */
/* from https://github.com/oxalorg/sakura */
html {
    font-size: 62.5%;
    font-family: serif;
}
body {
    font-size: 1.8rem;
    line-height: 1.618;
    max-width: 38em;
    margin: auto;
    color: #4a4a4a;
    background-color: #f9f9f9;
    padding: 13px;
}
@media (max-width: 684px) {
    body {
        font-size: 1.53rem;
    }
}
@media (max-width: 382px) {
    body {
        font-size: 1.35rem;
    }
}
h1,
h2,
h3,
h4,
h5,
h6 {
    line-height: 1.1;
    font-family: Verdana, Geneva, sans-serif;
    font-weight: 700;
    overflow-wrap: break-word;
    word-wrap: break-word;
    -ms-word-break: break-all;
    word-break: break-word;
    -ms-hyphens: auto;
    -moz-hyphens: auto;
    -webkit-hyphens: auto;
    hyphens: auto;
}
h1 {
    font-size: 2.35em;
}
h2 {
    font-size: 2em;
}
h3 {
    font-size: 1.75em;
}
h4 {
    font-size: 1.5em;
}
h5 {
    font-size: 1.25em;
}
h6 {
    font-size: 1em;
}
```

上面的这段 CSS 主要是改变字体样式（字体的大小，行高，等等），还有文本颜色和居中显示。当然你得学习 CSS 才知道如何给这些 CSS 属性取适当的值（上面的样式来自[sakura.css](https://github.com/oxalorg/sakura)）。好在上面的 CSS 不至于复杂到读不懂。加入上面的 CSS 后，页面看起来变成这样（[在线演示版](https://codepen.io/peterxjang/pen/jZWzEV?editors=1100)）：

![2](/images/modern-css/2.png#center)

看起来不一样吧 - 这就是 CSS 的作用，简单地把样式加入到一个文件里，不需要编程，也不需要复杂的逻辑。不幸的是，当我们想控制的不只是字体和颜色的样式时，事情开始会变得焦头烂额。

## 使用 CSS 实现页面布局

在 90 年代，在 CSS 被广泛采用前，没有什么可选择的办法来控制布局。HTML 最初设计是用于纯文本的，没有动态网页，没有侧边栏，等等。在早期，页面的布局依靠表格来实现 - 整个网页就是一个大 HTML 表格，其中的内容编排在表格的不同行和列中（注：通过 HTML 表格的行、列可以让文本对齐）。这个方法可行，但网页的内容和样式是紧耦合的（在一个 HTML 文件里），如果想改变一个网站的网页布局格式，需要改写大部分 HTML 代码。

一旦 CSS 登场，网页的内容（写在 HTML 里）和样式（写在 CSS 里）就很有必要分开了。大家会想办法把所有和样式相关的代码移入 CSS 里（不再有 HTML 表格）。但非常需要注意的是，CSS 也非真的设计用来控制页面布局的，所以早期这种解耦的尝试都没有找到什么好办法。

让我们用上面的例子看看 CSS 实际上如何解耦内容和布局。我们首先要重置边距（`margin`）和补白（`paddings`）- 它们会影响到布局设计时需要做的计算。同时给网页上不同的区块（section）设置不同的颜色 - 不需要好看，主要是测试不同的布局时，视觉上能分辨出不同的区块（这样就可以看出布局的效果）。

```css
/* RESET LAYOUT AND ADD COLORS */
body {
    margin: 0;
    padding: 0;
    max-width: inherit;
    background: #fff;
    color: #4a4a4a;
}
header,
footer {
    font-size: large;
    text-align: center;
    padding: 0.3em 0;
    background-color: #4a4a4a;
    color: #f9f9f9;
}
nav {
    background: #eee;
}
main {
    background: #f9f9f9;
}
aside {
    background: #eee;
}
```

现在这个网页看起来像这样（[在线演示版](https://codepen.io/peterxjang/pen/vdLROM?editors=1100)）：

![3](/images/modern-css/3.png#center)

好，我们现在可以开始使用 CSS 控制网页内容的布局了。我们将按出现的时间顺序介绍三种不同的布局方法，从经典的 `float` 布局开始。

### float 布局

CSS float 属性最早引入是让图片定位在文本栏的左边或者右边（就像你看到的新闻报纸）。Web 开发者发现并利用了一个事实 - 可定位的其实不只是图片，而是任意 HTML 元素 - 通过定位整个 div 元素里的内容，可以把网页纵横分割成不同的部分。因为这种纵横分割是通过想象的，随意的，所以很难保持准确一致的分割。

在 2006 年，A List Apart 上发表了一篇文章 [In Search of the Holy Grail](https://alistapart.com/article/holygrail)（寻找圣杯），该文章介绍了一种详尽的，所谓圣杯的布局方法（注：圣杯指的就是至高无上，完美） - 这就是经典的三栏式布局，布局包括三部分 - 头部（header），三栏主体部（3 columns），和注脚部（footer）。这么简单的布局居然被冠以“圣杯”，真是难以置信，可想而知在当时的技术条件下，想要通过 CSS 产生一致性的布局难度确实是很大的。

下面展示的就是把这篇文章所描述的 float 布局应用到前面的例子上：

```css
/* FLOAT-BASED LAYOUT */
body {
    padding-left: 200px;
    padding-right: 190px;
    min-width: 240px;
}
header,
footer {
    margin-left: -200px;
    margin-right: -190px;
}
main,
nav,
aside {
    position: relative;
    float: left;
}
main {
    padding: 0 20px;
    width: 100%;
}
nav {
    width: 180px;
    padding: 0 10px;
    right: 240px;
    margin-left: -100%;
}
aside {
    width: 130px;
    padding: 0 10px;
    margin-right: -100%;
}
footer {
    clear: both;
}
* html nav {
    left: 150px;
}
```

看上面的 CSS 代码，你会发现不少 CSS“黑”技巧（margin 是负值，clear：both，宽度 width 是硬算出来的，等等，A List Apart 的有篇文章很好的解释了这些“黑”技巧的细节和原因）。下面是效果（[在线演示版](https://codepen.io/peterxjang/pen/VQeXYg?editors=1100)）：

![4](/images/modern-css/4.png#center)

看起来还不错，但你会注意到 3 栏的高度是不一致的，而且页面也没有和屏幕等高。这个问题来源至 float - float 只能定位到左边或右边，但无法得知被定位区域内容的高度。这个问题好多年都没有比较直接的解决办法，直到好多年后 `flexbox` 布局的出现。

### flexbox 布局

CSS flexbox 属性的第一次出现是在 2009 年，但被广泛采纳则到了 2015 年。flexbox 被用来定义那些必须横跨不同行或列的区域，从这点上要比 float 更适合于页面布局。对于开发人员讲，在用了近 10 年的 float 布局后，在页面布局上终于可以不再需要借助于各种 float 的“黑”技巧。

下面就是我们上述例子的 flexbox 布局，采纳了 [Solved by Flexbox](https://philipwalton.github.io/solved-by-flexbox/demos/holy-grail/) 所描述的技巧（该网站专门展示各种 flexbox 布局的例子，很受欢迎）。注意到我们增加了一个 div 容器，把之前的三栏包含其内：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <title>Modern CSS</title>
        <link rel="stylesheet" href="index.css" />
    </head>
    <body>
        <header>This is the header.</header>
        <div class="container">
            <main>
                <h1>This is the main content.</h1>
                <p>...</p>
            </main>
            <nav>
                <h4>This is the navigation section.</h4>
                <p>...</p>
            </nav>
            <aside>
                <h4>This is an aside section.</h4>
                <p>...</p>
            </aside>
        </div>
        <footer>This is the footer.</footer>
    </body>
</html>
```

下面是 flexbox 布局的 CSS 代码：

```css
/* FLEXBOX-BASED LAYOUT */
body {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
}
.container {
    display: flex;
    flex: 1;
}
main {
    flex: 1;
    padding: 0 20px;
}
nav {
    flex: 0 0 180px;
    padding: 0 10px;
    order: -1;
}
aside {
    flex: 0 0 130px;
    padding: 0 10px;
}
```

这下比 float 布局紧凑多了。flexbox 的属性和值初看有点让人困惑，但我们不用像 float 布局那样需要搞出一些“负”边距的事情 - 这是巨大的改进，下面是效果（[在线演示版](https://codepen.io/peterxjang/pen/xYZWGz?editors=1100)）：

![5](/images/modern-css/5.png#center)

三栏都齐高而且充满了整个页面，看起来挺完美了，不过该布局方法有一些小缺陷。一个是浏览器支持，目前只有较新的浏览器支持 flexbox，旧的则不行。当然，浏览器厂商正在更尽力地停止对旧浏览器的支持，让网页设计者们有比较一致的开发体验。另一个是我们得加入这段 <span class="kwd">&lt;div class="container"&gt;</span>，最好的办法应该 - 理想情况下，任何的 CSS 布局不应该改变任何原有 HTML 文本。

所以最大的问题还是 CSS 代码本身 - flexbox 避免了 float 的“黑”技巧，但是 CSS 变得没有那么直观 - 没有看上去像是用于定义布局的。flexbox 代码不易阅读，也无法让开发者对于各个元素在页面上的布局状况产生视觉上的直观感。这样导致在编写 flexbox 布局代码时，编写人员得不断地臆想和测试。

非常重要的一点是，flexbox 是本身设计用来针对一个行或列内的页面元素所占位置的大小进行调节，而非整个页面的布局！虽然它可以为布局工作而且也比 float 好，但是我们终于有了针对整个页面 - 多行多列布局而发展出来的 CSS 规范，这个规范就是 <span class="kwd">CSS grid</span> ！

### grid 布局

CSS grid 规范第一次提出是在 2011 年（其实也就在 flexbox 规范提出后不久），但是却花了超长时间获得不同浏览器的支持。直到 2018 年初，CSS grid 才在大多数较新的浏览器上得到支持（其实要和过去一、两年比，这是个巨大的进步）。

下面是 grid 布局，采用了这篇文章（[CSS tricks](https://css-tricks.com/css-grid-one-layout-multiple-ways/)）中介绍的第一种方法。注意在同样的例子里，我们去掉了 <span class="kwd">&lt;div class="container"&gt;</span>（那是为了 flexbox 布局而加入的），HTML 是原装不动的，下面是 CSS：

```css
/* GRID-BASED LAYOUT */
body {
    display: grid;
    min-height: 100vh;
    grid-template-columns: 200px 1fr 150px;
    grid-template-rows: min-content 1fr min-content;
}
header {
    grid-row: 1;
    grid-column: 1 / 4;
}
nav {
    grid-row: 2;
    grid-column: 1 / 2;
    padding: 0 10px;
}
main {
    grid-row: 2;
    grid-column: 2 / 3;
    padding: 0 20px;
}
aside {
    grid-row: 2;
    grid-column: 3 / 4;
    padding: 0 10px;
}
footer {
    grid-row: 3;
    grid-column: 1 / 4;
}
```

从视觉效果看，和 flexobox 布局一样。但是，相关的 CSS 代码易读而且清楚的表明了所需要达到的页面布局效果。行、列的总体大小和形状定义在 body 这个选择器里，而各个 grid 中的东西则分别定义。

有一个比较令人困惑的是 <span class="kwd">{% label primary@grid-column %}</span> 属性，它定义了开始/结束栏。令人困惑是因为这个例子里，总共 3 栏，但数字却是从 1 到 4，看了下面这张图就比较容易理解了（[在线演示版](https://codepen.io/peterxjang/pen/vdLROM?editors=1100)）：

![6](/images/modern-css/6.png#center)

第一栏从 1 开始到 2 结束，第二栏是 2 到 3，第三栏是 3 到 4。而头部（header）是 1 到 4，横跨整个页面，nav 只是 1 到 2，横跨第一栏，等等。

一旦你习惯于 grid 的语法，它很显然就是你想表达页面布局的最佳方法。唯一的困难是浏览器支持，不过如上所述，近年来已有巨大进步。说 CSS grid 是第一个真正为页面布局设计的 CSS 工具一点都不为过。以往网页设计者对于创造性的网页布局设计却不得不保守（想得到却不能做），因为之前种种 CSS 工具非常脆弱，而且还需要各种“黑”技巧以及打补丁的做法。现在因为 CSS grid 到来，以前从未可能实现过的、创造性的网页布局设计，很有可能会来上一大波，这实在让人兴奋。

![7](/images/modern-css/7.png#center)

## 使用 CSS 预处理器

到此我们讨论了用 CSS 来实现简单样式以及布局。现在我们进入工具讨论，那些帮助提高 CSS 开发体验的工具（视 CSS 为计算机的一种语言），从 CSS 预处理器（css preprocessor）开始。

CSS 预处理器让你使用另一种语言，但会将之转化为 CSS，这样浏览器才能理解。 这个非常重要，因为早期浏览器在支持新的特性方面脚步都非常缓慢。第一个重要的 CSS 预处理器是 [Sass](http://sass-lang.com/)，在 2006 年发布。它的优点是更简洁的语法（用缩进取代括号，取消分号，等），并加入了一些 CSS 没有的语法特性，例如变量，helper functions，calculations。下面是上面的例子中颜色的部分用 Sass 的变量来实现：

```css
$dark-color: #4a4a4a
$light-color: #f9f9f9
$side-color: #eee
body
  color: $dark-color

header, footer
  background-color: $dark-color
  color: $light-color

main
  background: $light-color
nav, aside
  background: $side-color
```

变量可以重用并且是用\$符来定义的，括号和分号没有了，看起来更干净。除了干净的语法，Sass 对 CSS 引入变量等特性在当时是具有突破性的，因为它开启了编写更简洁和更易维护的 CSS 代码的可能性。

要使用 Sass，你必须先安装 [Ruby](https://www.ruby-lang.org/en/documentation/installation/)，该编程语言用来把 Sass 代码编译成正常的 CSS 代码。然后你还得安装 [Sass gem](http://sass-lang.com/install)，然后运行命令行把你的.sass 文件变成.css 文件：

```bash
sass --watch index.sass index.css
```

上面的命令把 index.sass 里的 Sass 代码转成 index.css 里正常 CSS 代码，--watch 的作用是当有任何代码改变时（文件会发生变化），自动重跑该命令，这在使用中非常方便。

上面这个处理过程被称为一个构建步骤，对于早在 2006 年的前端开发人员，想上手是一个不小的障碍。如果你熟悉像 Ruby 这样的编程语言，这个处理当然是很显然的。但是对于当时大多数前端开发人员，因为只习惯使用 HTML 和 CSS，没有使用这种的工具的需要。而要想获得 CSS 预处理器的新特性，则必须学习相关工具生态链，所以这对于任何想上手的开发人员，必须有较大的动力才行。

在 2009 年，[Less](http://lesscss.org/) 预处理器发布了。它也是 Ruby 编写的，和 Sass 也很类似。主要区别在语法上，它更接近于 CSS 语法。任何 CSS 代码都是合法的 Less 代码，下面是 Less 代码（同样的例子）：

```css
@dark-color: #4a4a4a;
@light-color: #f9f9f9;
@side-color: #eee;
body {
    color: @dark-color;
}

header,
footer {
    background-color: @dark-color;
    color: @light-color;
}

main {
    background: @light-color;
}
nav,
aside {
    background: @side-color;
}
```

和 Sass 的例子差不多（用 `@` 符而非 `$` 符定义变量），但没有 Sass 代码漂亮，因为保留了 CSS 的括号和分号。当然因为更接近 CSS 所以更容易让开发人员采纳它。在 2012 年，Less 用 Javascript 重写（取代了原来的 Ruby），这让 Less 的编译更快，同时更吸引开发人员，特别是那些已经采纳、使用 [Node.js](https://nodejs.org/en/) 的。

要把上面的代码转成正常的 CSS 代码，你首先要 [安装 Node.js](https://nodejs.org/en/download/)，然后 [安装 Less](http://lesscss.org/#using-less-installation)，接下来跑命令：

```bash
lessc index.less index.css
```

类似的，上面的命令把 index.less 里的代码转成 index.css 里正常 CSS 代码。注意的是，不像 sass 命令，Less 没有-watch 选项，意味着你必须安装和使用额外的工具来取得同样的效果。这对于那些习惯命令行的编程人员不是什么难事，但对于那些只想使用 CSS 预处理器的开发人员上手又增加了一道实实在在的门坎。

当 Less 获得大家的认可时，Sass 的开发人员也做出了调整，在 2010 年增加了一种新的语法 - [SCSS](http://sass-lang.com/documentation/file.SASS_CHANGELOG.html#SCSS__Sassy_CSS_)（和 Less 类似，是 CSS 的超集）。他们还发布了新工具 [LibSass](http://sass-lang.com/libsass)，用 C/C++实现 Sass Ruby 编译引擎，这让它更快并且在其它编程语言里也能够使用。

另一个 CSS 预处理工具是 [Stylus](http://stylus-lang.com/)，在 2010 年出现，用 Node.js 开发，和 Sass、Less 比较关注在更简洁的语法上。基本上 CSS 预处理的讨论不外乎这三个使用最广泛的工具。它们所提供的特性也很类似，最终你任选一个都不会错。

当然，有人会对 CSS 预处理的必要性提出质疑，因为浏览器正在而且最终会实现它们的特性（如变量，calculations）。更进一步，有一种不同的处理方法 - CSS 后处理让 CSS 预处理变得不再需要（当然这个观点不无争议）。接下来我们就谈谈后处理。

## 使用 CSS 后处理器

CSS 后处理器是使用 Javascript 分析并把“你的 CSS”转换成真正的 CSS 代码。从这点看，和 CSS 预处理非常像，你可以想象成用不同的方法处理同样的问题。 它们的区别是，CSS 预处理器用特别的语法来标记哪些代码需要转换，而 CSS 后处理器则是把正常的 CSS 代码转换成任意代码而无需引入特别的语法。这个最好就是通过例子来展示。 让我们看看上面例子中 header 部分原来的 CSS 代码：

```css
h1,
h2,
h3,
h4,
h5,
h6 {
    -ms-hyphens: auto;
    -moz-hyphens: auto;
    -webkit-hyphens: auto;
    hyphens: auto;
}
```

<span class="kwd">-</span> 开头的部分称为“浏览器开发商前缀”（vendor prefixes）。它的作用是让浏览器识别出这些是实验性的新增特性，开发人员可以采用之，虽然它们还没有彻底被浏览器实现。这里 <span class="kwd">-ms</span> 前缀是给微软 IE 的，<span class="kwd">-moz</span> 是给 Mozilla 火狐浏览器的，<span class="kwd">-webkit</span> 是给采用 webkit 渲染引擎的（谷歌的 Chome，Safari，新版的 Opera 浏览器都采纳之）。

为了使用这些 CSS 新增特性，要记住这些不同的 vendor prefixes，这是挺烦人的。当需要的时候，如果能有一个工具自动把 vendor prefixes 加入就太好了。当然我们可以通过 CSS 预处理，例如像下面这样（SCSS）：

```scss
@mixin hyphens($value) {
    -ms-hyphens: $value;
    -moz-hyphens: $value;
    -webkit-hyphens: $value;
    hyphens: $value;
}

h1,
h2,
h3,
h4,
h5,
h6 {
    @include hyphens(auto);
}
```

这里使用了 Sass 的 <span class="kwd">@mixins</span> 特性，它让你定义一段 CSS，然后在其它地方重用。当这个文件编译成正常的 CSS 代码时，任何@include 的语句将会被匹配的 @mixin 所定义的 CSS 代码取代。总的讲，这方法不算坏，但你需要为每一个需要 vendor prefixes 的 CSS 特性定义一个 mixins。这些 mixin 代码则需要维护，因为当浏览器支持这些特性时，你就需要移除相应的 vendor prefixes。

对比 mixins，还有一更好的方法是只写基本的 CSS，然后通过工具识别出那些需要加入 vendor prefixes 的 CSS 特性，并自动加入。CSS 后处理器就是可以做这个的。举个例子，如果你使用 [PostCSS](http://postcss.org/) 和其 [autoprefixer plugin](https://github.com/postcss/autoprefixer)，你就可以只写不带 vendor prefixes 的 CSS，然后让 PostCSS 做剩下的工作：

```css
h1,
h2,
h3,
h4,
h5,
h6 {
    hyphens: auto;
}
```

当你运行 CSS 后处理器时，hyphens 将会被适当的 vendor prefixes 取代（由 autoprefixer plugin 决定，无需你操心）。这意味着你可以编写正常的 CSS 代码，无需为浏览器 CSS 兼容或者特殊的 CSS 语法担心。

除了 autoprefixer 外，PostCSS 还有其它 plugins 让你做一些蛮酷的事情。[cssnext](http://cssnext.io/) 让你使用还处于试验性的 CSS 特性；[CSS modules](https://github.com/css-modules/css-modules) 能够自动改变 classes 以免 class 命名冲突；[stylelint](https://stylelint.io/) 能够识别 CSS 代码错误并让 CSS 代码保持风格一致。这些工具都是在过去一两年内出现的，新的开发流程融入了这些新的工具，展示出很多在过去完全无法做到的事情。

当然这有个代价，安装和使用像 PostCSS 这样的后处理器，和 CSS 预处理器相比，要花更多的功夫。不只是通过命令行安装和使用它们，还有安装和配置相关的插件（plugins），定义复杂的规则（例如目标针对哪种浏览器）。不停留于简单地运行 PostCSS，很多开发人员还把后处理器集成到了前端构建体系里，如 [Grunt](https://github.com/postcss/postcss#runners)，[Gulp](https://github.com/postcss/postcss#gulp)，和 [Webpack](https://github.com/postcss/postcss#webpack)，这些构建集成工具帮忙前端开发人员管理各种构建工具以及构建流程。

> 如果你从未使用过任何一种前端构建工具，学习所有必要的构建流程是件很累人的事情。如果你刚开始，请查看我的文章 <i class="fas fa-external-link-alt"></i> [给恐龙们解释一下当今的 Javascipt - Modern Javascript（译文）](/posts/modern-js-dinosaurs/)，那篇文章从前端开发人员角度出发介绍了 Javascript 工具的现今状况。

值得指出的是关于 CSS 后处理器是有些争议的。有些人抱怨术语太令人费解 - 有些说它们应该都叫 CSS 预处理器，有些则主张简单点就叫 CSS 处理器，等等。有些人认为，CSS 后处理器可以完全取代预处理器，有些则认为两者应该共处。不过有一点是清楚的，如果你有兴趣研究到底能把 CSS 发挥到什么程度，你应该学习一把如何使用 CSS 后处理器。

![8](/images/modern-css/8.png#center)

## 编写可维护 CSS 的方法

各种 CSS 预处理和后处理工具为改进 CSS 开发体验走过了漫长的道路。但这些工具并”不“足已解决维护庞大 CSS 代码的问题。为此，大家开始记录各种指导 CSS 编写的规范和守则，通常称之为 CSS 编写的指导方法。

在讨论任何一个特定的 CSS 编写指导方法之前，很重要的是要搞明白为什么随着时间的推移，CSS 代码变得难以维护。一个关键的 CSS 特性是 CSS 是全局性的 - 任何一个你所定义的样式将全局性地应用在页面上的任何一个部分。所以你需要做的工作是，你得遵循一套详细的命名规范来维护各个样式类（class）的独立性，或者依赖于特定的判定规则来决定对给定页面元素采用哪个样式。CSS 编写指导方法论就是提供这么一套有组织的方式来编写 CSS，让你避免编写庞大 CSS 代码时所遇到的痛点。让我们按照大致的时间顺序介绍一些非常出名的 CSS 编写指导方法。

### OOCSS

[OOCSS：面向对象的 CSS](https://github.com/stubbornella/oocss/wiki)（Object Oriented CSS ）第一次出现在 2009 年，这个方法基于两个原则。第一个是<span class="uline">结构（structure）和皮肤（skin）样式必须分离</span>。此原则意味着定义结构（如布局）的 CSS 代码不应该和定义皮肤的（如颜色，字体，等）混在一起。 这样更容易进行“换肤”。第二个原则是<span class="uline">容器和内容必须分离</span>。此原则意味着要把页面元素当作可重用的对象 - 对象意味着它无论在页面的任何位置上看起来都是一样的。

OOCSS 提供了深思熟虑的原则，但在实行上并没有太具体的规定，后来的 SMACSS 继承了它的核心概念，但加入了很多细节让其在实践中更容易上手。

### SMACSS

[SMACSS：模块化并可扩展的 CSS](https://smacss.com/)（Scalable and Modular Architecture for CSS）第一次出现在 2011 年，这个方法让你基于五个分类规则编写 CSS - 基本规则（base rules），布局规则（layout rules），模块规则（modules），状态规则（ state rules）， 以及主题规则（theme rules）。SMACSS 也推荐一定的命名规范，如对于布局规则，布局相关样式类，名字一律加前缀 <span class="kwd">l-</span> 或者 <span class="kwd">layout-</span> ；对于状态规则，样式类名字则加入相关状态描述前缀，例如 is-hidden 或者 is-collapsed。

和 OOCSS 相比，SMACSS 虽然有着更具体详细的规定，但是要求使用者想清楚采纳哪种具体的 CSS 规则，放入哪种分类当中。后来的方法如 BEM 让 CSS 编写者无需为其中的一些事情考虑，这使得它们的上手更容易些。

### BEM

[BEM](https://en.bem.info/)（Block，Element，Modifier）第一次出现是在 2010 年，基本思想是把用户界面分成独立的几大块。<span class="kwd">块（block）</span> 指的是一种可重用东西，例如搜索表单 - 被定义为 &lt;form class="search-form"&gt; &lt;/form&gt;。<span class="kwd">元素（element）</span> 指的是块里的一部分，本身不能单独拿出来重用，例如搜索表单里的按钮 - 被定义为 &lt;button class="search-form\_\_button"&gt;Search&lt;/button&gt;。<span class="kwd">修改者（modifier）</span> 指的是那些可以改变块或者元素的东西，这些改变包括块或元素的样式，状态，以及相关允许的动作，例如一个搜索表单禁用按钮 - 被定义为 &lt;button class="search-form\_\_button search-form\_\_button--disabled"&gt;Search&lt;/button&gt;。

BEM 比较容易理解，有着一套命名规范让新手使用而无需做太多复杂的考虑。但不好的另一面是，有些样式类名超长，和传统所提倡的 CSS 命名有冲突 - 传统上提倡 CSS 的样式类名对人来讲最好有意义。下面看看 Atomic CSS 如何把这种非传统做法带到另一个层次。

### Atomic CSS

[Atomic CSS](https://www.smashingmagazine.com/2013/10/challenging-css-best-practices-atomic-approach/)（也被称为 Functional CSS）第一次出现是在 2014 年，其方法围绕这一个思想：每个类（class）都是小型，单一目的的，而且类名就是基于视觉函数。这个方法和前面的 OOCSS，SMACSS，BEM 完全的不同，不是把页面元素当作可重用的对象，Atomic CSS 完全忘掉了这些对象而通过可重用的、单一目的的工具函数类来对页面元素做样式处理，像前面的 &lt;form class="search-form"&gt; &lt;/form&gt; 变成了&lt;button class="f6 br3 ph3 pv2 white bg-purple hover-bg-light-purple"&gt;Search&lt;/button&gt;。

如果上面的例子让你惊恐地萎缩了一下，那么是正常的，很多人都认为这个和已有的 CSS 最佳实践相违背的方法。然而，网上对此有着大量精彩的讨论 - 质疑所谓的 CSS 最佳实践是否在不同的情形下都成立。[这篇文章](https://adamwathan.me/css-utility-classes-and-separation-of-concerns/)对此做了很好的功课并强调指出传统的关注点分离导致了 CSS 最终依赖于 HTML（即使采用了象 BEM 这样的方法）。而象 Atomic 这样采用函数型的方法则会让 HTML 依赖于 CSS。两者都没有错，如果你仔细审视之，你会明白让 HTML 和 CSS 两者关注点分离到目前还没有完全实现！

其它方法如下面的 CSS in JS 实际上就是包含了这个概念 - HTML 和 CSS 就总是相互依赖的，当然，这也是至今最具争议的指导方法 ...

### CSS in JS

CSS in JS 第一次出现是在 2014 年，它不是把 CSS 分开放在样式表里，而是直接放在各个组件里。它是由 React Javascript 框架引入的（该框架另一个同样有争议的做法是把 HTML 定义的组件放在 Javascript 文件里而不是 HTML 文件中）。最初 React 是把样式直接内嵌，后来才采用通过 Javascript 产生相关的 CSS（根据组件产生独立的 CSS 类名）然后将之插入到文件里并带上样式标签。

CSS in JS 的做法和关注点分离的 CSS 最佳实践完全违背。这里的原因是，我们使用 web 的方法随着时间的推移已经完全不同了。早期的 web 主要是大量的静态网站 - 把 HTML 的内容从 CSS 样式中分离出来是有道理的。现阶段则变成了动态的 web 应用 - 开发人员应该把东西分成不同的、可重用的组件。

CSS in JS 的目的是想把组件定义成有严格的界限，它自身的 HTML/CSS/JS 都是内含的 - 一个组件的 CSS 代码完全没有可能去影响另一个组件。作为一个被广泛采纳的 Javascript 框架，React 就是要表达并追求这个目的，它影响着并让其它的框架如 Angular，Ember，Vue 也跟随这种做法。必须指出 CSS in JS 的做法还比较新，现阶段广泛地对 web 采用了组件开发方式，开发人员们正努力为此建立新的 CSS 最佳实践，并为此做各种尝试和试验。

现阶段各种不同的 CSS 方法很容易让人招架不住，但重要的是，我们必须认识到没有唯一正确的方法 - 当你的 CSS 代码足够的复杂时，它们只是你可能采用的不同的 CSS 工具或选项。你必须根据自己的工作情况深思熟虑，作出有利的技术选项。从长远讲，在 CSS 这个领域内，现阶段的各种方法，各种尝试都会让每一个开发人员受益。

## 结语

上面就是当今 CSS 的综述。我们介绍了如何用 CSS 来控制简单的样式如字体样式，如何采用 CSS 的 float，flexbox，以及 grid 来布局，如何用 CSS 预处理器来处理变量和 mixins，用 CSS 后处理器支持转换功能如增加 vendor prefixes，以及介绍不同方法来编写可维护的 CSS 以克服 CSS 全局性的问题。 我们还没有深入其它众多的 CSS 功能/特性，如选择器 selector，过渡 transition，动画 animations，形状 shapes，动态变量 dynamic variable，这些功能/特性可以一直列下去。关于 CSS 有太多的东西可讲，任何人如果想说 CSS 是容易的，恐怕他还不了解其中的一半。

当今的 CSS 技术还在快速地发展着，要想在工作中运用好该技术肯定是件让人很受挫的事。重要的是记住 web 在不同的历史阶段如何演变以及当时的技术状况，从中就知道有不少聪明的技术人员创建了实实在在的 CSS 工具和编写方法让 CSS 的最佳实践随着 web 技术的发展而发展。在现阶段做个开发者是个无比兴奋的事，我希望这篇文章能在你成神的路上给予你指导性的帮助！
