##### python global (python简明教程)

> 在函数内部定义变量时，他们与函数外部具有相同名称的其他变量没有任何关系，即变量名称对于函数来说是局部的，这称为变量的作用域，示例如下：

``` python
def func_local(x):
    print 'x is', x
    x = 2
    print 'Chanaged local x to',x

x = 50
func_local(x)
print 'x is still', x
#执行结果如下
# x is 50
# Chanaged local x to 2
# x is still 50
```

> 如果想在函数内部改变函数外的变量值，用global语句完成

``` python
def func_global():
    global y
    print 'y is', y
    y = 50
    print 'Changed local y to', y

y = 10
func_global()
print 'Value of y is', y
#执行结果如下：
# y is 10
# Changed local y to 50
# Value of y is 50
```
> 函数参数若是list、set、dict可变参数，在函数内改变参数，会导致该参数发生变化，例如：

``` python
def func_local(x):
    print 'x is', x
    x.append(10)
    print 'Chanaged local x to',x

x = range(6)
func_local(x)
print 'x is', x
# 执行结果如下：
#x is [0, 1, 2, 3, 4, 5]
#Chanaged local x to [0, 1, 2, 3, 4, 5, 10]
#x is [0, 1, 2, 3, 4, 5, 10]
```

``` python

def func_local(x):
    print 'x is', x
    x.add(10)
    print 'Chanaged local x to',x

x = set(range(6))
func_local(x)
print 'x is', x
# 执行结果如下：
# x is set([0, 1, 2, 3, 4, 5])
# Chanaged local x to set([0, 1, 2, 3, 4, 5, 10])
# x is set([0, 1, 2, 3, 4, 5, 10])
```
``` python
def func_local(x):
    print 'x is', x
    x['x'] = 2
    print 'Chanaged local x to',x

x = dict([('x',1), ('y', 2)])
func_local(x)
print 'x is', x
# 执行结果如下：
# x is {'y': 2, 'x': 1}
# Chanaged local x to {'y': 2, 'x': 2}
# x is {'y': 2, 'x': 2}
```
``` python
def func_local(x):
    print 'x is', x
    x = (4, 5, 6)
    print 'Chanaged local x to',x

x = (1,2,3,)
func_local(x)
print 'x is', x
# x is (1, 2, 3)
# Chanaged local x to (4, 5, 6)
# x is (1, 2, 3)
```
## 若传入可变参数如list、set、dict，在函数内部对参数做出修改，参数本身发生变化，tuple、str不变

'tuple' object does not support item assignment
