#### 一.类与对象
######  1、抽象基类（abc模块）

``` python
# 可判断某类是否有某方法
hasattr(obj, '__len__')


# 我们在某些情况之下希望判定某个对象的类型
isinstance(obj, list)


# 我们需要强制某个子类必须实现某些方法
# 如：实现了一个web框架，集合cache（如希望可替换成 redis, cache, memorychache 之一）
# 则需要设计一个抽象基类，指定子类必须实现某些方法
# 如何去模拟一个抽象基类：
class CacheBase():
    def get(self, key):
        raise NotImplementedError  #子类未重载该方法时，调用该方法会抛出异常
    def set(set, key, value):
        raise NotImplementedError

class RedisCache(CacheBase):
    pass

redis_cache = RedisCache()
# 调用时会抛出异常
redis_cache.set('key', 'value')

# 如果我们希望初始化时就抛出异常：
import abc
class CacheBase(metaclss=abc.ABCMeta):
    @abc.abstractmethod
    def get(self, key)
        pass

    @abc.abstractmethod
    def set(set, key, value):
        raise NotImplementedError

class RedisCache(CacheBase):
    pass

# 初始化时会抛出异常
redis_cache = RedisCache()
```

###### 2、isinstance 与 type 区别
type **无法找出继承关系**, 而**isinstance是可以的**

因此，类型判断一般使用isinstance ，**少用type**

``` python
class A:
    pass

class B(A):
    pass

b = B()
print(isinstance(b, B))    # 返回：True
print(isinstance(b, A))    # 返回：True

print(type(b))       # 返回：<class '__main__.B'>
print(type(b) is B)  # 返回：True
# type 无法找出继承关系
print(type(b) is A)  # 返回：False
```
##### 3、类变量 和 对象变量
* **类变量**：类 及 对象均可以调用，

  **不同点在于**：类调用时如果修改该变量数据，则在该类中数据被永久修改；对象在调用该变量时，如果是修改该变量，此时修改的是自己<u>对象中该变量的值</u>，不会影响到类中该变量的值

* **对象变量**：类不能调用，<u>只能对象可以调用</u>

* **私有属性/方法**：在类当中定义私有属性/方法 ，只能在类内使用，不能在类之外被点语法等调用。私有属性（**\__name**）在python内部中实际是被结构化处理成：**\_classname\__name**，在类外层调用，不能直接调用**\__name**属性，但可以调用**\_classname__name** 来获取私有属性的值，其中**classname**是类的名称





##### 4、super函数
super函数的调用，不是调用父类的放法，而是按MRO算法顺序调用的，具体参考下面代码
``` python
class A:
    def __init__(self):
        print('A')

class B:
    def __init__(self):
        print('B')
        super().__init__()

class C(A):
    def __init__(self):
        print('C')
        super().__init__()

class D(B, C):
    def __init__(self):
        print('D')
        super(D, self).__init__()

if __name__ == '__main__':
    print(D.__mro__)
# 返回：(<class '__main__.D'>,<class '__main__.B'>,<class '__main__.C'>,<class '__main__.A'>,<class 'object'>)
    d = D()
# 返回：
#   D
#   B
#   C
#   A
```

5、上下文管理器 with语句
with 上下文管理器有两个魔法函数：

**\__enter__** ：程序启动时调用

**\__exit__**   ：程序结束时调用
``` python
# 上下文管理器协议
class Sample():
    def __enter__(self):
        print('enter')
        #作用获取资源，自动调用
        return self
    def __exit__(self, exc_type, exc_val, exc_tb):
        #作用释放资源，自动调用
        print('exit')
    def do_something(self):
        print('doing something')

with Sample() as sample:
    sample.do_something()
# 返回：
#   enter
#   doing something
#   exit
```
 python 内置模块：contextlib 简化上下文管理器：
 ``` python
 import contextlib

 @contextlib.contextmanager # 加上contextlib装饰器，将file_open函数变成上下文管理器
 def file_open(file_name):
 # yield 前相当于 __enter__，yield 后相当于 __exit__
     print('file open')
     yield {}
     print('file end')

 with file_open('filename.txt') as f_opened:
     print('file processing')
 # 返回：
 #   file open
 #   file processing
 #   file end
 ```


#### 二.自定义序列类
###### 序列类型的分类　
* 容器序列：list、tuple、deque    --可放置任何类型
* 扁平序列：str、bytes、bytearray、array.array  --可遍历的、放置的数据类型需相同
* 可变序列：list、deque、bytearray、array
* 不可变序列：str、tuple、bytes

** append、extend区别：**

<u>append</u>：将数据**原原整整追加**到对象中，如a=[1,2]，a.append((3,4))，结果：[1,2,(3,4)]

<u>extend</u>：将数据**追加**到对象中，以自身类型为准。如a=[1,2]，a.extend([3,4])，结果：[1,2,3,4]

###### 实现可切片的对象
``` python
import numbers

class Group:
    def __init__(self, group_name, company_name, staffs)
        self.group_name = group_name
        self.company_name = company_name
        self.staffs = staffs

    def __reversed__(self):  # 实现数据反转功能
        self.staffs.reverse()

    def __getitem__(self, item): # 实现可切片功能
        cls = type(self)
        if isinstance(item, slice):
            return cls(group_name=self.group_name,
                 company_name=self.company_name, staffs=self.staffs[item])
        else isinstance(item, numbers.Integral):
            return cls(group_name=self.group_name,
                  company_name=self.company_name, staffs=[self.staffs[item]])

    def __len__(self):  # 实现len()查看长度功能
        return len(self.staffs)

    def __iter__(self):  # 实现可迭代功能
        return iter(self.staffs)

    def __contains__(self, item):  # 实现if判断功能
        if item in self.staffs:
            return True
        else:
            return False

staffs = ['MJ1','MJ2','MJ3','MJ4']
group = Group(company_name='i', group_name='user', staffs=staffs)
sub_group = group[:2]   # 调用getitem方法
len(group)   # 调用__len__方法
for i in group :    # 调用__iter__方法
if sb in group:     # 调用 __contains__方法
### 通过实现相关的魔法函数，可以使自定义的类具有某些功能 ###
```
###### bisect 维护已排序的序列
``` python
import bisect

# 用来处理已排序的序列，用来维持已排序的序列，升序
# 内部使用的是二分查找法
inter_list = deque()
bisect.insort(inter_list, 3) # 插入数据，默认在右边插入
bisect.insort(inter_list, 2)
bisect.insort(inter_list, 5)
bisect.insort(inter_list, 1)
bisect.insort(inter_list, 6)

print(bisect.bisect(inter_list, 3))  # 查找数据位置，   返回：3
print(bisect.bisect_left(inter_list, 3))  # 返回：2
print(inter_list)  # 返回：[1,2,3,5,6]
```

##### 列表推导式、生成器表达式、字典推导式
列表推导式性能高于列表操作

**列表生成式（列表推导式）：**
``` python  
# 1. 提取出1~20之间的奇数
odd_list = []
for i in range(21):
    if i%2 == 1:
        odd_list.append(i)
print(odd_list)

# 列表推导式：
odd_list = [i for i in range(21) if i % 2 == 1]
print(odd_list)


# 2. 逻辑复杂的情况
def hadle_item(item):
    return item * item
odd_list = [hadle_item(i) for i in range(21) if i % 2 == 1]

print(type(odd_list)) #　<class 'list'>
print(odd_list)
```

** 生成器表达式：**

生成器表达式，即将列表推导式的[]换成()即是
``` python
# 生成器表达式
odd_gen = (i for i in range(21) if i % 2 == 1)
print(type(odd_gen))  # <class 'generator'>
```

** 字典推导式：**
``` python  
# 字典推导式
my_dict = {'MJ1':22, 'MJ2':23, 'MJ3':24}
reversed_dict = {value:key for key, value in my_dict.items()}
print(reversed_dict)
```

** 集合推倒式：　**
``` python
my_dict = {'MJ1':22, 'MJ2':23, 'MJ3':24}
# 集合推导式
my_set = {key for key, value in my_dict.items()} # 只是把key放集合里
print(type(my_set)) # <type 'set'>
print(my_set) # set(['MJ1', 'MJ2', 'MJ3'])
```



#### 三、深入set、dict
dict常用方法：
``` python
a = {'name1':{'age':22}, 'name2':{'age':23}}
a.clear()  # 清空

new_dict = a.copy()  # 浅拷贝
new_dict['name1']['age'] = '25'  # a 会随之被修改

import copy
new_dict = copy.deepcopy(a)  # 深拷贝
new_dict['name1']['age'] = '26'  # a 不变


#formkeys
new_list = ['name1','name2']
new_dict = dict.fromkeys(new_list, {'age':23})  # 将可迭代对象转换成字典dict

 # 如果key没有为name，则不会报错，返回{}
new_dict.get('name', {})``

for key, value in new_dict.items():
    print(key, value)
# 不但将值取出，还将 'name':'27' 映射到字典里
default_value = new_dict.setdefault('name','27')  

new_dict.update(name='29', name3='30')
#new_dict.update([('name','31')])
#new_dict.update((('name','31'),))
```
###### set 和 frozenset：
同多数语言一样，set表示集合，最重要的特性就是**无序**，所以Python中的set不支持indexing，但支持len（set），x in set 等操作。

set有两种类型，set和frozenset。

set是可变的，有add（），remove（）等方法。既然是可变的，所以它不存在哈希值。

frozenset是冻结的集合，它是不可变的，存在哈希值，好处是它可以作为字典的key，也可以作为其它集合的元素。缺点是一旦创建便不能更改，没有add，remove方法。


``` python
#set 集合 fronzenset (不可变集合) 无序， 不重复
# s = set('abcdee')
# s = set(['a','b','c','d','e'])
s = {'a','b', 'c'}
# s = frozenset("abcde") #frozenset 可以作为dict的key
# print(s)

#向set添加数据
another_set = set("cef")
re_set = s.difference(another_set)
re_set = s - another_set
re_set = s & another_set
re_set = s | another_set

#set性能很高
# | & -  #集合运算
print(re_set)

print (s.issubset(re_set))
# if "c" in re_set:
#     print ("i am in set")

fza=frozenset('a')
adict={fza:1,'b':2} #正确
setb=set('a')
bdict={setb:1,'b':2} #错误

还有一点需要注意，不管是set还是frozenset，Python都不支持创建一个整数的集合。
seta=set(1) #错误
setb=set（'1'）#正确
```
###### 补充：
dict查找的性能远远大于list

在list中随着list数据的增大 查找时间会增大

在dict中查找元素不会随着dict的增大而增大

dict的key或者set的值 都必须是可以hash的

不可变对象 都是可hash的， 如str， fronzenset， tuple，自己实现的类 **\__hash__**

dict的内存花销大，但是查询速度快， 自定义的对象 或者python内部的对象都是用dict包装的

#### 四、对象引用、可变性和垃圾回收


[异步编程](https://www.cnblogs.com/Eric15/articles/9769044.html)


[引用地址](http://www.cnblogs.com/Eric15/articles/9749180.html)
