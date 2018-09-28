# 4.2.4 标准迭代器

有了上述知识作为铺垫，我们来尝试实现一个定义的可迭代对象。它只需要实现一个 `__iter__ `方法，返回迭代器即可，一个常见的写法如下：

```python
class MyCollection:
	count = 0
	_private_data = [1, 2, 3]

	def __iter__(self):
		return self

	def __next__(self):
		if self.count < len(self._private_data):
			temp = self._private_data[self.count]
			self.count += 1
			return temp
		else:
			raise StopIteration

c = MyCollection()
for i in c:
	print(i) # 输出三行，分别是 1，2 和 3
```
这段代码中，`__iter__` 函数返回了自己，并且自己实现了迭代器的接口，一切运行正常。

并且这段代码向我们展示了迭代器的第一个特点：**屏蔽内部的存取细节， 对外提供统一的访问逻辑。**

很可惜的是，这段代码是**标标准准的错误写法**，因为可迭代对象的迭代器一定不能是自己，或者说可迭代对象一定不能实现 `__next__` 方法，理由很简单，看一眼 4.2.1 节中的迭代器，它是一次性的，遍历完以后就回不去了。这里也是同理，如果我们再执行一次 for in，就得不到输出了。换个角度思考，上一节的结论告诉我们，同时实现了 `__iter__` 和 `__iter__` 方法的是迭代器，而迭代器是不能用于 `for in` 语句的。

正确的写法如下：

```python
class MyCollection:
	_private_data = [1, 2, 3]

	def __iter__(self):
		return MyCollectionIterator(self._private_data)

class MyCollectionIterator:
	count = 0
	def __init__(self, data):
		self.data = data

	def __iter__(self):
		return self

	def __next__(self):
		if self.count < len(self.data):
			temp = self.data[self.count]
			self.count += 1
			return temp
		else:
			raise StopIteration

for i in MyCollection():
	print(i)
```

代码很长，但思路很简单，就是遵守迭代器和可迭代对象的定义，把一次性的迭代工作交给可以重复创建实例的 `MyCollectionIterator` 类完成。

这也正是迭代器模式的另一个特点，**对象能够正确保存多次迭代的进度，支持多次迭代。**
