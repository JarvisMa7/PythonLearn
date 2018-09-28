# 5.3.3 执行 Shell

Python 中调用 Shell 命令的方法有很多种，我最常用的是 os.popen 函数，它的返回结果是文件句柄，因此可以调用 readlines 函数：

```python
import os

r = os.popen('pwd')
print(r.readlines())
# 输出 ['/Users/JarvisMa/Desktop\n']
```

这个结果表示 Shell 命令的输出只有一行，且内容是 `/Users/JarvisMa/Desktop`。
