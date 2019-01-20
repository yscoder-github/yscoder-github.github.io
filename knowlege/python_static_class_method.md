[(译文)Python中的staticmethod与classmethod](https://www.cnblogs.com/agnewee/p/5653936.html)

原文是stackoverflow的一则高票回答，[原文链接](https://stackoverflow.com/questions/12179271/meaning-of-classmethod-and-staticmethod-for-beginner)  


尽管classmethod和staticmethod**非常的相似**，但是两者在具体的使用上还是有着**细微的差别**：
**classmethod必须使用类对象作为第一个参数，而staticmethod则可以不传递任何参数。**

> Though classmethod and staticmethod are quite similar, there's a slight difference in usage for both entities: classmethod must have a reference to a class object as the first parameter, whereas staticmethod can have no parameters at all.


让我们通过实际的例子来看看。

让我们假设有处理日期信息的类：
``` python
class Date(object):
    day = 0
    month = 0
    year = 0

    def __init__(self, day=0, month=0, year=0):
        self.day = day
        self.month = month
        self.year = year
```
这个类很显然可以被用来存储某些日期信息（不考虑时区信息；让我们假设所有的日期都用UTC表示）  
这里定义了__init__，典型的类实例初始化方法，它作为典型的instancemethod接受参数，其中**第一个传递的必要参数是新建的实例本身。**

### 类方法
有一些可以通过使用classmethod很好解决的任务。

假设我们有很多('dd-mm-yyyy')格式字符串的日期信息，想要把它们创建成Date类实例。我们不得不在项目的不同地方做这些事情。

所以我们必须要做到：

分析得到的年月日字符串，把它们转化成三个整形变量或者拥有三个元素的元组的变量。
通过传递这些值实例化Date。
得到：

``` python
day, month, year = map(int, string_date.split('-'))
date1 = Date(day, month, year)
```
C++拥有重载的特性可以达到这种目的，但是Python缺乏此类特性。所以，python使用classmethod的方式。让我们尝试一种另类的构造函数。
``` python
    @classmethod
    def from_string(cls, date_as_string):
        day, month, year = map(date_as_string.split('-'))
        date1 = cls(day, month, year)  ### 这里是重点呢
        return date1
    date2 = Date.from_string('11-09-2012')
```


进一步分析一下以上代码的执行，以及它的优势：

* 1.在一个地方解析日期字符串并且重复使用它。  
* 2.做到很好的封装（相对于把执行字符串解析作为一个单独的函数在任何地方执行，这里使用的方法更符合OOP的范式）
* 3.cls表示**类对象**，而不是类实例。这样很酷，因为如果我们继承Date类，那么所有的子类也都将拥有from_string这个方法。



### 静态方法
那么staticmethod又是什么呢？它和classmethod非常的相似，但是**不强制要求传递参数**（但是做的事与类方法或实例方法一样）。

让我们来看一个使用的例子。

我们有一个日期字符串需要以某种方式验证。这个任务与之前**一样要定义在Date类内部**s，但是**不要求实例化它**。

静态方法在这种情况下就非常有用。看一下下面这个代码片段：
``` python
    @staticmethod
    def is_date_valid(date_as_string):
        day, month, year = map(int, date_as_string_split('-'))
        return day <= 31 and month <= 12 and year <= 3999

    is_date = Date.is_date_valid('11-09-2012')
```

现在正如我们了解到的staticmethod的使用，我们不需要访问它所属的类，它本质上就是一个函数，调用方式和调用函数一样，不同的是它不关注对象和对象内部属性。
> So, as we can see from usage of staticmethod, we don't have any access to what the class is---it's basically just a function, called syntactically like a method, but without access to the object and its internals (fields and another methods), while classmethod does.




觉得原文更好呀   
[英文原文](https://stackoverflow.com/questions/12179271/meaning-of-classmethod-and-staticmethod-for-beginner)



> Though classmethod and staticmethod are quite similar, there's a slight difference in usage for both entities: **classmethod must have a reference to a class object as the first parameter, whereas staticmethod can have no parameters at all.**

Example
``` python
class Date(object):

    def __init__(self, day=0, month=0, year=0):
        self.day = day
        self.month = month
        self.year = year

    @classmethod
    def from_string(cls, date_as_string):
        day, month, year = map(int, date_as_string.split('-'))
        date1 = cls(day, month, year)
        return date1

    @staticmethod
    def is_date_valid(date_as_string):
        day, month, year = map(int, date_as_string.split('-'))
        return day <= 31 and month <= 12 and year <= 3999

date2 = Date.from_string('11-09-2012')
is_date = Date.is_date_valid('11-09-2012')
```
Explanation
Let's assume an example of a class, dealing with date information (this will be our boilerplate):
``` python
class Date(object):

    def __init__(self, day=0, month=0, year=0):
        self.day = day
        self.month = month
        self.year = year
```

This class obviously could be used to store information about certain dates (without timezone information; let's assume all dates are presented in UTC).

Here we have \__init__, a typical initializer of Python class instances, which receives arguments as a typical instancemethod, having the first non-optional argument (self) that holds a reference to a newly created instance.

#### Class Method

We have some tasks that can be nicely done using classmethods.

Let's assume that we want to create a lot of Date class instances having date information coming from an outer source encoded as a string with format 'dd-mm-yyyy'. Suppose we have to do this in different places in the source code of our project.

So what we must do here is:

Parse a string to receive day, month and year as three integer variables or a 3-item tuple consisting of that variable.
Instantiate Date by passing those values to the initialization call.
This **will look like:**  **很明显下面这样很不美观呢而且会经常出现冗余代码呢 not reusable and not in one place compared to the method write using classmethod **

``` python
day, month, year = map(int, string_date.split('-'))
date1 = Date(day, month, year)
```

For this purpose, C++ can implement such a feature with overloading, but Python lacks this overloading. Instead, we can use classmethod. Let's create another "constructor".

``` python
    @classmethod
    def from_string(cls, date_as_string):
        day, month, year = map(int, date_as_string.split('-'))
        date1 = cls(day, month, year)
        return date1

date2 = Date.from_string('11-09-2012')
```
Let's look more carefully at the **above implementation**, and **review what advantages we have here:**

We've implemented date string parsing **in one place** and it's **reusable** now.  
Encapsulation works fine here (if you think that you could implement string parsing as a single function elsewhere, this solution fits the OOP paradigm far better).
**cls is an object that holds the class itself, not an instance of the class.** It's pretty cool because if we inherit our Date class, all children will have from_string defined also.

### Static method

What about staticmethod? It's pretty similar to classmethod but** doesn't take any obligatory parameters (like a class method or instance method does). **

Let's look at the next use case.

We have a date string that we want to validate somehow. This task is also logically bound to the Date class we've used so far, but doesn't require instantiation of it.

Here is where staticmethod can be useful. Let's look at the next piece of code:
``` python
    @staticmethod
    def is_date_valid(date_as_string):
        day, month, year = map(int, date_as_string.split('-'))
        return day <= 31 and month <= 12 and year <= 3999
    # usage:
    is_date = Date.is_date_valid('11-09-2012')
```
So, as we can see from usage of staticmethod, we don't have any access to what the class is---it's basically just a function, called syntactically like a method, but without access to the object and its internals (fields and another methods), while classmethod does.
