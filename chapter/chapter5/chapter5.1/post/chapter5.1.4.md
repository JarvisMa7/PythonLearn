# 5.1.4 多进程

Python 中实现多进程非常简单，因为接口与多线程基本一致：

```python
import multiprocessing
import os

def run_proc(i):
	print(i)

for i in range(5):
	p = multiprocessing.Process(target=run_proc, args=(i,))
	p.start()
print('End')
```

多进程中没有锁的概念，因为不同的线程可以共享进程的堆，而不同的进程就没有应用层面可以共享的内容了，只能依赖于操作系统提供的 API，比如共享内存、socket、管道、消息队列等。

以消息队列为例，简单展示下进程间共享数据：

```python
from multiprocessing import Process, Queue
import os

i = 0

def run_proc(q):
	for t in range(10):
		i = q.get()
		i += 1
		q.put(i)
		print(i)

q = Queue()
q.put(0)

p1 = Process(target=run_proc, args=(q,))
p1.start()

p2 = Process(target=run_proc, args=(q,))
p2.start()
```

每个进程都会从消息队列中读取变量，加一后放回队列，所以输出结果是 1 到 20。如果不用消息队列来共享，每个进程的变量 i 都是独立的，会输出两次 1 到 10。
