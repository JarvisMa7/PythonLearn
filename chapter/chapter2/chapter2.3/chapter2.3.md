# 2.3 函数内省

### 2.3 函数内省

在 2.2.2 节中，我们查看了函数变量的 `__defaults__` 属性，其实这就是一种内省，也就是在运行时动态的查看变量的信息。

前文说过，函数也是对象，因此函数的变量个数，变量类型都应该有办法获取到，如果你需要开发一个框架，也许会对函数有各种奇葩的检查和校验。

以下面这个函数为例：

```python
g = 1
def foo(m, *args, n, **kwargs):
	a = 1
	b = 2
```

首先可以获取函数名，函数所在模块的全局变量等：

```python
foo.__globals__   # 全局变量，包含了 g = 1
foo.__name__      # foo
```

我们还可以看到函数的参数，函数内部的局部变量：

```python
foo.__code__.co_varnames  # ('m', 'n', 'args', 'kwargs', 'a', 'b')
foo.__code__.co_argcount  # 只计算参数个数，不考虑可变参数和仅限关键字参数，所以得到 1
```

或者用 `inspect` 模块来查看更详细的信息：

```python
import inspect
sig = inspect.signature(foo)  # 获取函数签名

sig.parameters['m'].kind      # POSITIONAL_OR_KEYWORD 表示可以是定位参数或关键字参数
sig.parameters['args'].kind   # VAR_POSITIONAL 定位参数构成的数组
sig.parameters['n'].kind      # KEYWORD_ONLY 仅限关键字参数
sig.parameters['kwargs'].kind # VAR_KEYWORD 关键字参数构成的字典
inspect.getfullargspec(foo)       
# 得到：ArgSpec(args=['m', 'n'], varargs='args', keywords='kwargs', defaults=None)
```

本节的新 API 比较多，但并不要求记住这些 API 的用法。再次强调，本文的写作目的是为了建立读者对 Python 的总体认知，了解 Python **能做什么**，至于怎么做，那是文档该做的事。