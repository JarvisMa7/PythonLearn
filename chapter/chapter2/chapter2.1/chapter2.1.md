# 2.1 函数是一等公民

一等公民指的是 Python 的函数能够动态创建，能赋值给别的变量，能作为参传给函数，也能作为函数的返回值。总而言之，函数和普通变量并没有什么区别。

函数是一等公民，这是函数式编程的基础，然而 Python 中基本上不会使用 lambda 表达式，因为在 lambda 表达式的中仅能使用单纯的表达式，不能赋值，不能使用 while、try 等语句，因此 lambda 表达式要么难以阅读，要么根本无法写出。这极大的限制了 lambda 表达式的使用场景。

上文说过，函数和普通变量没什么区别，但普通变量并不是函数，因为这些变量无法调用。但如果某个类实现了 `__call__` 这个魔术方法，这个类的实例就都可以像函数一样被调用：

```python
class Person:
	def __init__(self):
		self.name = 'JarvisMa'
		self.age = 22
		self.sex = 'm'

	def __call__(self):
		print(self)

	def __str__(self):
		return 'Name: {user.name}, Age: {user.age}, Sex: {user.sex}'.format(user=self)

p = Person()
p() # 等价于 print(p)
```