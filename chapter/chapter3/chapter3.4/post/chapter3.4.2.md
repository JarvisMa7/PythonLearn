# 3.4.2 元类的概念

元类和类工厂函数的区别就像属性描述符和特性工厂函数的区别一样，**前者是类，可以继承**，后者不行。就像我们上一节中用 `type` 来生成类一样，Python 中的类都是 type 类的实例：

```python
'JarvisMa'.__class__  # <class 'str'>，字符串都是 str 类的实例
str.__class__            # <class 'type'> str 类是 type 类的实例
int.__class__            # <class 'type'> int 也是 type 类的实例
type.__class__           # <class 'type'> type 类是自己的实例，防止死循环
```

需要说明的是，`__class__` 表示的是元类，而不是父类，父类可以通过 3.3.2 节中介绍的 `__mro__` 属性来查看：

```python
int.__mro__  # (<class 'int'>, <class 'object'>)
str.__mro__  # (<class 'str'>, <class 'object'>)
```

可见 `int` 和 `str` 这些内置类的父类都是 `objcet`，我们可以认为 `object` 是所有类的父类，而 `type` 是所有类的元类，这个规则在这两个类之间也适用，可以验证一下：

```python
object.__class__   # <class 'type'>
type.__mro__       # (<class 'type'>, <class 'object'>)
```

可见 `object` 是 `type` 类构建出来的实例，`type` 是 `object` 类的元类，而 `object` 则是 `type` 类的父类。如下图所示：

![](https://ws1.sinaimg.cn/large/006tNbRwly1fvhmyks98kj30zk0eywif.jpg)

这里介绍元类和父类并非是为了烧脑，除了描述最基本的概念以外，我们应该意识到，类是由元类的 `__init__` 方法构造出来的实例，如果我们继承元类并且重写 `__init__` 方法，就可以控制类的初始化方法。
