[python总结(五)：\__get\__、\__getattr\__、\__getitem\__、\__getattribute\__之间的差异与联系](https://blog.csdn.net/yiifaa/article/details/78068962)



python的一切数据都是对象，包括函数、基本数据类型、自定义数据类型等等，这其中最复杂的就是对象内部存储的数据结构(引用)，包括类属性、数据描述符、实例属性及非数据描述符，不仅它们的优先级不一样，而且它们的回调函数也存在很大的差异，这也是本文需要阐述的地方。

如果以前有过Javascript的编程经验，初上Python肯定会对“.”运算符与“[]”之间的差异难以理解，它们不仅不能替换，而且完全不相关，如下：
``` python
mem = {'username': 'yiifaa'}
# 无法通过.运算符进行计算
mem.username
# 声明String
name = str('yiifaa')
# 无法使用“[]”运算符
name['upper']
```
### 1. 为实例添加“[]”运算符支持
这也是“_ getattribute_”与“_ getitem_”的最大差异，示例如下：
###### 1. “_ getattribute_”只适用于所有的“.”运算符；
###### 2. “_ getitem_”只适用于所有的“[]”运算符；

``` python
class Employee(object):

    def __init__(self, username, age):
        self.username = username
        self.age = age

    def __getattribute__(self, attr):
        return super(Employee, self).__getattribute__(attr)

    def __getitem__(self, attr):
        return super(Employee, self).__getattribute__(attr)

em = Employee('yiifaa', 32)
print em.username
#   现在支持“[]”运算符
print em['username']
```
通过实现“**\__getitem\__**”回调接口，现在Employee可以支持“[]”运算符。

### 2. 避免语法错误错误
在对象属性的调用中，如果没有调用了不存在的属性，则Python解释器会报错，如下：
``` python
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'str' object has no attribute 'length'
```
通过**覆盖**实现**\__ getattr\__**”回调接口可以解决此问题，如下：

``` python
#   直接返回空对象，将此方法添加到类Employee的声明中
def __getattr__(self, attr):
    return None

# 现在调用不存在的属性也不会报错
print em.company
```
那“**\__ getattribute\__**”与“**\__ getattr\__**”的最大差异在于：
###### 1. 无论调用对象的什么属性，包括不存在的属性，都会首先调用“**__getattribute\__**”方法；
###### 2. 只有找不到对象的属性时，才会调用“**\__ getattr__**”方法；



### 3. 将对象作为数据描述符
这就是**\__get__**”的作用了，将整个对象都作为数据描述符，但是请记住，要想**\__get\__**作为数据描述符，那么此对象只能作为**类属性**，作为**实例属性则无效**，如下：
``` python
class Dept(object):

      def __init__(self, name):
          self.name = name

      # target是拥有此属性的对象
      def __get__(self, target, type=None):
        # 默认返回self与obj都可以
        return 'Dept'

class Company(object):
    #   一定要作为类属性，作为实例属性无效
    dept = Dept('organ')

# 现在的测试结果
x = Company()
#   返回True
print type(x.dept) == str
```


### 4. 获取对象属性数据的三种方法
　　对象的所有**属性**都存储在**\__dict\__**中（启用了“_ slots_”除外），所以访问对象的属性数据有如下三种方法：
``` python
print yiifaa.name
print yiifaa.__dict__['name']
print getattr(yiifaa, 'name')
```
结论
每个以“**\__get**”为前缀的方法都是获取**对象内部数据**的**钩子**，但名称不一样，用途也存在较大的差异，只有在实践中理解它们，才能真正掌握它们的用法。
