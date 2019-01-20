[tensorflow数学运算](https://blog.csdn.net/zywvvd/article/details/78593618)　　　　

一、Tensor 之间的运算规则  
* 相同大小 Tensor 之间的任何算术运算都会将运算应用到元素级  
* 不同大小 Tensor(要求dimension 0 必须相同) 之间的运算叫做**广播**(broadcasting)  
* Tensor 与 Scalar(0维 tensor) 间的算术运算会将那个标量值传播到各个元素
* Note： TensorFLow 在进行数学运算时，一定要求各个 Tensor **数据类型一致**

二、常用操作符和基本数学函数
大多数运算符都进行了重载操作，使我们可以快速使用 (+ - * /) 等，但是有一点不好的是使用重载操作符后就不能为每个操作命名了。

#####  算术操作符：+ - * / %
tf.add(x, y, name=None)        # 加法(支持 broadcasting)  
tf.subtract(x, y, name=None)   # 减法  
tf.multiply(x, y, name=None)   # 乘法  
tf.divide(x, y, name=None)     # 浮点除法, 返回浮点数(python3 除法)  
tf.mod(x, y, name=None)        # 取余  


##### 幂指对数操作符：^ ^2 ^0.5 e^ ln
tf.pow(x, y, name=None)        # 幂次方  
tf.square(x, name=None)        # 平方  
tf.sqrt(x, name=None)          # 开根号，必须传入浮点数或复数  
tf.exp(x, name=None)           # 计算 e 的次方  
tf.log(x, name=None)           # 以 e 为底，必须传入浮点数或复数  


##### 取符号、负、倒数、绝对值、近似、两数中较大/小的
tf.negative(x, name=None)      # 取负(y = -x).  
tf.sign(x, name=None)          # 返回 x 的符号  
tf.reciprocal(x, name=None)    # 取倒数
tf.abs(x, name=None)           # 求绝对值  
tf.round(x, name=None)         # 四舍五入    
tf.ceil(x, name=None)          # 向上取整  
tf.floor(x, name=None)         # 向下取整
tf.rint(x, name=None)          # 取最接近的整数  
tf.maximum(x, y, name=None)    # 返回两tensor中的最大值 (x > y ? x : y)  
tf.minimum(x, y, name=None)    # 返回两tensor中的最小值 (x < y ? x : y)  


# 三角函数和反三角函数
tf.cos(x, name=None)      
tf.sin(x, name=None)      
tf.tan(x, name=None)      
tf.acos(x, name=None)  
tf.asin(x, name=None)  
tf.atan(x, name=None)     


# 其它
tf.div(x, y, name=None)  # python 2.7 除法, x/y-->int or x/float(y)-->float  
tf.truediv(x, y, name=None) # python 3 除法, x/y-->float  
tf.floordiv(x, y, name=None)  # python 3 除法, x//y-->int  
tf.realdiv(x, y, name=None)  
tf.truncatediv(x, y, name=None)  
tf.floor_div(x, y, name=None)  
tf.truncatemod(x, y, name=None)  
tf.floormod(x, y, name=None)  
tf.cross(x, y, name=None)  
tf.add_n(inputs, name=None)  # inputs: A list of Tensor objects, each with same shape and type  
tf.squared_difference(x, y, name=None)  
