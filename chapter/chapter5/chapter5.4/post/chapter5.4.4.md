# 5.4.4 相对导入和绝对导入

接上面的例子，假设我们在 `module2` 中要引用 module1`，代码如下：

```python
import string.module1
```

这样写没有问题，因为有 `__init__.py` 文件把目录标记为包，所以可以正确识别。然而如果我们的入口不是 `main.py`，而是直接执行 `python3 module2.py`，就无法识别到父目录里面的 `__init__.py` 文件了。此时可以采用相对导入：

```python
from . import module1
```

一个点表示当前的包，两个点就表示上一个包，以此类推……；相对导入只能使用 `from import `的语法，而绝对路径导入则不受限制，两种写法皆可。

相对路径的缺点在于容易丢失命名空间，比较这两种写法：

```python
from foo import bar
bar()     # bar 在哪里定义？
import foo.bar
foo.bar() # 在 foo 模块中定义
```

前者丢失了 `bar` 的来源，而后一种写法不会。

