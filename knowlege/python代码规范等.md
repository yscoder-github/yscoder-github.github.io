[其他变量命名参考](http://www.cnblogs.com/Maker-Liu/p/5528213.html)

##### 1. 命名规范
###### (1)模块

    模块尽量使用小写命名，首字母保持小写，尽量不要用下划线(除非多个单词，且数量不多的情况)
``` Python
# 正确的模块名
import decoder
import html_parser

# 不推荐的模块名
import Decoder
```

###### (2)类名
    类名使用驼峰(CamelCase)命名风格，首字母大写，私有类可用一个下划线开头

``` python
class Farm():
    pass

class AnimalFarm(Farm):
    pass

class _PrivateFarm(Farm):
    pass
```
> 将相关的类和顶级函数放在同一个模块里. 不像Java, 没必要限制一个类一个模块.

###### (3)函数
    函数名一律小写，如有多个单词，用下划线隔开

``` python
def run():
    pass

def run_with_env():
    pass
私有函数在函数前加一个下划线_
class Person():

    def _private_func():
        pass
```

###### (4)变量名
    变量名尽量小写, 如有多个单词，用下划线隔开
``` python
if __name__ == '__main__':
    count = 0
    school_name = ''
```

###### (5)常量
    常量使用以下划线分隔的大写命名

``` python
MAX_OVERFLOW = 100

Class FooBar:

    def foo_bar(self, print_):
        print(print_)
```

#### 2. 引号
    简单说，自然语言使用双引号，机器标示使用单引号，因此 代码里 多数应该使用 单引号

(1) 自然语言 使用双引号 "...", 例如错误信息；很多情况还是 unicode，使用u"你好世界"

(2)机器标识 使用单引号 '...', 例如 dict 里的 key

(3)正则表达式 使用原生的双引号 r"..."

(4)文档字符串 (docstring) 使用三个双引号 """......"""



#### 3. import 语句
    import 语句应该分行书写
``` python
# 正确的写法
import os
import sys

# 不推荐的写法
import sys,os

# 正确的写法
from subprocess import Popen, PIPE
```
    import语句应该使用 absolute import
``` python
# 正确的写法
from foo.bar import Bar

# 不推荐的写法
from ..bar import Bar
```
    import语句应该放在文件头部，置于模块说明及docstring之后，于全局变量之前；
    import语句应该按照顺序排列，每组之间用一个空行分隔
``` python
import os
import sys

import msgpack
import zmq

import foo
```

    导入其他模块的类定义时，可以使用相对导入
``` python  
from myclass import MyClass
```
    如果发生命名冲突，则可使用命名空间
``` python
import bar
import foo.bar

bar.Bar()
```


#### 4. 注释建议　
    在代码的关键部分(或比较复杂的地方), 能写注释的要尽量写注释
    比较重要的注释段, 使用多个等号隔开, 可以更加醒目, 突出重要性
``` python
app = create_app(name, options)

# =====================================
# 请勿在此处添加 get post等app路由行为 !!!
# =====================================

if __name__ == '__main__':
    app.run()
```

##### 文档注释（Docstring）
    作为文档的Docstring一般出现在模块头部、函数和类的头部，这样在python中可以通过对象的__doc__对象获取文档.

    编辑器和IDE也可以根据Docstring给出自动提示.

    文档注释以 """ 开头和结尾, 首行不换行, 如有多行, 末行必需换行, 以下是Google的docstring风格示例
``` Python
# -*- coding: utf-8 -*-
"""Example docstrings.
This module demonstrates documentation as specified by the `Google Python
Style Guide`_. Docstrings may extend over multiple lines. Sections are created
with a section header and a colon followed by a block of indented text.
Example:
    Examples can be given using either the ``Example`` or ``Examples``
    sections. Sections support any reStructuredText formatting, including
    literal blocks::
        $ python example_google.py
Section breaks are created by resuming unindented text. Section breaks
are also implicitly created anytime a new section starts.
"""
```

     不要在文档注释复制函数定义原型, 而是具体描述其具体内容, 解释具体参数和返回值等

``` python
#  不推荐的写法(不要写函数原型等废话)
def function(a, b):
    """function(a, b) -> list"""
    ... ...

#  正确的写法
def function(a, b):
    """计算并返回a到b范围内数据的平均值"""
    ... ...
```
