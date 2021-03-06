# 4.2.3  iter 方法

能够用 `for in` 语法遍历的对象必须是可迭代的，除了内置的数组、元组等类型外，自定义的类型也有办法变成可迭代的，因为 `iter` 函数最终会调用对象的 `__iter` 方法。我们只要能实现这个方法，返回适当的迭代器，就可以让对象变成可迭代的，并支持 `for in` 语法：

```python
class Foo:
	def __iter__(self):
		return iter([1, 2, 3])

for i in Foo():
	print(i) # 共三行输出，分别是 1、2 和 3
```

没实现 `__iter__` 方法，但是实现了 `__getitem__` 方法的对象也是可迭代的，这个函数在 1.1.5 节中已经介绍过，用来处理下标访问。Python 会创建一个迭代器，并且用从 0 开始的整数调用 `__getitem__` 方法作为迭代器的 next 值。如果实现了 `__len__` 是最好，Python 解释器只会调用指定次数的 `__getitem__`，否则会在越界时自动停止。

再次总结下，可迭代对象和迭代器是两个概念，写在 `for in` 中的是可迭代对象，它需要实现 `__iter__` 方法为 `iter` 方法提供一个迭代器。迭代器需要满足迭代器接口， 也就是两个函数。无参数的 `__next__` 方法提供下一个元素或者抛出异常，`__iter__` 函数返回自己。从这个角度看，迭代器都是可迭代对象。
