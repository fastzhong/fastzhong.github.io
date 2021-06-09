+++
title = "Python 102（基本语法）"
date = 2021-02-18T01:08:47+08:00
readingTime = true
categories = ["编程"]
tags = ["python"]
toc = true
+++

Python 基本语法，有编程经验的可以瞬间上手

<!--more-->

参考：[<i class="fas fa-external-link-alt"></i>&nbsp; Python 101](/posts/python101)

## 安装 Anaconda

1. 下载并安装 Anaconda: https://www.anaconda.com/products/individual
2. 运行 Anaconda Navigator
   ![Anaconda Navigator](/images/python/anaconda-navigator.png#center)
3. 创建一个独立的虚拟环境并激活它， 例如 mth251：

```
[I] ➜ mkdir workspace/suss/MTH251 && cd workspace/suss/MTH251
[I] ➜ conda create -m mth251 python=3.8.5
```

4. 切换到 mth251，然后安装 jupyter notebook：
   ![jupyter notebook](/images/python/jupyter-notebook.png#center)
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

㊫ 结合代码看一目了然 [Python101 notebook](https://github.com/fastzhong/fastzhong.github.io/blob/master/static/resources/python101.ipynb)

![Python3](/images/python/Python3.png#center)

## Python 程序

有几种形式：

```python
# add1.py
def add(a, b):
  return a+b
```

```python
# app.py
import add from add1

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

-   第一个倾向于称之模块 <span class="kwd">module</span>，定义或声明，没有执行什么，可以被别的 python 文件作为模块引入并调用（import）；
-   第二个倾向于称之脚本 <span class="kwd">script</span>，主要是可以独立执行，\_\_main\_\_是主函数入口，输出如下：

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

有了以上知识基本上可以开始编程，其它一些非常有用的高级功能，如 lambda，itertools， decorator 等等以后做博客。

---

<i class="fas fa-map-marker-alt"></i>&nbsp;&nbsp; Python 的语法总结：[<i class="fas fa-external-link-alt"></i>&nbsp; Learn python3 in one picture.](https://github.com/coodict/python3-in-one-pic)
