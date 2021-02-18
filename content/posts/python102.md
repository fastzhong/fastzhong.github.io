+++
title = "Python 102 in 2021"
date = 2021-02-20T01:08:47+08:00
readingTime = true
categories = ["编程"]
tags = ["python"]
toc = true
+++

Python 基本语法，有编程经验的可以瞬间上手

<!--more-->

https://www.processon.com/view/link/5f5059d6f346fb7afd40407c#map

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
-   第二个倾向于称之 script，主要是可以独立执行，有输出；
-   第三个进一步把 python 文件变为像 bash script （第一行类似 #!/usr/bin/env bash）那样可直接执行；
