# 5.4.2 模块查找顺序

对于被 `import` 的模块，Python 首先会检查它是不是内置的模块，比如我们把刚刚的 `module.py` 文件重命名为 `time.py`，再引用：

```python
import time
time.add(1, 2)
# 报错：AttributeError: module 'time' has no attribute 'add'
```

这是因为 Python 最先查找内置的模块，我们打印 time 模块就可以看到 `<module 'time' (built-in)>` ，说明这是一个内置模块。PS：`string` 模块不是内置模块，坑了我一晚上。

如果导入的不是内置模块，Python 会依次在 `sys.path` 这个数组中的每个路径中寻找。按照查找优先级，它由三个部分组成：

1. Python 执行的入口文件（比如这里的 `main.py`）所在的路径
2. 系统的环境变量 $PYTHONPATH 所表示的目录
3. `site` 路径，也就是 `/usr/local/lib/python3.5/site-packages` 这种。

如果我们把 `time.py` 改名为 `string.py` 就会得到正常结果，这是因为它属于第一部分，而 Python 默认的 `string` 模块位于第三部分，优先级比较低。

一般来说，Python 工程中的文件都能在第一部分被找到，而 `pip`安装的第三方库位于第三部分。环境变量 `PYTHONPATH` 一般都是空，但不排除某些 IDE，比如 PyCharm 会修改它。这种行为很危险，因为能在 PyCharm 中编译通过很可能是借助环境变量才找到了模块，一旦迁移到别的环境就无法编译，我似乎遇到过这个坑，将本地可以运行的代码上传到 VPS 以后就找不到模块了。

