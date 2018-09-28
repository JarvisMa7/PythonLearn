# 5.4.3 包与 init.py

当代码量进一步膨胀时，可能多个模块也无法合理的拆分逻辑了，我们可以把实现某个特定功能的若干个模块组合起来，形成一个包。包在逻辑上可以理解为若干个模块的组合， 在物理上讲，包都是文件夹，模块都是文件。

注意，**文件夹不都是包，只有存在 \__init__.py 文件的文件夹才是模块！**

在导入时，我们可以导入类，也可以导入类里面的全局变量或者全局函数，还可以导入包：

```python
import package
import module
```

如果类名和变量名太长，可以用 as 关键字重命名：

```python
import module.var as v
import module.func as f
```

一般来说导入包并没有太大的作用，后续还是需要导入包中的模块。如果确实需要直接用到包中的函数或者变量，可以把它定义在 `__init__.py` 文件中。

`__init__.py` 用于把一个目录标记为包，如果没有这个文件，目录又和 Python 模块重名，就会调用到 Python 的模块，假设文件层级如下：

```python
dir
|-- main.py
|-- string
	|-- module1.py
```

我们尝试在 `main.py` 中导入包：

```python
import string
print(string)
# <module 'string' from '/usr/local/Cellar/python3/3.5.2_3/Frameworks/Python.framework/Versions/3.5/lib/python3.5/string.py'>
```

会发现导入的其实是模块。虽然这里 `string` 目录如果是一个文件，它的查找优先级会高于 Python 中的非内置模块，但如果当做包来导入，Python 就无法识别了。解决方案也很简单，给 `string` 目录添加一个 `__init__.py` 文件，把 `string` 目录标记为包即可。

除了标记目录为包以外，`__init__.py` 文件还可以定义一个 `__all__` 变量，用于批量导入，假设目录层级如下：

```python
dir
|-- main.py
|-- string
	|-- module1.py
	|-- module2.py
	|-- module3.py
	```
	
先在 `__init__` 文件中定义要批量导入的模块：

```python
# In __init__.py
__all__ = ['module1', 'module2']
```

然后在 `main.py` 文件中用星号 * 批量导入：

```python
from string import *
print(module1) # <module 'string.module1' from '/Users/JarvisMa/Desktop/string/module1.py'>
print(module2) # <module 'string.module1' from '/Users/JarvisMa/Desktop/string/module1.py'>
print(module3) # NameError: name 'module3' is not defined
```

在打印 `module3` 的时候会报错，这是因为 `__all__` 变量没有暴露它，需要我们手动导入。
