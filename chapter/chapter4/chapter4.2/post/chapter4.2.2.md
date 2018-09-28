# 4.2.2 可迭代对象

用 `next` 函数去迭代一个迭代器对象，不仅语法繁琐，每次还要用 `try catch` 来处理随时都有可能发生的 `StopIteration` 异常，这种写法实在是太啰嗦了。所以我们平时都用 `for in` 语法来遍历字符串、数组等可迭代对象：

```python
for c in 'JarvisMa':
	print(c)
这种写法其实是对迭代器的封装：

it = iter('JarvisMa')
while True:
	try:
		print(next(it))
	except StopIteration:
		del it
		break
```
可见 `for in` 语法省略了大量的模板代码。可以看到这里的字符串是可迭代对象，在用 `for in` 遍历时，其实是通过 `iter` 函数获取了可迭代对象的迭代器，然后用 `next` 函数去遍历这个迭代器，这揭示了迭代器和可迭代对象之间重要的关系：**Python 用 iter 函数从可迭代对象中获取迭代器。**
