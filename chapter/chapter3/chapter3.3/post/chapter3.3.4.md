# 3.3.4 抽象类

有时候我们需要在父类定义一个方法，然后交给子类去实现。这种方法叫做抽象方法，定义了抽象方法的类叫做抽象类。抽象类不应该被实例化，在 Java 中，`Interface` 就是抽象类，它不能被实例化，只有实现了协议的类才能创建实例。

在 Python 中，抽象类需要把自己的 `metaclass` 设置为 `abc.ABCMeta`，并且用装饰器去标记抽象函数：


```python
import abc

class Base:
	pass

class BaseItem(Base, metaclass=abc.ABCMeta):   # 需要标记 metaclass
	@abc.abstractmethod                        # 抽象函数
	def get_price(self):
		 """Method that should do something."""

item = BaseItem()
# Traceback (most recent call last):
# ...
# TypeError: Can't instantiate abstract class BaseItem with abstract methods get_price
```

如果设置了 `metaclass` 并且标记了抽象函数，那么任何没有实现抽象函数的子类（包括抽象类自己）都无法实例化。
