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




#### 可调用的对象
> 　　你也许已经知道，在Python中，方法是最高级的对象。这意味着他们也可以被传递到方法中，就像其他对象一样。这是一个非常惊人的特性。

> 　　在Python中，一个特殊的魔术方法可以让类的实例的行为表现的像函数一样，你可以调用它们，将**一个函数**当做一个**参数**
传到**另外一个函数**中等等。这是一个非常强大的特性，其让Python编程更加舒适甜美。

``` python
__call__(self, [args...]):
```
　　　允许一个类的**实例**像函数一样被调用。实质上说，这意味着 x() 与 **x.\__call__()** 是相同的。
注意 **\__call__** 的参数可变。这意味着你可以定义 **\__call__** 为其他你想要的函数，无论有多少个参数。

　　**\__call__** 在那些类的实例经常改变状态的时候会非常有效。调用这个实例是一种**改变这个对象状态的直接和优雅的做法**。
用一个实例来表达最好不过了:

``` python
# -*- coding: UTF-8 -*-
class Entity:
    """
    调用实体来改变实体的位置
    """
def __init__(self, size, x, y):
    self.x, self.y = x, y
    self.size = size
def __call__(self, x, y):
    """
    改变实体的位置
    """
    self.x, self.y = x, y
```

#### 创建对象描述器
　　描述器是通过获取、设置以及删除的时候被访问的类。当然也可以改变其它的对象。描述器并**不是独立的**。
相反，它意味着被一个所有者类持有。当创建面向对象的数据库或者类，里面含有相互依赖的属相时，描述器将会非常有用。
一种典型的使用方法是用**不同的单位表示同一个数值**，**或者表示某个数据的附加属性**。

　　为了成为一个**描述器**，一个类必须至少有**\__get\__，\__set\__，\__delete\__**方法被实现：

``` python
__get__(self, instance, owner):
```

定义了当描述器的值被取得的时候的行为。instance是拥有该描述器对象的一个实例。owner是拥有者本身
``` python
__set__(self, instance, value):
```
定义了当描述器的值被改变的时候的行为。instance是拥有该描述器类的一个实例。value是要设置的值。
``` python
__delete__(self, instance):
```
定义了当描述器的值被删除的时候的行为。instance是拥有该描述器对象的一个实例。

下面是一个描述器的实例：单位转换。
``` python
# -*- coding: UTF-8 -*-
class Meter(object):
    """
    对于单位"米"的描述器
    """
    def __init__(self, value=0.0):
        self.value = float(value)
    def __get__(self, instance, owner):
        return self.value
    def __set__(self, instance, value):
        self.value = float(value)
class Foot(object):
    """
    对于单位"英尺"的描述器
    """
    def __get__(self, instance, owner):
        return instance.meter * 3.2808
    def __set__(self, instance, value):
        instance.meter = float(value) / 3.2808
class Distance(object):
    """
    用米和英寸来表示两个描述器之间的距离
    """
    meter = Meter(10)
    foot = Foot()
　　使用时：
>>>d = Distance()
>>>print d.foot
>>>print d.meter
32.808
10.0
```


#### 复制
　　有时候，尤其是当你在处理可变对象时，你可能想要复制一个对象，然后对其做出一些改变而不希望影响原来的对象。这就是Python的copy所发挥作用的地方。
``` python
__copy__(self):
```
　 　　定义了当对你的类的实例调用copy.copy()时所产生的行为。copy.copy()返回了你的对象的一个浅拷贝——这意味着，当实例本身是一个新实例时，它的所有数据都被引用了——例如，当一个对象本身被复制了，它的数据仍然是被引用的（因此，对于浅拷贝中数据的更改仍然可能导致数据在原始对象的中的改变）。

``` python
__deepcopy__(self, memodict={}):
```
　　定义了当对你的类的实例调用**copy.deepcopy()**时所产生的行为。
**copy.deepcopy()**返回了你的对象的一个深拷贝——对象和其数据都被拷贝了。memodict是对之前被拷贝的对象的一个缓存——这优化了拷贝过程并且阻止了对递归数据结构拷贝时的无限递归。当你想要进行对一个单独的属性进行深拷贝时，调用copy.deepcopy()，并以memodict为第一个参数。



















###
