# 3.2.1 静态函数与类方法

静态函数其实和类的方法没什么关系，它只是恰好定义在类的内部而已，所以这里我用函数（function) 来形容它。它可以没有参数：

```python
class Person:
	@staticmethod   # 用 staticmethod 这个修饰器来表明函数是静态的
	def sayHello():
		print('Hello')

Person.sayHello() # 输出 'Hello`
```

静态函数的调用方式是类名加上函数名。类方法的调用方式也是这样，唯一的不同是需要用 `@staticmethod` 修饰器，而且方法的第一个参数必须是类：

```python
class Person:
	@classmethod    # 用 classmethod 这个修饰器来表明这是一个类方法
	def sayHi(cls):
		print('Hi: ' + cls.__name__)

Person.sayHi() # 输出 'Hi: Person`
```

类方法和静态函数的调用方法一致，在定义时除了修饰器不一样，唯一的区别就是类方法需要多声明一个参数。这样看起来比较麻烦，但静态函数无法引用到类对象，自然就无法访问类的任何属性。

于是问题来了，静态函数有何意义呢？有的人说类名可以提供命名空间的概念，但在我看来这种解释并不成立，因为每个 Python 文件都可以作为模块被别的模块引用，把静态函数从类里抽取出来，定义成全局函数，也是有命名空间的：

```python
# 在 module1.py 文件中：
def global():
	pass 

class Util:
	@staticmethod
	def helper():
		pass

# 在 module2.py 文件中：
import module1
module1.global()        # 调用全局函数
module1.Util.helper()   # 调用静态函数
```

从这个角度看，定义在类中的静态函数不仅不具备命名空间的优点，甚至调用语法还更加啰嗦。对此，我的理解是：**静态函数可以被继承、重写，但全局函数不行**，由于 Python 中的函数是一等公民，因此很多时候用函数替代类都会使代码更加简洁，但缺点就是无法继承，后面还会有更多这样的例子。
