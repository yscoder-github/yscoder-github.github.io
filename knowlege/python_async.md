##### Python 中的异步编程：Asyncio
[Python 中的异步编程：Asyncio](http://python.jobbole.com/87988/?utm_source=blog.jobbole.com&utm_medium=relatedPosts)
> 注：哪怕连异动范式的存在都不知道的情况下，你也可以成功地使用 Python。但是，如果你对底层运行模式感兴趣的话，asyncio 绝对值得查看。

##### 异步是怎么一回事?
在传统的顺序编程中， 所有发送给解释器的指令会一条条被执行。此类代码的输出容易显现和预测。 但是…

譬如说你有一个脚本向3个不同服务器请求数据。 有时，谁知什么原因，发送给其中一个服务器的请求可能意外地执行了很长时间。想象一下从第二个服务器获取数据用了10秒钟。在你等待的时候，整个脚本实际上什么也没干。如果你可以写一个脚本可以不去等待第二个请求而是仅仅跳过它，然后开始执行第三个请求，然后回到第二个请求，执行之前离开的位置会怎么样呢。就是这样。你通过切换任务最小化了空转时间。尽管如此，当你需要一个几乎没有I/O的简单脚本时，你不想用异步代码。

还有一件重要的事情要提，所有代码在一个线程中运行。所以如果你想让程序的一部分在后台执行同时干一些其他事情，那是不可能的。

##### 准备开始
这是 asyncio 主概念最基本的定义:

**协程** — **消费数据**的**生成器**，但是**不生成数据**。Python 2.5 介绍了一种新的语法让发送数据到生成器成为可能。我推荐查阅David Beazley “A Curious Course on Coroutines and Concurrency” 关于协程的详细介绍。

**任务** — 协程调度器。如果你观察下面的代码，你会发现它只是让 event_loop 尽快调用它的_step ，同时 _step 只是调用协程的下一步。

``` Python
class Task(futures.Future):  
    def __init__(self, coro, loop=None):
        super().__init__(loop=loop)
        ...
        self._loop.call_soon(self._step)

    def _step(self):
            ...
        try:
            ...
            result = next(self._coro)
        except StopIteration as exc:
            self.set_result(exc.value)
        except BaseException as exc:
            self.set_exception(exc)
            raise
        else:
            ...
            self._loop.call_soon(self._step)
```

**事件循环** — 把它想成 asyncio 的中心执行器。
现在我们看一下所有这些如何融为一体。正如我之前提到的，异步代码在一个线程中运行。
