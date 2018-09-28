# 3.4.1 类工厂函数

有些类的功能很单一，仅仅用来存储数据。但如果先声明一个长长的 `__init__` 函数，再挨个写 `self.xxx = xxx` 这种模板代码，就显得很啰嗦。其实也可以自己实现一个 1.1.2 节中的具名元组。具名元zu组是一个函数，它返回一个类：

```python
People = collections.namedtuple('People', ['name', 'age'])
p = People('JarvisMa', '22')
```

`namedtuple` 这种函数可以称为类工厂函数，因为它可以根据传入的参数，动态的生成类，我们来实现一个简化版的：

```python
def my_tuple(cls, names):
	names = names.split(' ')   # 这里会得到属性的数组
	def __init__(self, *args):
		for (name, value) in zip(self.__slots__, args):
			# slots 是属性名，args 是初始化的参数，一一对应起来用 setattr 给实例的属性赋值
			setattr(self, name, value)  

	cls_attrs = dict(__slots__ = names, __init__ = __init__)
	return type(cls, (object, ), cls_attrs)

People = my_tuple('People', 'name age')
p = People('JarvisMa', '22')
p.name   # 'JarvisMa'
p.age    # 22
```

这个类工厂非常简陋，比如不支持关键字参数，但用来演示类工厂的原理是已经足够了。类工厂的核心原理在于 `type` 函数，它不仅可以传入一个实例，返回实例的类型，也可以像这里的使用一样，传入三个参数，构造一个类。第一个参数表示类名，第二个参数是继承关系，最后一个则是类的属性。
