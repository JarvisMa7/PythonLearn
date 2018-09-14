# 1.3.3 字符串格式化

最初级的字符串格式化方法是使用 `+` 来拼接：

```python
class Person:
	def __init__(self):
		self.name = 'JarvisMa'
		self.age = 22
		self.sex = 'm'

p = Person()
print('Name: ' + p.name + ', Age: ' + str(p.age) + ', Sex: ' + p.sex)
# 输出：Name: JarvisMa, Age: 22, Sex: m
```

这里必须要把 `int` 类型的年龄转成字符串以后才能进行拼接，这是因为 Python 是强类型语言，不支持类型的隐式转换。

这种做法的缺点在于如果输出结构比较复杂，极容易出现引号匹配错误的问题，可读性非常低。

Python 2 中的做法是使用占位符，类似于 C 语言中 `printf`：

```python
content = 'Name: %s, Age: %i, Sex: %c' % (p.name, p.age, p.sex)
print(content)
```

从结构上看，要比上一种写法清楚得多， 但每个变量都需要指定类型，这和 Python 的简洁不符。实际上每个对象都可以通过 `str()` 函数转换成字符串，这个函数的背后是 `__str__` 魔术方法。

Python 3 中的写法是使用 `format` 函数，比如我们来实现一下`__str__ `方法：

```python
class Person:
	def __init__(self):
		self.name = 'JarvisMa'
		self.age = 22
		self.sex = 'm'
	def __str__(self):
		return 'Name: {user.name}, Age: {user.age}, Sex: {user.sex}'.format(user=self)

p = Person()
print(p)
# 输出：Name: JarvisMa, Age: 22, Sex: m
```

除了把对象传给 `format` 函数并在字符串中展开以外， 也可以传入多个参数，并且通过下标访问他们：

```python
print('{0}, {1}, {0}'.format(1, 2))
# 输出：1, 2, 1，这里的 {1} 表示第二个参数
```
