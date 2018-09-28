# 4.2.5 初始生成器

稍有追求的程序员都难以容忍这么多模板代码（两个 `__iter__`， 一个 `__next__`），好在 Python 的生成器可以简化上述代码：

```python
class MyCollection:
	_private_data = [1, 2, 3]

	def __iter__(self):
		for i in self._private_data:
			yield i
```

从直观上看，这里用 `yield` 关键字替换了 `return`，打破了 “可迭代对象不能实现 `__next__` ” 的规定，但却能够支持多次遍历，这段代码的工作原理会在介绍完生成器以后解释。