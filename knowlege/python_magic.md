[参考博客](https://www.cnblogs.com/pyxiaomangshe/p/7927540.html)
#### 介绍
　在Python中，所有以“**\__**”双下划线包起来的方法，都统称为“Magic Method”，中文称『魔术方法』,例如类的初始化方法 **\__init\__** ,Python中所有的魔术方法均在官方文档中有相应描述，但是对于官方的描述比较混乱而且组织比较松散。很难找到有一个例子。

##### 构造和初始化
　　每个Pythoner都知道一个最基本的魔术方法， **\__init\__** 。通过此方法我们可以定义一个对象的初始操作。然而，当调用 x = SomeClass() 的时候， **\__init\__** 并不是第一个被调用的方法。实际上，还有一个叫做**\__new\__** 的方法，两个**共同构成了“构造函数”**。
>　**\__new\__**是用来创建类并返回这个类的**实例**, 而**\__init__**只是将传入的参数来初始化该实例。


在对象生命周期调用结束时，**\__del\__** 方法会被调用，可以将**\__del__**理解为“构析函数”。下面通过代码的看一看这三个方法:
```  python
from os.path import join
class FileObject:
    '''给文件对象进行包装从而确认在删除时文件流关闭'''
    def __init__(self, filepath='~', filename='sample.txt'):
        #读写模式打开一个文件
        self.file = open(join(filepath, filename), 'r+')
    def __del__(self):
        self.file.close()
        del self.file
```

##### 控制属性访问
　　许多从其他语言转到Python的人会抱怨它缺乏类的真正封装。(没有办法定义私有变量，然后定义公共的getter和setter)。Python其实可以通过魔术方法来完成封装。我们来看一下:　　许多从其他语言转到Python的人会抱怨它缺乏类的真正封装。(没有办法定义私有变量，然后定义公共的getter和setter)。Python其实可以通过魔术方法来完成封装。我们来看一下:

``` python
__getattr__(self, name):
```
　　定义当用户试图获取一个不存在的属性时的行为。这适用于对普通拼写错误的获取和重定向，对获取一些不建议的属性时候给出警告(如果你愿意你也可以计算并且给出一个值)或者处理一个 AttributeError 。只有当调用不存在的属性的时候会被返回。
``` python
__setattr__(self, name, value):
```
　　与__getattr__(self, name)不同，__setattr__ 是一个封装的解决方案。无论属性是否存在，它都允许你定义对对属性的赋值行为，以为这你可以对属性的值进行个性定制。实现__setattr__时要避免"无限递归"的错误。




``` python
# -*- coding: utf-8 -*-
class FunctionalList:
    ''' 实现了内置类型list的功能,并丰富了一些其他方法: head, tail, init, last, drop, take'''
    def __init__(self, values=None):
        if values is None:
            self.values = []
        else:
            self.values = values
    def __len__(self):
        return len(self.values)
    def __getitem__(self, key):
        return self.values[key]
    def __setitem__(self, key, value):
        self.values[key] = value
    def __delitem__(self, key):
        del self.values[key]
    def __iter__(self):
        return iter(self.values)
    def __reversed__(self):
        return FunctionalList(reversed(self.values))
    def append(self, value):
        self.values.append(value)
    def head(self):
        # 获取第一个元素
        return self.values[0]
    def tail(self):
        # 获取第一个元素之后的所有元素
        return self.values[1:]
    def init(self):
        # 获取最后一个元素之前的所有元素
        return self.values[:-1]
    def last(self):
        # 获取最后一个元素
        return self.values[-1]
    def drop(self, n):
        # 获取所有元素，除了前N个
        return self.values[n:]
    def take(self, n):
        # 获取前N个元素
        return self.values[:n]
```







#### 补充:
###### 1.关于 **\__all\__**
``` python
# -*- coding: utf-8 -*-
__all__ = ['bar', 'baz']
waz = 5
bar = 10
def baz():
  return 'baz'

```
引用上面文件：
``` python
# -*- coding: utf-8 -*-
# import sys
from foo import *

print bar
print baz

# The following will trigger an exception, as "waz" is not exported by the module
# 下面的代码就会抛出异常，因为 "waz"并没有从模块中导出，因为 __all__ 没有定义
print waz
```

###### 2.







































###
