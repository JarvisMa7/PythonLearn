# 4.4.3 标准库中的上下文

使用标准库中的装饰器可以节省一些模板代码，`__enter__` 和 `__exit__` 方法可以写在一起，以 `yield` 为分界线：

```python
@contextlib.contextmanager
def Reverse():
	def reverse_write(content):
		original_write(content[::-1])

	original_write = sys.stdout.write
	sys.stdout.write = reverse_write
	yield 'Enter context'[::-1] # 下一行开始，是 __exit__ 的逻辑
	sys.stdout.write = original_write
```

`@contextlib.contextmanager` 的缺点是无法通过 `return True/False` 来控制是否需要冒泡异常，必须把 `yield` 代码放到 `try catch` 中。

## 5. 其他 Python 特色
### 5.1 多线程与GIL
#### 5.1.1 多线程

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
