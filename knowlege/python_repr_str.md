#### 1. __str__与__repr__的区别
``` python
class Test:
    def __init__(self, value="hello world!"):
        self.value = value
test = Test()
test  # <__main__.Test instance at 0x7fa0dd5941b8>
print test #  <__main__.Test instance at 0x7fa0dd5941b8>
```

上面打印类对象并不是很友好，显示的是对象的内存地址


下面我们重构下该类的**\__repr\__**以及**\__str\__**，看看它们俩有啥区别

###### (1)重写__repr__

``` python
class Test:
    def __init__(self, value="hello world!"):
        self.value = value
    def __repr__(self):
        return 'using repr({})'.format(self.value)
test = Test()
test # using repr(hello world!)
print test  # using repr(hello world!)
```
重构**\__repr\__**方法后，不管直接输出对象还是通过print打印的信息都按我们**\__repr\__**方法中定义的格式进行显示了

###### (2)重写__str__
``` python
class Test:
    def __init__(self, value="hello world!"):
        self.value = value
    def __repr__(self):
        return 'using repr({})'.format(self.value)
    def __str__(self):
        return 'using str({})'.format(self.value)

test = Test()
test # using repr(hello world!)
print test  # using str(hello world!)
```
直接输出对象ts时并没有按我们**\__str\__**方法中定义的格式进行输出，而用print输出的信息却改变了

**\__repr\__**和**\__str\__**这两个方法都是用于显示的，**\__str\__**是面向用户的，
而**\__repr\__**面向程序员。

打印操作会首先尝试__str__和str内置函数(print运行的内部等价形式)，它通常应该返回一个友好的显示。

**\__repr\__**用于所有其他的环境中：用于交互模式下提示回应以及repr函数，如果没有使用**\__str\__**，会使用print和**\__str\__**。它通常应该返回一个编码字符串，可以用来重新创建对象，或者给开发者详细的显示。

当我们想所有环境下**都统一显示**的话，可以重构**\__repr\__**方法；当我们想在不同环境下支持不同的显示，例如终端用户显示使用**\__str\__**，而程序员在开发期间则使用底层的**\__repr\__**来显示，实际上**\__str\__**只是覆盖了**\__repr\__**得到更友好的用户显示。
