# 1.1.5 魔术方法

也许你已经注意到了，数组和字符串都支持切片，而且语法高度统一。这在某些强类型语言（比如我经常接触的 Objective-C 和 Java）中是不可能的，事实上，Python 能够支持这样统一的语法，并非巧合，而是因为所有用中括号进行下标访问的操作，其实都是调用这个类的 `__getitem__` 方法。

比如我们完全可以让自己的类也支持通过下标访问：

```python
class Book
	def __init__(self)
		self.chapters = [1, 2, 3]
	def __getitem__(self, n):
		return self.chapters[n]

b = Book()
print(b[1]) # 结果是 2
```

需要注意的是，这段代码几乎不会出问题（除非数组越界），这是因为我们直接把下标传到了内部的 `self.chapters` 数组上。但如果要自己处理下标，需要牢记它不一定是数字，也可以是切片，因此更完整的逻辑应该是：

```python
def __getitem__(self, n):
	if isinstance(n, int): # n是索引
		# 处理索引
	if isinstance(n, slice): # n是切片
		# 通过 n.start，n.stop 和 n.step 来处理切片
```

与静态语言不同的是，任何实现了 `__getitem__`都支持通过下标访问，而不用声明为实现了某个协议，这种特性也被称为 “鸭子类型”。鸭子类型并不要求某个类 是什么，仅仅要求这个类 **能做什么**。

顺便说一句，实现了 `__getitem__` 方法的类都是可迭代的，比如：

```python
b = Book()
for c in b:
	print(c)
```

后续的章节还会介绍更多 Python 中的魔术方法，这种方法的名称前后都有两个下划线，如果读作 “下划线-下划线-getitem” 会比较拗口，因此可以读作 “dunder-getitem” 或者 “双下-getitem”，类似的，我想每个人都能猜到 `__setitem__` 的作用和用法。