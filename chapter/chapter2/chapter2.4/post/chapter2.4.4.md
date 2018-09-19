# 2.4.4 装饰器工厂

在 2.4.2 节的代码注释中我解释过，装饰器后面不要加括号，被装饰的函数自动作为参数，传递到装饰器函数中。如果加了括号和参数，就变成手动调用装饰器函数了，大多数时候这与预期不符（因为装饰器的参数一般都是被装饰的函数）。

不过装饰器可以接受自定义的参数，然后返回另一个装饰器，这样外面的装饰器实际上就是一个装饰器工厂，可以根据用户的参数，生成不同的装饰器。还是以上面的装饰器为例，我希望输出的内容不是固定的 1，而是用户可以指定的，代码就应该这么写：

```python
import functools

def decorate(content):                        # 这其实是一个装饰器工厂
	def real_decorator(origin_func):          # 这才是刚刚的装饰器
		@functools.wraps(origin_func)
		def new_func():
			print('You said ' + str(content)) # 现在输出内容可以由用户指定
			origin_func()
		return new_func                       # 在装饰器里，返回的是新的函数
	return real_decorator                     # 装饰器工厂返回的是装饰器
```

装饰器工厂和装饰器的区别在于它可以接受参数，返回一个装饰器：

```python
@decorate(2017)
def sayHello():
	print('Hello')
sayHello()
```

其实等价于：

```python
real_decorator = decorate(2017)      # 通过装饰器工厂生成装饰器
new_func = real_decorator(sayHello)  # 正常的装饰器工作逻辑
new_func()                           # 调用的是装饰过的函数
```
