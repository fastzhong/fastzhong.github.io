+++
title = "Python 101 in 2020（简介） "
date = 2020-04-01T01:08:47+08:00
readingTime = true
categories = ["编程"]
tags = ["python"]
toc = true
+++

Python 近两三年伴随着机器学习变得很火，其实 🐍 比 ☕️ 年头还早。

<!--more-->

![Python学习路线 2020](/images/python/python-roadmap-2020.png#center)

## 最人类化的编程语言

Why Python，请看 Python 的编程哲学

```python
>>> import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
美 优于 丑
Explicit is better than implicit.
明确 优于 隐晦
Simple is better than complex.
简单 优于 复杂
Complex is better than complicated.
复杂 也好过 繁复
Flat is better than nested.
扁平 优于 嵌套
Sparse is better than dense.
稀疏 优于 拥挤
Readability counts.
可读性很重要
Special cases aren't special enough to break the rules.
Although practicality beats purity.
固然代码实用与否 比洁癖更重要
我们以为的特例也往往没有特殊到必须打破上述规则的程度
Errors should never pass silently.
不要无故忽视异常
Unless explicitly silenced.
除非刻意地静默
In the face of ambiguity, refuse the temptation to guess.
如果遇到模棱两可的逻辑，请不要自作聪明地瞎猜。
There should be one-- and preferably only one --obvious way to do it.
应该提供一种，且最好只提供一种，一目了然的解决方案
Although that way may not be obvious at first unless you're Dutch.
当然这是没法一蹴而就的，除非你是荷兰人
Now is better than never.
固然，立刻着手 好过 永远不做
Although never is often better than *right* now.
然而，永远不做 也好过 不审慎思考一撸袖子就莽着干
If the implementation is hard to explain, it's a bad idea.
如果你的实现很难解释，它就一定不是个好主意
If the implementation is easy to explain, it may be a good idea.
即使你的实现简单到爆，它也有可能是个好办法
Namespaces are one honking great idea -- let's do more of those!
命名空间大法好，不搞不是地球人
```

Python 在我看来是最“美”的编程语言，易读性极好，很可惜不是我和很多人第一次学习编程的语言，否则学习编程就不用那么费力了 - 因为看 Python 的代码，就像看散文一样，比较一下各种 Hello World：

```C
/* C */
# include <studio.h>
int main() {
        print("Hello, World!\n);
}
```

```Java
/* Java */
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

```Javascript
/* Javascript */
console.log("Hellow, World!");
```

```Ruby
# Ruby
puts "Hello, World!"
```

```Python
# Python
print "Hello, World!"
```

## Python 解释器

编程语言主要有两类，`解释型` 🆚 `编译型`，来自百度百科的解释：

```txt
计算机不能直接理解任何除机器语言以外的语言，所以必须要把程序员所写的程序语言翻译成机器语言，计算机才能执行程序。将其他语言翻译成机器语言的工具，被称为编译器。

编译器翻译的方式有两种：一个是 _编译_，一个是 _解释_。两种方式之间的区别在于翻译时间点的不同。当编译器以解释方式运行的时候，也称之为解释器。

编译型语言写的程序执行之前，需要一个专门的编译过程，把程序编译成为机器语言的文件，比如 exe 文件，以后要运行的话就不用重新翻译了，直接使用编译的结果就行了（exe 文件），因为翻译只做了一次，运行时不需要翻译，所以编译型语言的程序执行效率高。

解释则不同，解释性语言的程序不需要编译，省了道工序，解释性语言在运行程序的时候才翻译，比如解释性 basic 语言，专门有一个解释器能够直接执行 basic 程序，每个语句都是执行的时候才翻译。这样解释性语言每执行一次就要翻译一次，效率比较低。解释是一句一句的翻译。

编译型与解释型，两者各有利弊。编译型由于直接运行目标机器代码，无中间步骤，已针对平台优化，通常速度和效率高；解释型的抽象于平台，跨平台可移植性好。
```

> Python 语言到底是被解释还是被编译的？

Python 的实现方式和 Java 类似，想兼顾两者优势，Python 语言的默认解释器（从官方下载的 Python）是 C 实现的 ⏤ `CPython`（直接叫 Python），CPython 把 Python 程序编译成 bytecode（称为中间代码），然后 bytecode 可运行在 CPython 的虚拟机上。

![Python env](/images/python/CPython.png#center)

Python 除了 CPython，还有多种实现 - [Jython](http://www.jython.org/archive/21/docs/whatis.html)、[IronPython](http://ironpython.net/)、[Cython](http://cython.org/)，[Numba/Anaconda](http://numba.pydata.org/)等，针对不同的虚拟机：

![Python implementations](/images/python/python-implementations.png#center)

不同的解释器是为了方便和其它技术集成，例如默认的 CPython，可以方便地用 C 为 Python 写扩展 [Cython](http://cython.org/)，增加代码执行速度，而 JPython 则可以让 Python 程序里直接调用 JVM 里的 Java Class。

既然有 C、Java、C#的 Python 解释器，还要不得不提一个特殊的解释器 `PyPy`，PyPy 比默认的 CPython 快不少，可以到 5 倍，参考 [http://speed.pypy.org/](http://speed.pypy.org/)，因为引入了个高大上的技术 just-in-time compilation (`JIT`)。bytecode 还是要要翻译成目标机器语言，而 JIT 能加速这个过程，“粗略”地讲：

1. 找出那些需要多次执行的 bytecode；
2. 把它们编译成机器代码；
3. 将它们缓存起来；
4. 再次碰到同样的 bytecode 时，直接调用已经缓存的机器代码；

偷拿个 JVM 上的 JIT 示意图:  
![JVM JIT](/images/python/JIT.png#center)

JIT 在 JVM 上得到了极大应用，号称还快过原生编译型语言的程序如 C/C++。PyPy 采用了同样的技术：

![PyPy JIT](/images/python/PyPy.png#center)

1. PyPy 的解释器是用 RPython 编写的，RPython 类似 Python，相当于 Python 的子集，而且是*强*类型
2. RPython 被编译成了的各种目标机器代码，默认的编译器实现是针对 C 的，但和 CPython 编译器比较，加入了 JIT，除了 C，理论上同样也可以有针对其它平台如 JVM 的实现

还有各种 Python 的解释器，编译器，扩展等，不过 CPython（默认）是最完整的 Python 解释器，即是官方，也是使用的首选。

## Python 版本及各种包

软件开发像搭积木，除了语言本身，更重要的是依赖于各种第三方提供的模块，库，包（各种语言使用的名称不太相同）。做开发的都知道版本和兼容是个通用的大问题，大家都在不同的开发语言上，不停的尝试不同的方法。Python 有两个主流版本 2.x & 3.x 互相不兼容（Python 由于一开始就不是大厂工业化支持，当初属于 Python 之父探索性研究，向后兼容性一直比较不好，快速发展和保证兼容是矛盾的，各有利弊）。Python 3 在 2008 年就出现了，只是近一两才成为首选，但是仍然有众多的包是针对 Python 2 的（这也是 Python 3 这么长时间无法流行起来的原因）。有的时候主版本相同，次版本不同（例如 3.1 & 3.2），细微差别也会导致兼容性问题。除了语言本身，模块和库也有版本兼容性问题，不兼容的模块和库是不能直接拿来用的，想象一下，你机器上同时有两个项目 A 和 B，A 是用 Python 2 开发的，而 B 是用 Python 3 开发的，怎么办？如果 B 依赖于一个第三方的包，版本是 1.0，C 依赖同一个包，但要求版本是 2.0（依赖冲突）。另外还有同一个包可能被依赖多次（层级依赖）。开发人员直接就要面对这个现实而头疼的问题，几十个包不同的版本如何放在同一台机器上，多份拷贝，怎么管理，如何进行升级？嘿嘿，问题不是新的，方法也是不止一种的，这又产生了多一层的混乱：

![Python environment](/images/python/python-environment.png#center)

💡 包管理或者依赖管理是 Python 比较弱的一个方面，这里就不仔细讨论各种 [历史方案](https://packetpushers.net/setting-up-a-python-environment-where-to-develop/) (想了解各种工具的自行 Google)。

## Python setup for Year 2020

这个是很多人忽略，或者困恼初学者的地方。学习一门技术，起手架很重要，你需要上手的正确姿势（最佳实践），原因在于电脑和其它工程技术一样，随着日新月异的发展变得不再是 1+1 那么简单。

下面直接给出答案：

-   <span style="color:orange">pyenv</span>: 管理多个 Python 版本
-   <span style="color:orange">poetry</span>: Python 虚拟环境和包管理器合体（看作 pipenv 的超集，而 pipenv 是 pip 的升级版）

还有一个独辟蹊径的方案是：[Anaconda](https://www.anaconda.com/)

> Anaconda = Python 解释器 + 环境管理器 + 包管理器 + 很多第三方的包（特别是针对大数据和数据分析）

Anaconda 把 Python 连同 n 多的第三方库/包都打包测试好了，一个安装程序就行，属于全家桶，确实省却不少时间，很方便。Anaconda 有自己的包管理器 conda 不是 pip， 具体见 [conda vs. pip vs. virtualenv](http://stuarteberg.github.io/conda-docs/_downloads/conda-pip-virtualenv-translator.html) 以及这个教程 [Why you need Python environments and how to manage them with Conda](https://medium.freecodecamp.org/why-you-need-python-environments-and-how-to-manage-them-with-conda-85f155f4353c)。这个方法有利有弊，好处是一个方法通吃，所需要的东西都有了；坏处是体积较大，fork 一个 virutalenv 不方便。这里不进一步讨论，有兴趣的可自行探索。

### pyenv

pyenv 借鉴了 rbenv，可以用来管理 Python 解释器及其版本。pyenv 不依赖于 Python，通过 [shims](https://en.wikipedia.org/wiki/Shim_computing)，截胡所有的 Python 命令:

> Shim 是一个小程序，可以透明地截取调用并修改调用参数，自己处理该调用或者把调用导向相应位置。

pyenv 的截胡是通过 Path 环境变量：

```bash
$(pyenv root)/shims:/usr/local/bin:/usr/bin:/bin
```

#### 安装 pyenv

```bash
$ curl -L https://raw.githubusercontent.com/pyenv/pyenv-installer/master/bin/pyenv-installer | bash
```

若更新已有的 pyenv

```bash
$ pyenv update
```

Mac 可以通过 [brew](http://brew.sh/) 安装：

```bash
$ brew update
$ brew install pyenv
```

安装后，pyenv 会修改 .bashrc：

```bash
export PATH="/home/xxx/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

没有的话或者其它 shell 如 zsh，自己手动加入。

#### 使用 pyenv

```bash
$ pyenv
pyenv 1.1.4
Usage: pyenv <command> [<args>]

Some useful pyenv commands are:
   commands    List all available pyenv commands
   local       Set or show the local application-specific Python version
   global      Set or show the global Python version
   shell       Set or show the shell-specific Python version
   install     Install a Python version using python-build
   uninstall   Uninstall a specific Python version
   rehash      Rehash pyenv shims (run this after installing executables)
   version     Show the current Python version and its origin
   versions    List all Python versions available to pyenv
   which       Display the full path to an executable
   whence      List all Python versions that contain the given executable

See `pyenv help <command>' for information on a specific command.
For full documentation, see: https://github.com/pyenv/pyenv#readme
```

_列出所有可供安装的版本_：

```bash
$ pyenv install -l
Available versions:
```

_安装某个版本_：

```bash
$ pyenv install 2.7.14
$ pyenv install 3.6.2
```

_列出已安装了的版本_：

```bash
$ pyenv versions
Available versions:
```

_卸载某个版本_：

```bash
$ pyenv uninstall 2.7.14
```

安装完 Python 的各种版本后，pyenv 有四种指明 Python 版本的机制：

-   **shell**：通过环境变量 PYENV_VERSION 指定
-   **local**：由当前或者父目录下的.python-version 文件指定
-   **global**：pyenv root（默认下是~/.pyenv）下面的 version 文件指定
-   **system**：系统安装的 python，不在~/.pyenv 之下，Mac 或 Linux 系统都自带 Python

_显示当前有效的 Python 版本_：

```bash
$ pyenv version
```

_指定当前 shell 使用的 Python 版本_：

```bash
$ pyenv global 2.7.14
$ pyenv shell 3.6.2
```

### poetry

pip & virtualenv 这里只是作为背景知识简单介绍，直接使用 poetry。

#### pip

pip 用来安装、升级和卸载第三方包，Python 已经自带，也是曾经使用最广泛的。

```bash
[I] ➜ pip -h

Usage:
  pip <command> [options]

Commands:
  install                     Install packages.
  download                    Download packages.
  uninstall                   Uninstall packages.
  freeze                      Output installed packages in requirements format.
  list                        List installed packages.
  show                        Show information about installed packages.
  check                       Verify installed packages have compatible dependencies.
  config                      Manage local and global configuration.
  search                      Search PyPI for packages.
  wheel                       Build wheels from your requirements.
  hash                        Compute hashes of package archives.
  completion                  A helper command used for command completion.
  debug                       Show information useful for debugging.
  help                        Show help for commands.
```

#### virtualenv

virrtualenv 将 python 解释器，依赖的包，和每个项目建立一对一关系，这样不同项目就不互相影响了，所以称之为虚拟环境。poetry 将每个项目的虚拟环境存储在 virtualenvs.path 指定的目录之下。

#### poetry

-   poetry 安装

安装后，查看 poetry 的默认设置：

```bash
$ python -m pip install pipx
$ brew install poetry
$ poetry config --list
$ poetry config virtualenvs.in-project true
$ poetry config --list
cache-dir = "/Users/xxx/Library/Caches/pypoetry"
virtualenvs.create = true
virtualenvs.in-project = true
virtualenvs.path = "{cache-dir}/virtualenvs"  # /Users/xxx/Library/Caches/pypoetry/virtualenvs
```

修改 virtualenvs.in-project = false <font color="orange">→ true</font>，这样运行 poetry install 或者 poetry add 时会自动检查虚拟环境，如果没有就自动创建。当然也可以用 poetry env use 命令来人工创建。

-   poetry 使用

    -   类似 node.js 里的 project.yml，PEP 518 引入的新标准 pyproject.toml 文件管理依赖列表和项目的各种 meta 信息，用来替代 Pipfile、requirements.txt、setup.py、setup.cfg、MANIFEST.in 等等各种配置文件（其它工具使用如 pip/pip-tools，Pipenv）。
    -   依赖分为两种，普通依赖（生产环境）和开发依赖。
    -   安装某个包，会在 <span style="color:orange">pyproject.toml</span> 文件中默认使用 upper bound 版本限定，比如 Flask^1.1。这被叫做 Caret requirements，比如某个依赖的版本限定是 ^2.9.0，当你执行 poetry update 的时候，它或许会更新到 2.14.0，但不会更新到 3.0.0；假如固定的版本是 ^0.1.11，它可能会更新到 0.1.19，但不会更新到 0.2.0。总之，在更新依赖的时候不会修改最左边非零的数字号版本，这样的默认设定可以确保你在更新依赖的时候不会更新到具有不兼容变动的版本。另外也支持更多依赖版本限定符号。
    -   不会像 Pipenv 那样随时更新你的锁定依赖版本，锁定依赖存储在 <span style="color:orange">poetry.lock</span> 文件里（这个文件会自动生成）。所以，记得把你的 poetry.lock 文件纳入版本控制。
    -   执行 poetry 或 poetry list 命令查看所有可用的命令。

_快速初始化一个新项目_：

```bash
[I] ➜ poetry new foo
Created package foo in foo

[I] ➜ tree foo
foo
├── README.rst
├── foo
│   └── __init__.py
├── pyproject.toml
└── tests
    ├── __init__.py
    └── test_foo.py

2 directories, 5 files

~/workspace
[I] ➜ cat foo/pyproject.toml
───────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: foo/pyproject.toml
───────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ [tool.poetry]
   2   │ name = "foo"
   3   │ version = "0.1.0"
   4   │ description = ""
   5   │ authors = ["xxx <xxx@gmail.com>"]
   6   │
   7   │ [tool.poetry.dependencies]
   8   │ python = "^3.8"
   9   │
  10   │ [tool.poetry.dev-dependencies]
  11   │ pytest = "^5.2"
  12   │
  13   │ [build-system]
  14   │ requires = ["poetry>=0.12"]
  15   │ build-backend = "poetry.masonry.api"
───────┴───────────────────────────────────────────────────────────────────────────────────────────────────────────

```

_激活虚拟环境_：

如果你想快速在当前目录对应的虚拟环境中执行命令，可以使用 poetry run <你的命令> 命令，比如：

```bash
$ poetry run python app.py
```

如果你想显式的激活虚拟环境，使用 poetry shell 命令：

```bash
$ poetry shell
```

_安装包_：

使用 poetry add 命令来安装一个包：

```bash
$ poetry add flask
```

添加 --dev 参数可以指定为开发依赖：

````bash
$ poetry add pytest --dev
```

_追踪 & 更新包_：

```bash
$ poetry show
````

添加 --tree 选项可以查看依赖关系：

````bash
$ poetry show --tree
```

添加 --outdated 可以查看可以更新的依赖：

```bash
$ poetry show --outdated
```

更新所有锁定版本的依赖：

```bash
$ poetry update
````

如果你想更新某个指定的依赖，传递包名作为参数：

```bash
$ poetry update package
```

_卸载包_：

```bash
$ poetry remove package
```

## 如何学习 Python

![how to program](/images/python/how-to-programming.jpg#center)

Python 是学习编程的首选，没有之一（如果连 Python 都学不会，就不要搞编程了）。 但要如何学习 Python 还真不好回答，即使是大神，下面是大叔的观点：

-   别走”正规“路径，那都是"非正常"学习方法：绝大多数教程都是”反“人类的，特别是所谓的”正规“教育，我们从来不需要上完中文教育课程才会用中文。学习语言是个自然而然的事情，而且我们的语言能力从来不是通过正规教育获得的。用学语法，背字典，做题目的方法来学语言，能做到当然相当厉害，不过世上没有几个能做到，而且学习效率、效果奇差无比，学了几个月死于”无趣“。

-   选择问题入手，而不是语言本身：就像你选择了目标，它会带你去慢慢领略路途的各种风光，而不是迷失在森林里。语言首先是工具，是拿来用的不是学的。Python 很多教程都是拿爬虫为例，为什么呢，因为 Python 是在 Google 开始大规模应用的，爬虫要解决的问题比较多，能解决好非常不简单，这样在解决问题的过程中顺带就把语言学会学深了。

-   由于 Python 属于“所编即所得”，学习过程相当愉快，如果你有其它语言的编程经验，两三个小时，就能了解基本的语法，一到两天，就能上手，以致于你会对 Python 是不是个玩具语言有疑问。我学 Python 头两三年都有如此想法，到后来，才发现那些所谓的 Java，.Net 企业级大型软件好像牛逼轰轰，质量上其实越大越差，Linux 操作系统核心由一人从无到有，Swift 由一人搞定 LVVM，搭起 Swift，贡献 Clang，技术的核心和人或者说人的思想有关。快速通过基础阶段后可以选择某些方向深入，Python 的强项在于能快速编写处理复杂问题的程序，有很多具体的领域值得深入，如股票分析，机器学习，推荐系统，安全应用，自动化运维等等。

-   语言五花八门，就像练武功，搞艺术，同一流派还有各种门户，这有契机和因缘，登顶的路从来不只一条，不喜欢 Python，没有关系，别勉强，勇敢的做选择 - Ruby，C++，Haskell，Elang，等。少林寺的当然说少林武功天下第一，去了少林才知道自己是不是武当的料。

和十几年前不同，现在不仅学习资料都是一丢丢的而且质量非常高，推荐：

-   [Programming with Mosh](https://programmingwithmosh.com/) ㊫ [Python Tutorial - Python for Beginners \[2020\]](https://www.youtube.com/watch?v=kqtD5dpn9C8)
-   [Clever Programmer](https://www.cleverprogrammer.com/) ㊫ [Python Tutorial for Beginners - Full Course in 11 Hours \[2020\]](https://www.youtube.com/watch?v=4F2m91eKmts)
-   [freeCodeCamp](https://www.freecodecamp.org/) ㊫ [Learn Python - Full Course for Beginners \[Tutorial\]](https://www.youtube.com/watch?v=rfscVS0vtbw)
-   [freeCodeCamp](https://www.freecodecamp.org/) ㊫ [Python for Everybody - Full University Python Course](https://www.youtube.com/watch?v=8DvywoWv6fI)
-   [freeCodeCamp](https://www.freecodecamp.org/) ㊫ [Intermediate Python Programming Course](https://www.youtube.com/watch?v=HGOBQPFzWKo)

---

<i class="fas fa-map-marker-alt"></i>&nbsp;&nbsp; 以上都是些我早自学 Python 前想知道的。小结：

-   选择 Python3
-   为每个项目建立 virtual env
-   VS Code 或 PyCharm 作为 IDE
-   学习 Pythonic/PEP8
-   Python 102 in 2021
