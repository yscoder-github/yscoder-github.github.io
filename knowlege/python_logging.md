用Python写代码的时候，在想看的地方写个print xx 就能在控制台上显示打印信息，这样子就能知道它是什么了，但是当我需要看大量的地方或者在一个文件中查看的时候，这时候print就不大方便了，所以Python引入了logging模块来**记录**我想要的信息。

print也可以输入日志，logging相对print来说更好**控制输出在哪个地方**，怎么输出及控制消息级别来**过滤掉**那些不需要的信息。

##### 1.日志级别
``` python
import logging  # 引入logging模块
# 将信息打印到控制台上
logging.debug(u"苍井空")
logging.info(u"麻生希")
logging.warning(u"小泽玛利亚")
logging.error(u"桃谷绘里香")
logging.critical(u"泷泽萝拉")
```
打印结果如下：
``` python
WARNING:root:小泽玛利亚
ERROR:root:桃谷绘里香
CRITICAL:root:泷泽萝拉
```
上面可以看到只有后面三个能打印出来

> 默认生成的root logger的level是logging.WARNING,低于该级别的就不输出了

> 级别排序:CRITICAL > ERROR > WARNING > INFO > DEBUG

> debug : 打印全部的日志,详细的信息,通常只出现在诊断问题上

> info : 打印info,warning,error,critical级别的日志,确认一切按预期运行

> warning : 打印warning,error,critical级别的日志,一个迹象表明,一些意想不到的事情发生了,或表明一些问题在不久的将来(例如。磁盘空间低”),这个软件还能按预期工作

> error : 打印error,critical级别的日志,更严重的问题,软件没能执行一些功能

> critical : 打印critical级别,一个严重的错误,这表明程序本身可能无法继续运行

> 这时候，如果需要显示低于WARNING级别的内容，可以引入NOTSET级别来显示


##### 2、部分名词解释

* Logging.Formatter：这个类配置了日志的格式，在里面自定义设置日期和时间，输出日志的时候将会按照设置的格式显示内容。

* Logging.Logger：Logger是Logging模块的主体，进行以下三项工作：
 > 1.  为程序提供记录日志的接口
 > 2. 判断日志所处级别，并判断是否要过滤
 > 3. 根据其日志级别将该条日志分发给不同handler

* 常用函数有：
> Logger.setLevel() 设置日志级别<br/>
> Logger.addHandler() 和 Logger.removeHandler() 添加和删除一个Handler<br/>
> Logger.addFilter() 添加一个Filter,过滤作用 <br/>
> Logging.Handler：Handler基于日志级别对日志进行分发，如设置为WARNING级别的Handler只会处理WARNING及以上级别的日志。<br/>
常用函数有：
setLevel() 设置级别
setFormatter() 设置Formatter
