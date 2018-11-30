#### python gdb调试　

在交互环境中通常使用pdb.run来调试：

``` python
import pdb

def pdb_test(arg):
    for i in range(arg):
        print(i)
    return arg

pdb.run("pdb_test(3)")
```



###### b 函数名、行号：

     打断点，b可以查询所有的断点。
``` python
(Pdb) b pdb_test
Breakpoint 1 at c:\users\plpcc\desktop\pdbtest.py:3
(Pdb) b
Num Type         Disp Enb   Where
1   breakpoint   keep yes   at c:\users\plpcc\desktop\pdbtest.py:3
```


######  c：运行程序，直到遇到断点。
``` python
(Pdb) c
> c:\users\plpcc\desktop\pdbtest.py(4)pdb_test()
-> for i in range(arg):
```


###### l：查看断点周围的代码
``` python
(Pdb) l
1     import pdb
2    
3 B   def pdb_test(arg):
4  ->      for i in range(arg):
5             print(i)
6         return arg
7    
8     pdb.run("pdb_test(3)")
```



###### a:查看参数
``` python
(Pdb) a
arg = 3
```



###### s, n：单步运行,区别s会进入路径中的函数，n不会进入

p：查看表达式的值
```  python
(Pdb) p i
0
condition：
```
   条件断点，只有条件为true断点才命中

> c:\users\plpcc\desktop\pdbtest.py(5)pdb_test()
-> print(i)
(Pdb) l
1     import pdb
2    
3     def pdb_test(arg):
4         for i in range(arg):
5 B->          print(i)
6         return arg
7    
8     pdb.run("pdb_test(3)")
[EOF]
(Pdb) b
Num Type         Disp Enb   Where
2   breakpoint   keep yes   at c:\users\plpcc\desktop\pdbtest.py:5
(Pdb) condition 2 i==1   //i==1时才触发断点2
New condition set for breakpoint 2.
(Pdb) b
Num Type         Disp Enb   Where
2   breakpoint   keep yes   at c:\users\plpcc\desktop\pdbtest.py:5
  stop only if i==1
(Pdb) c
0                       //i==0直接打印未断住
> c:\users\plpcc\desktop\pdbtest.py(5)pdb_test()
-> print(i)             //触发断点，i==1
(Pdb) p i
1
bt:

    查看调用堆栈

(Pdb) bt
c:\python33\lib\bdb.py(405)run()
-> exec(cmd, globals, locals)
<string>(1)<module>()
> c:\users\plpcc\desktop\pdbtest.py(5)pdb_test()
-> print(i)
r：

    执行到函数返回

(Pdb) r
1
2
--Return--
> c:\users\plpcc\desktop\pdbtest.py(6)pdb_test()->3 //代码位置、函数返回值->3
-> return arg                                       //代码位置的语句
(Pdb) l
1     import pdb
2    
3     def pdb_test(arg):
4         for i in range(arg):
5             print(i)
6  ->      return arg
7    
8     pdb.run("pdb_test(3)")


通过pdb.set_trace() 在代码中指定位置嵌入一个断点，通常可以通过调试开关来控制

import pdb

__DEBUG__ = True

def pdb_test(arg):
  if True == __DEBUG__:
      pdb.set_trace()
  for i in range(arg):
      print(i)
  return arg

pdb_test(3)


运行后在pdb.set_trace()位置被断住，当__DEBUG__ = False，代码正常运行

> c:\users\plpcc\desktop\pdbtest.py(8)pdb_test()
-> for i in range(arg):
(Pdb) l
3     __DEBUG__ = True
4    
5     def pdb_test(arg):
6         if True == __DEBUG__:
7             pdb.set_trace()
8  ->      for i in range(arg):
9             print(i)
10         return arg
11    
12     pdb_test(3)
[EOF]


通过pdb.pm()进行事后调试，可以跟踪异常程序最后的堆载信息：

Traceback (most recent call last):
File "C:\Users\plpcc\Desktop\pdbTest.py", line 13, in <module>
  pdb_test(3)
File "C:\Users\plpcc\Desktop\pdbTest.py", line 10, in pdb_test
  1/0
ZeroDivisionError: division by zero
>>> import pdb
>>> pdb.pm()
> c:\users\plpcc\desktop\pdbtest.py(10)pdb_test()
-> 1/0
(Pdb) l
5     def pdb_test(arg):
6         if True == __DEBUG__:
7             pdb.set_trace()
8         for i in range(arg):
9             print(i)
10  ->          1/0
11         return arg
12    
13     pdb_test(3)
