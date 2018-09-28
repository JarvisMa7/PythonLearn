# 5.1.1 多线程

多线程操作一般通过 `threading` 模块来完成，启动一个线程其实就是把线程要执行的函数传递到 Thread 类的初始化方法中，然后调用 `Thread` 实例的 `start` 方法：

```python
import threading
from time import sleep

def think():
	print('start thinking in thread: ' + threading.current_thread().name)
	sleep(1)
	print('end thinking')

for i in range(3):
	t = threading.Thread(target = think)
	t.start()
print('Exit')
# start thinking in thread: Thread-1
# start thinking in thread: Thread-2
# start thinking in thread: Thread-3
# Exit
# end thinking
# end thinking
# end thinking
```

从输出结果来看，Python 的线程都是异步执行，如果要同步执行某个线程，需要调用线程的 `join` 方法，表示阻塞当前线程，直到整个线程退出为止：

```python
for i in range(3):
	t = threading.Thread(target = think)
	t.start()
	t.join()
print('Exit')
# start thinking in thread: Thread-1
# end thinking
# start thinking in thread: Thread-2
# end thinking
# start thinking in thread: Thread-3
# end thinking
# Exit
```