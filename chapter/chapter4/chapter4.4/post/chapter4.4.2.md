# 4.4.2 自定义上下文

`with` 块的本质是为了简化 `try finally` 语句，以上一节的代码为例，跟在 `with` 后面的 `open()` 函数会返回一个对象，它是 `TextIOWrapper` 类的实例，我们把它称为上下文管理器，上下文管理器需要实现 __enter__` 和 `__exit__` 方法。

`__enter__` 方法的返回值可以用 `as` 来引用，这里上下文管理器的 `__enter__` 方法的返回值是 `self`，所以以下两种写法中的文件句柄 `fp` 是等价的：

```python
fp = open(file_path)
with open(file_path) as fp
```

第一行中的 `fp` 可以理解为第二行中的上下文管理器，也就是 `with` 后面表达式的返回值。而 `as` 后面的 `fp` 则是上下文管理器的 `__enter__` 方法的返回值，由于这里返回的是 `self`，所以两者恰好相同。

无论以哪种方式退出 `with` 块（正常结束或者因为抛出异常而退出），都会调用上下文管理器的 `__exit__` 方法。注意，这里不是 `__enter__` 方法返回值的 `__exit__` 方法。这一点很好理解，因为前者一定实现了 `__enter__` 方法，但后者不一定。

了解了上下文的概念后，我们可以自定义一个上下文，其实也就是定义一个实现了 `__enter__` 和 `__exit__` 方法的类：

```python
import sys

class Reverse():
	def reverse_write(self, content):
		self.original_write(content[::-1])

	def __enter__(self):
		self.original_write = sys.stdout.write
		sys.stdout.write = self.reverse_write
		return 'Enter context'[::-1]  # 这个字符串会被倒序打印，所以先倒序一次

	def __exit__(self, exc_type, exc_value, traceback):
		sys.stdout.write = self.original_write
		print(exc_type, exc_value, traceback)
		return True

with Reverse() as r:
	print(r)  # 因为字符串已经被倒序过，所以这里输出 'Enter context'
	print('JarvisMa') # 输出 'retfiwstseb'
	raise AttributeError # 输出 <class 'AttributeError'>  <traceback object at 0x10bcb45c8>
	print('will not be print')  # 因为发生了异常，所以不会执行这一行
```

在进入上下文的时候，我们用自定义的方法替换了系统的标准输出，所以上下文中所有的输出都是倒序的。直到上下文结束时才换回来。

`__exit__` 方法有三个参数，分别表示异常的类型，异常的实例，以及发生异常处的调用栈。如果在 `with` 块中发生了异常，异常处后面的代码都不会执行。`__exit__ `方法如果返回 True，表示异常已经被正确处理，否则异常会向上冒泡到 `with` 代码块外面。
