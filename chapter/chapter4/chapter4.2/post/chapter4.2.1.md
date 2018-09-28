# 4.2.1 迭代器

迭代器接口定义了两个方法，`__next__` 方法没有参数，用于返回序列的下一个元素，如果没有元素就抛出 `StopIteration` 异常，`__iter__` 方法返回自己。

根据鸭子类型的定义，一个类不用声明为迭代器，只要它实现了迭代器接口中定义的两个方法，就可以迭代：

```python
class MyIterator:
	index = 0
	def __next__(self):
		if self.index > 2:
			raise  StopIteration
		else:
			self.index += 1
			return self.index
	def __iter__():
		return self

i = MyIterator()
next(i) # 得到 1，i 的 index 为 1
next(i) # 得到 1，i 的 index 为 2
next(i) # 得到 1，i 的 index 为 3
next(i) # 根据 if 判断的条件，抛出 StopIteration 异常，迭代结束
```

`next` 函数的参数是迭代器，用于获取迭代器中的下一个元素。
