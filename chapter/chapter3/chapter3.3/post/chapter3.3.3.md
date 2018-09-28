# 3.3.3 Mixin

Python 语法直接支持多继承：

```python
class A:
		def foo(self):
				print('A')
		
class B(A):         # 继承自 A， 重写 foo 方法
		def foo(self):
				print('B')
		
class C(A):         # 继承自 A，重写 foo 方法
		def foo(self):
				print('C')
		
class D(B, C):    # 多继承的语法，父类之间用逗号间隔
			pass

d = D()
d.foo()               # 输出：'B' 
```

这就是著名的菱形问题，D 继承自 B 和 C，而 B 和 C 都继承自 A，他们的继承关系构成一个菱形。调用 D 类实例的 `foo` 方法会让人产生疑惑，它的父类们都实现了 `foo` 方法，到底以谁为准？

Python 有一套算法来计算遍历顺序，这个顺序叫做**方法解析顺序（Method Resolution Oder，MRO）**。这个算法叫做 C3 算法，可以参考这篇官方文档：[The Python 2.3 Method Resolution Order](https://www.python.org/download/releases/2.3/mro/)

不过绝大多数情况下，除非你的代码极度依赖多继承，否则都不需要了解这个算法的具体工作原理。一方面，我们可以调用某个特定父类的方法。我们也许已经注意到两个事实，首先类中定义的方法其实都是类的属性，但调用者都是类的实例。其次，类方法的第一个参数都是 `self`，表示方法的调用者，但我们调用时并不需要传入实例。这是因为其实实例方法的正规调用方式是这样的：

```python
C.foo(c())
 # 或者是
C.foo(d)
```

这种调用方式符合定义，而且能够解释上述的两个疑问。然而这种调用方式不仅写起来麻烦，还很不合理，因为我们不仅要实例对象，还需这个实例所属的类才能调用。因此，Python 的做法是将类中定义的方法绑定到每一个实例上：


```python
D.foo # <function B.foo at 0x10696c158> 这个是真正的方法对象
d   # <__main__.D object at 0x1075fa400> 这个是 D 的一个实例
d.foo   #<bound method B.foo of <__main__.D object at 0x106967400>> 注意看地址和 d 是一致的
d.foo.__self__  # <__main.D object at 0x1075fa400，通过 __self__ 引用绑定的实例
```
可以清楚的从 `d.foo` 的输出结果看出来，它是绑定到 `d` 对象上的函数，第一个参数 `self` 就是`d`。

另一方面，我们不仅可以调用任意父类的方法，还可以通过类的 `__mro__` 属性查看父类的继承顺序：


```python
D.__mro__
# (<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)
```

所以，多继承的方法调用顺序一般情况下不会对开发代码造成太大的困扰。
