# 3.2.2 属性 attribute

Python （等多数动态语言）中的类并不像 C/OC/Java 这些静态语言一样，需要预先定义属性。我们可以直接在初始化函数中创建属性：

```python
class Person:
	def __init__(self, name):
		self.name = name

bs = Person('JarvisMa')
bs.name  # 值是 'JarvisMa'
```

由于 `__init__` 函数是运行时调用的，所以我们可以直接给对象添加属性：

```python
bs.age = 22
bs.age  # 因为刚刚赋值了，所以现在取到的值是 22
```

如果访问一个不存在的属性，将会抛出异常。从以上特性来看，对象其实和字典非常相似，但这种过于灵活的特性其实蕴含了潜在的风险。比如某个封装好的父类中定义了许多属性， 但是子类的使用者并不一定清楚这一点，他们很可能会不小心就重写了父类的属性。一种隐藏并保护属性的方式是在属性前面加上两个下划线：

```python
class Person:
	def __init__(self):
		self.__name = 'JarvisMa'

bs = Person()

bs.__name          # 这样是无法获取属性的
bs._Person__name   # 这样还是可以读取属性
```

这是因为 Python 会自动处理以双下划线开头的属性，把他们重名为 `_Classname__attrname` 的格式。由于 Python 对象的所有属性都保存在实例的 `__dict__` 属性中，我们可以验证一下：

```python
bs = Person()
bs.__dict__ 
# 得到 {'_Person__name': 'JarvisMa'}
```

但很多人并不认可通过名称改写（name mangling) 的方式来存储私有属性，原因很简单，只要知道改写规则，依然**很容易**的就能读写私有属性。与其自欺欺人，不如采用更简单，更通用的方法，比如给私有属性前面加上单个下划线 `_`。

注意，以单个下划线开头的属性不会触发任何操作，完全靠自觉与共识。**任何稍有追求的 Python 程序员，都不应该读写这些属性**。
