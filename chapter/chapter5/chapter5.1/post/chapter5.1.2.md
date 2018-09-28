# 5.1.2 线程锁

像 `a += 1` 这样的语句，是线程不安全的，因为它不是原子性操作。如果想保证某段代码最多同时被一个线程执行，可以给它加锁：

```python
i = 0
lock = threading.Lock()

def think():
	global i
	lock.acquire()
	i += 1
	lock.release()
```

注意到这里的加锁和释放锁又是上下对应的模板代码，这类代码都可以用 `with` 块配合上下文解决。Python 提供了现成的写法：

```python
lock = threading.Lock()

def think():
	global i
	with lock:
		i += 1
```
在 `threading.Lock` 类的 `__exit__`方法中会自动释放锁。
