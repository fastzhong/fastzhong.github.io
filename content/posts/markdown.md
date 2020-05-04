+++
title = "2 分钟了解 Markdown"
date = 2018-04-15T19:04:41+08:00
categories = ["工具"]
tags = []
toc = false
+++

Markdown 和 Json 一样都是好东西，简单、好用，利于专注写作：

<!--more-->

![using-markup-for-languages-in-hugo_01](/images/2mins-markdown/using-markup-for-languages-in-hugo_01.png)

![using-markup-for-languages-in-hugo_02](/images/2mins-markdown/using-markup-for-languages-in-hugo_02.png)

Markdown 就是一种轻量级的标记语言（Lightweight Markup Language），用人话说就是，普通的文本加上简单的排版或格式化语法（特殊标记），和 HTML，RTF 类似，只是 Markdown 的语法非常简单。Markdown 编写工具很多，在线的也有，很多编程开发工具都支持。需要知道，Markdown 语法没有标准，取决于 Markdown 渲染引擎，各个系统略微有差异。VS Code 和 IntelliJ 都支持 Markdown，特别推荐专门的可视化编辑器 [Typora](https://typora.io/)。

下面直接介绍 Markdown 的基本语法：

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

### 黑体字 strong

用`**`或者`__`来表记黑体：

```markdown
这是个**黑体字 strong**
```

效果：
这是个**黑体字 strong**

### 斜体字 italic

用 `*` 或者 `_` 来表示斜体：

```markdown
这是个*斜体字 italic*
```

效果：
这是个*斜体字 italic*

### 删除线 strikethrought

`~~`

```markdown
~~加一条删除线 strikethrought~~
```

效果：
~~加一条删除线 strikethrought~~

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

效果：

-   美国
-   俄罗斯
-   英国
-   法国
-   中国
    -   北京
    -   上海
    -   广州

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

效果：

1. 个
2. 十
3. 百
4. 千
5. 万
    1. 十万
    2. 百万
    3. 千万
6. 亿

### 链接 link

```markdown
[点击就去 Google](https://www.google.com)
```

效果：
[点击就去 Google](https://www.google.com)

### 图片 image

```markdown
![Singapore](http://www.rw-designer.com/icon-image/924-48x48x32.png)
```

效果：
![Singapore](http://www.rw-designer.com/icon-image/924-48x48x32.png)

### 表格 table

略微复杂点，`｜` 控制分列，`-` 控制分行，`:` 控制对齐方式，左右或居中。

```markdown
| 职工号 | 姓名 | 年龄 |
| :----: | :--- | :--: |
|   1    | 张三 |  20  |
|   2    | 李四 |  30  |
|   3    | 王二 |  40  |
```

效果：

| 职工号 | 姓名 | 年龄 |
| :----- | :--- | :--: |
| 1      | 张三 |  20  |
| 2      | 李四 |  30  |
| 3      | 王二 |  40  |

### 转义

markdown 支持在特殊字符前面插入 `\`，插入之后，将不再解析这些字符，而是原样输出。

```markdown
这个不是\# 标题
这个不是\*\*黑体字\*\*
```

效果：

这个不是\# 标题  
这个不是\*\*黑体字\*\*

### 分割线

```markdown
--- 
```

效果：

---
