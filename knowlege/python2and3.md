[链接地址](http://www.runoob.com/python/python-2x-3x.html)
### 1. uicode
Python 2 有 ASCII str() 类型，unicode() 是单独的，不是 byte 类型。

   现在， 在 Python 3，我们最终有了 Unicode (utf-8) 字符串，以及一个字节类：byte 和 bytearrays。

由于 Python3.X 源码文件默认使用utf-8编码，这就使得以下代码是合法的：

``` python
中国 = 'china'
>>>print(中国)
china
```

###### python2.x 如下:
``` python
>>> str = "我爱北京天安门"
>>> str
'\xe6\x88\x91\xe7\x88\xb1\xe5\x8c\x97\xe4\xba\xac\xe5\xa4\xa9\xe5\xae\x89\xe9\x97\xa8'
>>> str = u"我爱北京天安门"
>>> str
u'\u6211\u7231\u5317\u4eac\u5929\u5b89\u95e8'
```

###### python3.x 如下:
``` python
>>> str = "我爱北京天安门"
>>> str
'我爱北京天安门'
```

### 2.除法运算
Python中的除法较其它语言显得非常高端，有套很复杂的规则。Python中的除法有两个运算符，/和//

首先来说/除法:
在python 2.x中/除法就跟我们熟悉的大多数语言，比如Java啊C啊差不多，整数相除的结果是一个整数，把小数部分完全忽略掉，浮点数除法会保留小数点的部分得到一个浮点数的结果。

**在python 3.x中/除法不再这么做了，对于整数之间的相除，结果也会是浮点数。**



### 3.异常
在 Python 3 中处理异常也轻微的改变了，在 Python 3 中我们现在**使用 as 作为关键词**。

捕获异常的语法由 except exc, var 改为 except exc as var。

使用语法except (exc1, exc2) as var可以同时捕获多种类别的异常。 Python 2.6已经支持这两种语法。

1. 在2.x时代，所有类型的对象都是可以被直接抛出的，在3.x时代，只有继承自BaseException的对象才可以被抛出。
2. 2.x raise语句使用逗号将抛出对象类型和参数分开，3.x取消了这种奇葩的写法，直接调用构造函数抛出对象即可。
在2.x时代，异常在代码中除了表示程序错误，还经常做一些普通控制结构应该做的事情，在3.x中可以看出，设计者让异常变的更加专一，只有在错误发生的情况才能去用异常捕获语句来处理。


### 4.xrange
在 Python 2 中 xrange() 创建迭代对象的用法是非常流行的。

比如： for 循环或者是列表/集合/字典推导式。

这个表现十分像生成器（比如。**"惰性求值"**）。但是这个 xrange-iterable 是无穷的，意味着你可以无限遍历。

由于它的惰性求值，如果你不得仅仅不遍历它一次，xrange() 函数 比 range() 更快（比如 for 循环）。尽管如此，对比迭代一次，不建议你重复迭代多次，因为生成器每次都从头开始。

在 **Python 3** 中，**range() 是像 xrange() 那样实现以至于一个专门的 xrange() 函数都不再存在（在 Python 3 中 xrange() 会抛出命名异常）**。

``` python

import timeit

n = 10000
def test_range(n):
    return for i in range(n):
        pass

def test_xrange(n):
    for i in xrange(n):
        pass   
```
