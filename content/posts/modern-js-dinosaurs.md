+++
title = "给恐龙们解释一下当今的Javascript - Modern Javascript（译文）"
date = 2017-11-01T19:04:01+08:00
categories = ["前端 & 全栈开发"]
tags = ["javascript"]
toc = true
draft = false
+++

现代 Javascript 的进化史 - 编程语言之争犹如宗教战争，但 IT 史上，发展最快，生长最野蛮，也最无语的编程语言，毫无争议的当属 Javscript。

<!--more-->

💬 恐龙 ，远古时代的动物，指的应该是像我这样，因为 Internet 的出现，20 年前就知道，接触过 Javascript，但还好没有"入坑"但快要“入土”的程序员。

---

原文：[<i class="fas fa-external-link-alt"></i>&nbsp; Modern JavaScript Explained For Dinosaurs](https://medium.com/@peterxjang/modern-javascript-explained-for-dinosaurs-f695e9747b70)

如果你不是从 Javascritp 最开始一路跟下来，学习 Javascript 是件多么苦难的事你肯定不清楚。Javascript 的生态发展和变化之快，之大，让人搞不懂这些五花八门的工具到底想干啥，想解决什么莫名其妙的问题。我自己从 1998 年开始学习编程但直到 2014 年才开始学习 Javascript。在那时，我记得我看到 Browserfy 上有这么一段：

> "Browserfy lets you require('module') in the browser by bundling up all of your depednecies."

我完全看不懂这是啥意思，并试图明白它对 Javascript 开发者有什么帮助。

本篇博客从提供一个历史的上下文去看 Javascript 工具是如何演变成了今天（2017 年）的样子。我们从构建一个简单的网页例子开始，像恐龙级程序猿那样，不用任何工具，只有 HTML 和 Javascript。从历史上下文着手，能让你更好的学习和适应还在不断变化和发展的 Javascript，让我们开始吧。

## 最原始的方法

让我们用原始的方法 - 手工方式指定文件的下载和连接。下面就是一个简单的 <span class="kwd">ntex.html</span> 文件，里面包含 Javascript 代码文件的连接：

index.js 就是和 index.html 在同一网站服务器目录下的另一个文件，内容如下：

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <title>JavaScript Example</title>
        <script src="index.js"></script>
    </head>
    <body>
        <h1>Hello from HTML!</h1>
    </body>
</html>
```

这行 &lt;script src="index.js"&gt;&lt;/script&gt; 我们让浏览器载入了同一目录下的 <span class="kwd">index.js</span> ：

```javascript
// index.js
console.log("Hello from JavaScript!");
```

这两个文件就构成了你所需要的一个简单网页。现在我们加入一个简单的第三方 Javascript 库（不是我们自己开发的）<span class="kwd">moment.js</span>，这个库可以帮我们格式化日期，举个例子，我们可以调用 moment 这个方法：

```javascript
moment().startOf("day").fromNow(); // 20 hours ago
```

调用前提是我们必须把 moment.js 搞到我们的网站上来，从 moment.js 的[网页](http://momentjs.com/)上可以看到下面的信息：

![0](/images/modern-js/0.png#center)

从安装的方法（右上部分）看到安装 moment.js 包含好几个动作，让我们先忽略之，我们直接下载 moment.js ，放到我们的网站服务器上，并在 index.html 里包含它：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <title>Example</title>
        <link rel="stylesheet" href="index.css" />
        <script src="moment.min.js"></script>
        <script src="index.js"></script>
    </head>
    <body>
        <h1>Hello from HTML!</h1>
    </body>
</html>
```

注意，moment.js 是在 index.js 之前的，所以先被浏览器载入，也意味着我们才可以在 index.js 里调用 moment 方法：

```javascript
// index.js
console.log("Hello from JavaScript!");
console.log(moment().startOf("day").fromNow());
```

哈哈，这就是我们使用各种 Javascript 库的原始方法。好处是它很简单，坏处是，每次我们都要去各个网站上找，然后手工下载这些库，每次库如果升级了，还得重复这个步骤。

## 包管理器的方法：NPM

从 2010 年起，各种 Javascript 包管理器 package manager 开始出现，可以自动下载、升级各种库。在 2013 年，[Bower](https://bower.io/) 包管理器无疑是最流行的包管理器，但到了 2015 年左右，被 [npm](https://www.npmjs.com/) 超越。

> _需要指明的是，[Yarn](https://yarnpkg.com/en/) 作为 npm 替代品，虽然在 2016 年 yarn 从 npm 上抢走了很多的注意力，但 Yarn 内部依旧是 npm。_

npm 原来是作为 [node.js](https://nodejs.org/en/) 的包管理器，node.js 是设计在服务器端运行的 Javascript Runtime，而不是前端。让 npm 变成了在浏览器里运行的各种 Javascript 库的管理器，感觉会有点怪怪的。

> _包管理器的使用通常涉及 Unix/Linux 命令行，以前前端开发人员是不要求掌握的，如果你从未使用过 Unix/Linux 命令行，你可以阅读这个[教程](https://www.learnenough.com/command-line-tutorial)。如何使用命令行，对现代 Javascript 开发人员还是很重要的。_

让我们看看如何用 npm 自动下载 moment.js。如果你已经安装了 node.js，那么 npm 已经安装了（包含在 node.js 里）。然后从命令行进入 index.html 所在目录，敲入：

```bash
$ npm init
```

你会面对几个问题，统统敲 Enter，结束后就会出现一个新文件 package.json。package.json 是 npm 用来保存项目信息的配置文件。默认的 package.json 看起来像这个样子：

```json
{
    "name": "your-project-name",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
    },
    "author": "",
    "license": "ISC"
}
```

现在可以根据 npm 用法来安装 moment.js 这个包，键入下面的命令：

```bash
$ npm install moment --save
```

这个命令会做两件事：下载 momen.js 包里所有的代码到一个叫 <span class="kwd">node_modules</span> 的目录里，第二，更新 package.json，并记录 moment.js 成为一个项目依赖。

这个将来分享项目给其它 Javascript 开发人员时有用，因为不需要分享 node_module 目录，那样太大了，只要分享 package.json 就好，然后 npm install 会根据它来下载和安装所需的 Javascript 包。

现在我们不再需要手工从 moment.js 网站上下载 moment.js，而通过 npm 自动下载和更新。我们会看到 moment.js 其实存到了*node_modules/moment/min* 的目录里，这意味着在 index.html 里我们要加入这样的连接 _src="node_modules/moment/min/moment.min.js_：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <title>JavaScript Example</title>
        <script src="node_modules/moment/min/moment.min.js"></script>
        <script src="index.js"></script>
    </head>
    <body>
        <h1>Hello from HTML!</h1>
    </body>
</html>
```

npm 帮我们解决了下载和更新，但坏处是我们得在 node_modules 目录里找到包的位置并手工加入到 index.html 里，下面我们看看能不能把这后面这一步也自动完成呢。

![1](/images/modern-js/1.png#center)

## Webpack：模块捆绑器

大部分的编程语言提供 import 机制把代码从一个文件载入到另一文件里。Javascript 最初没有设计这个（没有 module 的概念），原因是 Javascript 只是设计在浏览器里跑的，而浏览器是没有读写本地文件系统的权限的（安全原因）。所以在很长时间里，如果你把 Javascript 代码分散在几个文件里，只能通过全局变量载入这些文件。这正是上面的例子所展示的，整个 moment.min.js 被载入到 HTML 里，并定义给全局变量 moment，这个变量可以被之后载入任何 Javascript 文件所调用。

在 2009 年，开启了一个名为 <span class="kwd">CommonJS</span> 的项目，为了在浏览器之外实现这个目标。CommonJS 很大一部分是对 Javascript 的模块 module 给出规范，以便 Javascript 最终可以像其它编程语言那样可以在不同的文件之间 import 或 export，而非通过全局变量。而采用 CommonJS 最著名的就是 node.js ：

![2](/images/modern-js/2.png#center)

如前所说 node.js 其实是个 Javascript 服务器，下面是早期在 node.js 里如何使用模块的例子，和上面的区别是在 Javascript 代码里直接载入：

```javascript
// index.js
var moment = require("moment");
console.log("Hello from JavaScript!");
console.log(moment().startOf("day").fromNow());
```

这是没有问题的，因为 node.js 是服务器端的程序，可以直接访问文件系统。node.js 也知道 npm module 的路径，所以不需要这样载入 _require('./node_modules/moment/min/moment.min.js)_ 而只需要简单的 _require('moment')_ 即可。

但如果试着在浏览器端这样做，会得到错误信息（require is not defined）。这就是需要模块捆绑器 module bundler 这种工具的原因。模块捆绑器通过一个构建动作产生最终和浏览器兼容的 Javascript 来解决这个问题。这个例子里，我们可以让模块捆绑器搜索所有的 require 语句（这些语句在浏览器里都是不支持），把它们取代成实际的文件，最终结果变成一个大大的 Javascript 文件（这也是捆绑名字的由来），而且我们也就不再需要这些 require 语句了。

现在让我们来看看如何用 <span class="kwd">Webpack</span> 实现前面这个例子。首先我们安装 Webpack 到这个项目里。Webpack 本身也是 npm 的一个包，所以同样通过 npm 安装：

```bash
$ npm install webpack --save-dev
```

注意 <span class="kwd">--save-dev</span> 这个参数表明 Webpack 是个开发依赖（开发环境依赖的包而非生产环境），这在项目文件 package.json 里可以看到 - 运行上面的命令后 package.json 会被自动更新成：

```json
{
    "name": "modern-javascript-example",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
    },
    "author": "",
    "license": "ISC",
    "dependencies": {
        "moment": "^2.19.1"
    },
    "devDependencies": {
        "webpack": "^3.7.1"
    }
}
```

现在 Webpack 作为一个包也安装在了 node_modules 的目录里，接下来我们可以使用 Webpack 了：

```bash
$ ./node_modules/.bin/webpack index.js bundle.js
```

这个命令将会运行 Webpack，从 index.js 开始，找出所有的 require，把他们相应的代码（从相应文件读取），最后产生一个文件叫 _bundle.js_ 。这也意味着浏览器里我们将不在使用 index.js 而是 bundle.js ，所以 index.html 要做相应的更改：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <title>JavaScript Example</title>
        <script src="bundle.js"></script>
    </head>
    <body>
        <h1>Hello from HTML!</h1>
    </body>
</html>
```

如果在浏览器里访问这个 html，结果就和开始一样。

如果服务器端每次更改 index.js，我们都必须重跑 Webpack，这是挺麻烦的，如果还想使用 Webpack 的高级功能（例如 source maps），将会更麻烦。Webpack 可以自动读取项目目录下的配置选项文件 webpack.config.js ，针对上面的例子，可以是这样：

```json
// webpack.config.js
module.exports = {
  entry: './index.js',
  output: {
    filename: 'bundle.js'
  }
};
```

这样每次跑 Webpack 时，可以简化命令行参数，因为 index.js 和 bundle.js 已经在 webpack.confing.js 里指明了：

```bash
$ ./node_modules/.bin/webpack
```

虽然简化了这个步骤，但每次更改 index.js，还是要重复，接下来我们将进一步优化它。

到现在为止的一切看起来没有做很多事情，但整个开发流程而言顺畅不少，增加一个 Javascript 库，只需增加一条 require 语句而非在 HTML 里增加一条 script，同时捆绑成一个 Javascript 文件，浏览器的载入也比较快。我们现在只在整个开发流程中增加了一个构建步骤，但构建过程里其实我们可以做更多の事，让我们一起继续见证 Javascript 的牛 x 或者说奇葩。

![3](/images/modern-js/3.png#center)

## babel：转译代码

转译代码『Tanspiling』就是把一种编程语言转成类似的另一种语言。这对前端开发非常重要，因为浏览器的更新和标准化需要时间，通常比较慢，而新的前端开发语言可以不断试验新的特性，然后再转译成和浏览器兼容的语言。

> _转译『Tanspiling』好像是在 Javascript 发展中出现的，“恐龙”以前没有听过，只听过编译『Compiling』，区别是转译用于同质语言，编译用于不同质语言，转译是编译的特例。例如把 C“编译”成机器代码，而把 Typescript“转译”为 Javascript。_

对于 CSS，这类新的语言有 Sass ，Less 和 Stylus 等。对于 Javascript，曾经最有名的是 Coffeescript（出现于 2010 年），而现在用得最多的则是 babel 和 Typescript 。Coffeescript 对 Javascript 的改进甚多 - optional parentheses，significant whitespace，等等。 babel 不是新的语言，它是一个转译器，把新版本的 Javascript（ [ES2015](https://babeljs.io/learn-es2015/) 或更高版本）转成老旧版本（ES5），因为不是所有的浏览器都支持新版本的 Javascript。很多人选择 babel 因为它本身和 Javscript 很接近。

_Javascript 语言是个奇葩，其标准一直缺失，比较著名的是 [ECMAscript](https://zh.wikipedia.org/wiki/ECMAScript)_

让我们在上面的例子中加入 babel。首先，安装 babel:

```bash
$ npm install babel-core babel-preset-env babel-loader --save-dev
```

我们为开发环境安装了三个包：<span class="kwd">babel-core</span> 这个是 babel 的主体，<span class="kwd">babel-preset-env</span> 这个用来定义那些新的 Javvascript 特性需要转译，最后 <span class="kwd">babel-loader</span> 则是为了让 babel 和 Webpack 共同工作。 我们需要在 webpack.config.js 中加入 babel-loader：

```json
// webpack.config.js
module.exports = {
  entry: './index.js',
  output: {
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['env']
          }
        }
      }
    ]
  }
};
```

这个语法有点让人困惑（好在我们不需要经常做这件事），基本上它是让 Webpack 找出所有的.js 文件（node_modules 目录之外）然后用 babel-loader 载入之。具体的用法需要学习和参考 Webpack 文档。

好，现在在我们的 Javascript 可以使用 ES2015 的特性，下面的例子是在 index.js 里使用[ES2015 template string](https://babeljs.io/learn-es2015/#ecmascript-2015-features-template-strings)：

```javascript
// index.js
var moment = require("moment");

console.log("Hello from JavaScript!");
console.log(moment().startOf("day").fromNow());

var name = "Bob",
    time = "today";
console.log(`Hello ${name}, how are you ${time}?`);
```

我们也可以使用 [ES2015 import statement](https://babeljs.io/learn-es2015/#ecmascript-2015-features-modules) 而不是 require 来载入模块：

```javascript
// index.js
import moment from "moment";

console.log("Hello from JavaScript!");
console.log(moment().startOf("day").fromNow());

var name = "Bob",
    time = "today";
console.log(`Hello ${name}, how are you ${time}?`);
```

虽然这个例子里 import 和 require 相差不大，但 import 有着更多的特性。改完 index.js 后，我们需要重跑 Webpack:

```bash
$ ./node_modules/.bin/webpack
```

我们可以刷新浏览器页面来看看 babel 是否完成它的工作，当然现在许多新版本的浏览器都支持 ES2015，但你可以用老的浏览器试试例如 IE9 ，或者你在 bundle.js 里看看有没有转译后的代码：

```javascript
// bundle.js
// ...
console.log("Hello " + name + ", how are you " + time + "?");
// ...
```

虽然这个例子没有什么好让人激动的，但代码转译是很厉害的功能。例如新的 Javascritp 特性 [async/await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) 能让你写出更好的代码。尽管转译有时看起来麻烦，但是对 Javascript 的改进在过去几年起了巨大的作用，因为程序员可以现在就使用和测试其未来的语言特性。

我们粗略完成想要做的事，整个流程还是有很多可以改进的地方。如果我们关心性能，我们可以压缩最终的捆绑文件，这个不难，只需要往 Webpack 里加。所以下面我们看看各种方便的工具，用来解决各种各样的问题。

## npm 脚本：task runner

现在我们只针对 Javascript 的模块增加了一个构建步骤，对于复杂情况，则应使用 task runner，它是属于自动化的构建工具，自动化各种构建过程中要完成的任务，对于前端开发而言，这些任务包括了最小化代码，优化图片，运行测试代码，等等。

在 2013 年，Grunt 成为了最流行的前端构建任务自动化工具，后来是 Gulp。两者都依赖不同的 plugins，这些 plugins 的背后其实是不同的命令行工具。现在最流行的做法则是直接利用 npm 包管理器的脚本功能，不需要 plugin 而是通过脚本直接调用这些工具。

让我们来看看用 npm 脚本让 Webpack 的使用更加容易些。这只要修改一下 package.json：

```json
{
    "name": "modern-javascript-example",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "build": "webpack --progress -p",
        "watch": "webpack --progress --watch"
    },
    "author": "",
    "license": "ISC",
    "dependencies": {
        "moment": "^2.19.1"
    },
    "devDependencies": {
        "babel-core": "^6.26.0",
        "babel-loader": "^7.1.2",
        "babel-preset-env": "^1.6.1",
        "webpack": "^3.7.1"
    }
}
```

我们增加了两段脚本 build 和 watch 。执行下面的命令就可运行 build 脚本：

```bash
$ npm run build
```

这将触发 Webpack 运行，并且显示执行的进度 <span class="kwd">--pregress</span>，同时最小化输出 <span class="kwd">-p</span>。

执行下面的命令就可运行 watch 脚本：

```bash
$ npm run watch
```

这将触发 Webpack 自动重新运行如果 Javascript 的文件有所改变 <span class="kwd">--watch</span>，这对开发是很有帮助的。

注意我们不必指明 Webpack 的路径，因为 node.js 知道 npm 模块都在 node_modules 目录下。我们还可以安装 webpack-dev-server 这辅助工具，这是个简易实时重载的 web 服务器：

```bash
$ npm install webpack-dev-server --save-dev
```

package.json 变为：

```json
{
    "name": "modern-javascript-example",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "build": "webpack --progress -p",
        "watch": "webpack --progress --watch",
        "server": "webpack-dev-server --open"
    },
    "author": "",
    "license": "ISC",
    "dependencies": {
        "moment": "^2.19.1"
    },
    "devDependencies": {
        "babel-core": "^6.26.0",
        "babel-loader": "^7.1.2",
        "babel-preset-env": "^1.6.1",
        "webpack": "^3.7.1"
    }
}
```

我们可以启动它：

```bash
$ npm run server
```

它会自动在你浏览器里打开 index.html（localhost:8080）。任何时候如果你改动了 index.js 里的 Javascript，webpack-dev-server 会自动重新构造捆绑 Javascript 文件并且让你的浏览器自动刷新。这不仅节约你的时间，而且让你专注在你的代码，而不是在代码，浏览器之间不停切换以检测所做的修改。

你还可以做更多的事情，你可以在 [这里](https://webpack.js.org/guides/development/) 了解更多的 Webpack 以及 webpack-dev-server。当然你也可以类似的用 npm 脚本把 Sass 转换成 CSS，压缩图像文件，等等，任何命令行可以做的事都可以包含进来。更多 npm 脚本的高级特性可以观看 Kate Hudson 的 [这个讲座](https://www.youtube.com/watch?v=0RYETb9YVrk)。

## 结语

这就是现代 Javascript 开发最基本的部分。我们从纯 HTML 和 Javascript，到使用包管理器自动下载第三方包，通过模块捆绑打包成单一 Javascript 文件，使用转译器来提前使用 Javascript 的新特性，并可以用任务自动化工具来自动化各种构建任务。对初学者来讲，已经经历了不少。对于任何进入编程的新人，前端开发曾经是一个很好的切入点，因为那时前端开发非常容易起步，直接运行，现在则非常吓人，因为有了各种各样复杂的工具，而且还在不断迅速的改变着。

当然现在的情况也没有那么坏，因为随着 node.js 的生态系统被广泛接受，使用 npm 作为包管理器，node require 或 import 语句来调用模块，npm 脚本来运行完成各种任务，即简化了开发流程，又让工具使用保持一致性，这比前一两年有了太大的转变。

无论是新手还是有经验的开发人员，前端开发框架现今多数都提供了相应的工具让其容易上手：Ember 有 ember-cli，然后它影响了 Angular - angular-cli，接着是 React - create-react-app，Vue - vue-cli，等等。这些工具会把一个项目所需的一切准备工作做好，让你可以立马开始写代码。当然工具不是魔法，只是它们使用一致的方法，很多时候，在某个点你需要 Webpack，babel 等做一些特殊的事情，所以了解本文上面所描述的各个环节就很重要了。

现今的 Javascript 开发是比较让人沮丧的，因为其一直以很快的速度变化着。虽然很多时候看起来在重复发明轮子，但 Javascript 的快速演化还是帮助其不断推出创新技术，例如 hot reloading，real-time linting，time-travel debugging。成为程序猿还是一个很令人激动的事情，我希望本文能在你成为开发人员的路途上带来指示性帮助。

![4](/images/modern-js/4.png#center)
