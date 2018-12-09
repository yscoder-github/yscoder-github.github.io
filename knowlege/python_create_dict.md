[Python 创建字典的多种方式](http://www.cnblogs.com/yooma/p/8065481.html)


###### 1.通过关键字dict和关键字参数创建
``` python
dic = dict(spam = 1, egg = 2, bar =3)
print dic # {'bar': 3, 'egg': 2, 'spam': 1}
```


###### 2.通过二元组列表创建
``` python
list = [('spam', 1), ('egg', 2), ('bar', 3)]
>>> dic = dict(list)
>>> dic
{'bar': 3, 'egg': 2, 'spam': 1}
```


##### 3.dict和zip结合创建
``` python
dic = dict(zip('abc', [1, 2, 3]))
>>> dic
{'a': 1, 'c': 3, 'b': 2}
```

##### 4.通过字典推导式创建
``` python
>>> dic = {i:2*i for i in range(3)}
>>> dic
{0: 0, 1: 2, 2: 4}
```

##### 5.通过dict.fromkeys()创建
通常用来初始化字典, 设置value的默认值
```
>>> dic = dict.fromkeys(range(3), 'x')
>>> dic
{0: 'x', 1: 'x', 2: 'x'}
```  


##### 6.其他
``` python
>>> list = ['x', 1, 'y', 2, 'z', 3]
>>> dic = dict(zip(list[::2], list[1::2]))
>>> dic
{'y': 2, 'x': 1, 'z': 3}
```
