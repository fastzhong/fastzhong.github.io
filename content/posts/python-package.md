+++
title = "Python的 Scripts，Modules，Packages 和 Libraries"
date = 2020-04-01T01:08:47+08:00
readingTime = true
categories = ["编程"]
tags = ["python"]
toc = true
draft = true
+++

Python 由于比较草根，概念不是那么正规，好在经过二十多年的发展开始稳定下来，这里澄清学习 Python 开发前一些最基本的概念。

<!--more-->

### Script vs Module

```python
# add.py
def add(a, b):
  return a+b
```

```python
# add.py
def add(a, b):
  return a+b

result = add(1,2)
print(result)
```

```python
# add.py

def add(a, b):
  return a+b

result = add(1,2)
print(result)
```

```python
#!/usr/bin/env python3
# add.py

def add(a, b):
  return a+b

result = add(1,2)
print(result)
```

两个文件其实都差不多：

-   第一个倾向于称之 module，没有执行什么，主要是可以被别的 python 文件引入并调用；
-   第二个倾向于称之 script，主要是独立执行，有输出；
-   第三个进一步把 python 文件变为像 bash script 那样可直接执行；
