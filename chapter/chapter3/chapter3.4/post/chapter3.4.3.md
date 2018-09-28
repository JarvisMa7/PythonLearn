# 3.4.3 元类的使用示例

在介绍属性描述符时，我们用计数器来实现 `storage_name` 的自动生成，从而避免冗余的代码，但代码的可读性会下降，因为属性的名称无法获得，只能用递增的数字来区别。利用元类，我们可以在不影响可读性的前提下，实现存储名称的自动生成：

```python
class Quantity:
	__counter = 0

	def __init__(self):
		cls = self.__class__
		self.storage = '_{}#{}'.format(cls.__name__, cls.__counter)
	def __get__(self, instance, owner):
		return instance.__dict__[self.storage]
	def __set__(self, instance, value):
		if value > 0:
			instance.__dict__[self.storage] = value
		else:
			raise ValueError('value must be > 0')
# Quantity 类是可以自动生成 storage 名称的描述符类，和之前的逻辑基本类似，可以不用关注

# 继承自 type 类，是一个自定义的元类    
class QuantityMeta(type):
	def __init__(self, name, bases, attr_dict): 
		# name 表示类名，bases 是继承关系，attr_dict 则是属性列表，和 type 方法的参数含义一致
		super().__init__(name, bases, attr_dict)
		for key, attr in attr_dict.items():
			# 注意，类有很多属性，但只有描述符类型的属性才需要修改
			if isinstance(attr, Quantity):
				type_name = type(attr).__name__
				# 这里的 key 就是原来的属性名，比如 price、number
				attr.storage = '_{}#{}'.format(type_name, key)

# 为了避免让用户知道太多元类的细节，我们创建一个基类 Entity，并把它的元类设置为 QuantityMeta
class Entity(metaclass=QuantityMeta): pass

# 现在用户的类只要继承自 Entity 就可以了
class Item(Entity):
	price = Quantity()
	number = Quantity()

	def __init__(self):
		pass

i = Item()
i.price = 1
print(i.__dict__)  # 得到 {'_Quantity#price': 1}，可读性良好
#i.price = -1      # 抛出异常
```

虽然代码比较长，但其实核心很简单，在元类的 `__init__` 方法中，我们可以获取将要生成的类的名称、父类和属性，就像在类工厂函数中传给 `type` 类的那些参数一样。有了这些信息，我们可以把当初用计数器生成的临时存储名称给改正为可读性更高的名称。
