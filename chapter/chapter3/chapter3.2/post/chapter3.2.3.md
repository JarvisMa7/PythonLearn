# 3.2.3 特性 property

使用过别的面向对象语言的读者应该都清楚属性的 `getter` 和 `setter`函数的重要性。它们封装了属性的读写操作，可以添加一些额外的逻辑，比如校验新值，返回属性前做一些修饰等等。最简陋的 `getter` 和 `setter`就是两个普通函数：

```python
class Person:
	def get_name(self):
		return self.name.upper()

	def set_name(self, new_name):
		if isinstance(new_name, str):
			self.name = new_name.lower()

	def __init__(self, name):
		self.name = name

bs = Person('JarvisMa')
bs.get_name()   # 得到大写的名字： 'JARVISMA'
bs.set_name(1)  # 由于新的名字不是字符串，所以无法赋值
bs.get_name()   # 还是老的名字： 'JARVISMA'
```

工作虽然完成了，但方法并不高明。在 1.2.3 节中我们就见识到了 Python 的一个特点：“内部高度封装，完全对外透明”。这里手动调用 `getter` 和 `setter` 方法显得有些愚蠢、啰嗦，比如对比下面的两种写法，在变量名和函数名很长的情况下，差距会更大：

```python
bs.name += '1996'
bs.set_name(bs.get_name() + '1996')
```

Python 提供了 `@property` 关键字来装饰 `getter` 和 `setter` 方法，这样的好处是可以直接使用点语法，了解 Objective-C 的读者对这一特性一定倍感亲切：

```python
class Person:
	@property                        # 定义 getter
	def name(self):                  # 函数名就是点语法访问的属性名
		return self._name.upper()    # 现在真正的属性是 _name 了

	@name.setter                     # 定义 setter
	def name(self, new_name):        # 函数名不变
		if isinstance(new_name, str):
			self._name = new_name.lower()  # 把值存到私有属性 _name 里

	def __init__(self, name):
		self.name = name

bs = Person('JarvisMa')
bs.name      # 其实调用了 name 函数，得到大写的名字： 'JARVISMA'
bs.name = 1  # 其实调用了 name 函数，因为类型不符，无法赋值
bs.name      # 还是老的名字： 'JARVISMA'
```

我们已经在 2.4 节详细学习了装饰器，应该能意识到这里的 `@property` 和 `@xxx.setter` 都是装饰器。因此上述写法实际上等价于：

```python
class Person:
	def get_name(self):
		return self._name.upper()

	def set_name(self, new_name):
		if isinstance(new_name, str):
			self._name = new_name.lower()
	# 以上是老旧的 getter 和 setter 定义
	# 如果不用 @property，可以定义一个 property 类的实例
	name = property(get_name, set_name)
```
	
可见，特性的本质是给类创建了一个类属性，它是 property 类的实例，构造方法中需要把 `getter`、`setter` 等函数传入，我们可以打印一下类的 `name` 属性来证明：

```
Person.name  # <property object at 0x107c99868>
```

理解特性的工作原理至关重要。以这里的 `name` 特性为例，我们访问了对象的 `name` 属性，但是它并不存在，所以会尝试访问类的 `name` 属性，这个属性是 `property` 类的实例，会对读写操作做特殊处理。这也意味着，如果我们重写了类的 `name` 属性，那么对象的读写方法就不会生效了：

```python
bs = Person()
Person.name = 'hello'
bs.name  # 实例并没有 name 属性，因此会访问到类的属性 name，现在的值是 'hello` 了
```

如果访问不存在的属性，默认会抛出异常，但如果实现了 `__getattr__` 函数，还有一次挽救的机会：

```python
class Person:
	def __getattr__(self, attr):
		return 0

	def __init__(self, name):
		self.name = name

bs = Person('JarvisMa')
bs.name    # 直接访问属性
bs.age     # 得到 0，这是 __getattr__ 方法提供的默认值
bs.age = 1 # 动态给属性赋值
bs.age     # 得到 1，注意！！！这时候就不会再调用 __getattr__ 方法了
```

由于 `__getattr__` 只是兜底策略，处理一些异常情况，并非每次都能被调用，所以不能把重要的业务逻辑写在这个方法中。
