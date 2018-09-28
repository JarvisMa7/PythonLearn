# 3.2.4 特性工厂

在上一节中，我们利用特性来封装 `getter` 和 `setter`，对外暴露统一的读写接口。但有些 `getter` 和 `setter` 的逻辑其实是可以复用的，比如商品的价格和剩余数量在赋值时，都必须是大于 0 的数字。这时候如果每次都要写一遍 `setter`，代码就显得很冗余，所以我们需要一个能批量生产特性的函数。由于我们已经知道了特性是 `property` 类的实例，而且是类的属性，所以代码可以这样写：

```python
def quantity(storage_name):  # 定义 getter 和 setter
	def qty_getter(instance):
		return instance.__dict__[storage_name]
	def qty_setter(instance, value):
		if value > 0:
			# 把值保存在实例的 __dict__ 字典中
			instance.__dict__[storage_name] = value 
		else:
			raise ValueError('value must be > 0')
	return property(qty_getter, qty_setter) # 返回 property 的实例
	```
	
有了这个特性工厂，我们可以这样来定义特性：

```python
class Item:
	price = quantity('price')
	number = quantity('number')

	def __init__(self):
		pass

i = Item()
i.price = -1 
# Traceback (most recent call last):
# ...
# ValueError: value must be > 0
```

作为追求简洁的程序员，我们不禁会问，在 `price = quantity('price')` 这行代码中，属性名重复了两次，能不能在 `quantity` 函数中自动读取左边的属性名呢，这样代码就可以简化成 `price = quantity()` 了。

答案显然是否定的，因为右边的函数先被调用，然后才能把结果赋值给左边的变量。不过我们可以采用迂回策略，变相的实现上面的需求：

```python
def quantity():
	try:
		quantity.count += 1
	except AttributeError:
		quantity.count = 0

	storage_name = '_{}:{}'.format('quantity', quantity.count)  

	def qty_getter(instance):
		return instance.__dict__[storage_name]
	def qty_setter(instance, value):
		if value > 0:
			instance.__dict__[storage_name] = value
		else:
			raise ValueError('value must be > 0')
	return property(qty_getter, qty_setter)
	```
	
这段代码中我们利用了两个技巧。首先函数是一等公民， 所以函数也是对象，自然就有属性。所以我们利用 `try ... except` 很容易的就给函数工厂添加了一个计数器对象 count，它每次调用都会增加，然后再拼接成存储时用的键 `storage_name` ，并且可以保证不同 property 实例的存储键名各不相同。

其次，`storage_name` 在 `getter` 和 `setter` 函数中都被引用到，而这两个函数又被 property 的实例引用，所以 `storage_name` 会因为被持有而延长生命周期。这也正是闭包的一大特性：能够捕获自由变量并延长它的生命周期和作用域。

我们来验证一下：

```pyyhon
class Item:
	price = quantity()
	number = quantity()

	def __init__(self):
		pass

i = Item()
i.price = 1
i.number = 2
i.price     # 得到 1，可以正常访问
i.number    # 得到 2，可以正常访问
i.__dict__  # {'_quantity:0': 1, '_quantity:1': 2}
```

可见现在存储的键名可以被正确地自动生成。
