+++
title = "5 分钟学会 Markdown"
date = 2018-04-15T19:04:41+08:00
categories = ["工具"]
tags = ["markdown"]
toc = true

+++

Markdown 和 Json 一样，超级简单，却超级好用

<!--more-->

![markdown](/images/md/cheatsheet-md.png)

Markdown 就是一种轻量级的标记语言（Lightweight Markup Language），用人话说就是，普通的文本加上简单的排版或格式化语法（特殊标记），和 HTML，RTF 类似，只是 Markdown 的语法非常简单。Markdown 编写工具很多，在线的也有，很多编程开发工具都支持。需要知道，Markdown 语法没有标准，取决于 Markdown 渲染引擎，各个系统略微有差异。VS Code 和 IntelliJ 都支持 Markdown，喜欢可视化编辑器的话推荐 [Typora](https://typora.io/)。Markdown 文本需要通过转换器变成 HTML 便可以在浏览器里显示，这就是支持 Markdown 的静态博客生成器和 CMS 的原理。

## Markdown 语法

### 大小不同的标题 Heading

用 `#` 来表示标题：

```markdown
# 大小不同的标题 Heading

## 标题 1 Heading 1

### 标题 2 Heading 2

#### 标题 3 Heading 3

##### 标题 4 Heading 4

###### 标题 5 Heading 5
```

💬 效果就不演示了

### 黑体字 strong

用`**`或者`__`来表记黑体：

```markdown
这是个**黑体字 strong**
```

<span class="label">效果</span><font color="orange">**黑体字 strong**</font>

### 斜体字 italic

用 `*` 或者 `_` 来表示斜体：

```markdown
这是个*斜体字 italic*
```

<span class="label">效果</span><font color="orange"> _斜体字 italic_</font>

### 删除线 strikethrought

`~~`

```markdown
~~加一条删除线 strikethrought~~
```

<span class="label">效果</span><font color="orange">~~加一条删除线 strikethrought~~</font>

### 引用块

```markdown
> 道可，道非，常道
```

<span class="label">效果</span><font color="orange">

> 道可，道非，常道

</font>

### 无序列表 unordered list

`*, +, -` 都可以表示无序列表：

```markdown
-   美国
-   俄罗斯
-   英国
-   法国
-   中国
    -   北京
    -   上海
    -   广州
```

<span class="label">效果</span> <font color="orange">

-   美国
-   俄罗斯
-   英国
-   法国
-   中国
    -   北京
    -   上海
    -   广州

</font>

### 有序列表 ordered list

```markdown
1. 个
2. 十
3. 百
4. 千
5. 万
    1. 十万
    2. 百万
    3. 千万
6. 亿
```

<span class="label">效果</span> <font color="orange">

1. 个
2. 十
3. 百
4. 千
5. 万
    1. 十万
    2. 百万
    3. 千万
6. 亿

</font>

### 链接 link

```markdown
[点击就去 Google](https://www.google.com)
```

<span class="label">效果</span>[点击就去 Google](https://www.google.com)

### 图片 image

```markdown
![Singapore](http://www.rw-designer.com/icon-image/924-48x48x32.png)
```

<span class="label">效果</span>![Singapore](http://www.rw-designer.com/icon-image/924-48x48x32.png)

### 表格 table

略微复杂点，`｜` 控制分列，`-` 控制分行，`:` 控制对齐方式，左右或居中。

```markdown
| 职工号 | 姓名 | 年龄 |
| :----: | :--- | :--: |
|   1    | 张三 |  20  |
|   2    | 李四 |  30  |
|   3    | 王二 |  40  |
```

<span class="label">效果</span><font color="orange">

| 职工号 | 姓名 | 年龄 |
| :----- | :--- | :--: |
| 1      | 张三 |  20  |
| 2      | 李四 |  30  |
| 3      | 王二 |  40  |

</font>

表格支持还是比较不方便的，可采用在线表格转换工具：[tableconvert](https://tableconvert.com/)

### 转义

markdown 支持在特殊字符前面插入 `\`，插入之后，将不再解析这些字符，而是原样输出。

```markdown
这个不是\# 标题
这个不是\*\*黑体字\*\*
```

<span class="label">效果</span><font color="orange">

这个不是\# 标题  
这个不是\*\*黑体字\*\*

</font>

### 分割线

```markdown
---
+++
---
```

<span class="label">效果</span>

---

### 脚注

```text
你可以使用 Markdown[^1]写文档；Kubernetes[^k]非常 NB，你确定不想玩玩嘛
[^1]: Markdown 一种简易的纯文本标记语言
[^k]: 云上的操作系统
```

<span class="label">效果</span>用 Markdown[^1]写文档；Kubernetes[^k]非常 NB，你确定不想玩玩嘛  
[^1]: Markdown 一种简易的纯文本标记语言
[^k]: 云上的操作系统

## 扩展

虽然不是 markdown 语法，但非常有用的功能，静态 blog 或 CMS 基本通过第三方插件来支持，

### 代码块

\`\`\`后面接语言种类 - txt，xml，html，markdown，c，c++，java，python，bash，等等。

````text
```c
# include <stdio.h>
void main(){
printf("Hello world!\n");
}
```
````

<span class="label">效果</span>

```c
# include <stdio.h>
void main(){
	printf("Hello world!\n");
}
```

还可以有插件支持数学公式。

### html

实在不行，可以直接上 html，例如 markdown 里不支持下划线：

```html
<u>下划线</u>
```

<span class="label">效果</span><font color="orange"><u>下划线</u></font>

也不支持字体颜色：

```html
<span style="color:green">绿色字体</span>
```

<span class="label">效果</span><span style="color:green">绿色字体</span>

### 画图

Typora 支持简单的 Markdown 画图，但至今我还没有看到比较好的。

## 在线练习

<i class="fas fa-external-link-alt"></i>&nbsp; [操练一把](https://www.markdowntutorial.com/)

## 工具

-   [VS Code + markdown](https://marketplace.visualstudio.com/search?term=markdown&target=VSCode&category=All%20categories&sortBy=Relevance)
    让 VS Code 支持 markdown，目前在用

-   [Typora ](https://typora.io/)
    markdown 编辑神器，把 markdown 编辑态和展示态融为一体，所写即所见

-   [hugo](https://gohugo.io/)
    静态网站生成器，支持 markdown 来编写 blog，目前在用

-   [tableconvert](https://tableconvert.com/)
    表格转换工具

-   [markmap-lib](https://markmap.js.org/repl/)
    markdown 转 mindmap

-   [mdnice](https://mdnice.com/)
    在线 markdown 编辑，可以选择主题，直接对接微信公众号等
