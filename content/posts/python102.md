+++
title = "Python 102 in 2021"
date = 2021-02-18T01:08:47+08:00
readingTime = true
categories = ["编程"]
tags = ["python"]
toc = true
+++

Python 基本语法，有编程经验的可以瞬间上手

<!--more-->

## 安装 Anaconda

1. 下载并安装 Anaconda: https://www.anaconda.com/products/individual
2. 运行 Anaconda Navigator
   ![Anaconda Navigator](/images/python101/anaconda-navigator.png#center)
3. 创建一个独立的虚拟环境并激活它， 例如 mth251：

```
[I] ➜ conda create -m mth251 python=3.8.5
```

4. 切换到 mth251，然后安装 jupyter notebook：
   ![jupyter notebook](/images/python101/jupyter-notebook.png#center)
5. 激活 mth251，然后启动 jupyter notebook

```bash
[I] ➜ conda activate mth251
(mth251) 13:03:38 in workspace/suss/MTH251 via 🅒 mth251
[I] ➜ conda activate mth251
(mth251) 13:11:18 in workspace/suss/MTH251 via 🅒 mth251
[I] ➜ jupyter notebook
```

jupyter notebook 是一个在线互动的编程环境，可以敲下第一行代码：

```python
print("Hello, world!")
```

## Python 基本语法

[㊫ Python101 notebook](https://github.com/fastzhong/fastzhong.github.io/blob/master/static/resources/python101.ipynb)

### Data Type

-   numbers: integer, float, complex
-   string: “” ‘’ \
-   byte
-   boolean
-   operators, casting
-   list: [], indexing, slicing, len, append, insert, pop, push
-   tuple: (), immutable list
-   set: {}, not repeatable
-   dict: {}, key-value pairs
-   None
-   dynamic data type

### Variable

local vs. global

### Syntax & code block

-   import
-   comments: #, “”” ”””
-   indention

### flow control

-   for
-   while
-   break/continue
-   if elif else

### function

-   def f()：
-   arguments

### Object-Orient Program

-   class ClassA & instance
-   \_\_int\_\_ & self
-   encapsulation: attributes & methods
-   inheritance: class ChildClass(Parent)
-   Child can override Parent
-   polymorphism: same interface, different implementation
-   everything is OO

### Python 程序

有几种形式：

```python
# add1.py
def add(a, b):
  return a+b

result = add(1,2)
print(result)
```

```python
# add2.py
def add(a, b):
  return a+b

result = add(1,2)
print(result)

if __name__=="__main__":
    result = add(3,4)
    print(result)
```

```python
#!/usr/bin/env python3
# add3.py

def add(a, b):
  return a+b

if __name__=="__main__":
    result = add(1,2)
    print(result)
```

三个文件其实都差不多：

-   第一个倾向于称之模块 module，没有执行什么，主要是可以被别的 python 文件引入并调用；
-   第二个倾向于称之脚本 script，主要是可以独立执行，\_\_main\_\_是主函数入口，输出如下：

```bash
[I] ➜ python add2.py
3
7
```

-   第三个进一步把上面脚本变为像 bash script （第一行类似 #!/usr/bin/env bash）那样可直接执行：

```bash
[I] ➜ add3.py
3
7
```
