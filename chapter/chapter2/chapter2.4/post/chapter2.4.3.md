# 2.4.3 装饰器进阶

上一节中的装饰器主要是为了介绍工作原理，它的功能非常简单，并不会改变被装饰函数的运行结果，仅仅是在导入时装饰函数，然后输出一些内容。换句话说，即使不执行函数，也要执行装饰器中的 `print` 语句，而且因为直接返回函数的缘故，其实没有真正的起到装饰的效果。

如何做到装饰时不输出任何内容，仅在函数执行最初输出一些东西呢？这是常见的 AOP（面向切片编程） 的需求。这就要求我们不能再直接返回被装饰的函数，而是应该返回一个新的函数，所以新的装饰器需要这么写：

```python
def decorate(origin_func):
	def new_func():
		print(1)
		origin_func()
	return new_func

decorate
def sayHello():
	print('Hello')

sayHello() # 运行结果不变，但是仅在调用函数 sayHello 时才会输出 1
```

这个例子的工作原理是，`sayHello` 函数作为参数 `origin_func` 被传到装饰器中，经过装饰以后，它实际上变成了 `new_func`，会先输出 1 再执行原来的函数，也就是 `sayHello`。

这个例子很简陋，因为我们知道了 `sayHello` 函数没有参数，所以才能定义一个同样没有参数的替代者：`nwe_func`。如果我们在开发一个框架，要求装饰器能对任意函数生效，就需要用到 2.2.3 中介绍的 `*` 和 `**` 这种不定参数语法了。

如果查看 `sayHello` 函数的名字，得到的结果将是 `new_func`：

```python
sayHello.__name__  # new_func
```

这是很自然的，因为本质上其实执行的是：

```python
new_func = decorate(sayHello)
```

而装饰器的返回结果是另一个函数 `new_func`，两者仅仅是运行结果类似，但两个对象并没有什么关联。

所以为了处理不定参数，并且不改变被装饰函数的外观（比如函数名），我们需要做一些细微的修补工作。这些工作都是模板代码，所以 Python 早就提供了封装：

```python
import functools

def decorate(origin_func):
	@functools.wraps(origin_func)  # 这是 Python 内置的装饰器
	def new_func(*args, **kwargs):
		print(1)
		origin_func(*args, **kwargs)
	return new_func
```
