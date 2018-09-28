# 3.2.5 属性描述符

文件描述符的作用和特性工厂一样，都是为了批量的应用特性。它的写法也和特性工厂非常类似：

```python
class Quantity:
	def __init__(self, storage_name):
		self.storage = storage_name
	def __get__(self, instance, owner):
		return instance.__dict__[self.storage]
	def __set__(self, instance, value):
		if value > 0:
			instance.__dict__[self.storage] = value
		else:
			raise ValueError('value must be > 0')
```

主要有以下几个改动：

1. 不用返回 `property` 类的实例了，因此 `getter` 和 `setter` 方法的名字是固定的，这样才能满足协议。
2. `__get__` 方法的第一个参数是描述符类 `Quantity` 的实例，第二个参数 `self` 是要读取属性的实例，比如上面的 `i`，也被称作托管实例。第三个参数是托管类，也就是 `Item`。
3. `__set__` 方法的前两个参数含义类似，第三个则是要读取的属性名，比如 `price`。

和特性工厂类似，属性描述符也可以实现 `storage_name` 的自动生成，这里就不重复代码了。看起来属性描述符和特性工厂几乎一样，但由于属性描述符是类，它就可以继承。比如这里的 `Quantity` 描述符有两个功能：自动存储和值的校验。自动存储是一个非常通用的逻辑，而值的校验是可变的业务逻辑，所以我们可以先定义一个 `AutoStorage` 描述符来实现自动存储功能，然后留下一个空的 `validate` 函数交给子类去重写。

而特性工厂作为函数，自然就没有上述功能，这两者的区别类似于 3.2.1 节中介绍的静态函数与全局函数的区别。
