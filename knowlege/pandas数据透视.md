
'''
重塑和轴转向
stack  将数据的列“旋转”为行
unstack  将数据的行“旋转”为列

``` python
from pandas import DataFrame,Series
import pandas as pd
import numpy as np
data = DataFrame(np.arange(6).reshape(2, 3),
       index = pd.Index(['Ohio', 'Colorado'], name='state'),
       columns = pd.Index(['one', 'two', 'three'], name='number'))
print(data)
'''
number    one  two  three
state                    
Ohio        0    1      2
Colorado    3    4      5
'''
# 将数据列选"旋转"为行
print(data.stack())
'''
state     number
Ohio      one       0
          two       1
          three     2
Colorado  one       3
          two       4
          three     5
'''
# 将数据的行“旋转”为列
print(data.unstack())
'''
number  state   
one     Ohio        0
        Colorado    3
two     Ohio        1
        Colorado    4
three   Ohio        2
        Colorado    5
'''


```
