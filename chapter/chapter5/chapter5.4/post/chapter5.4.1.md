# 5.4.1 Python 中的模块化

Python 不仅可以用来编写短小精悍的脚本文件，也能用来开发大型项目，这就需要把代码合理的写在各个模块中，确保**高内聚、低耦合**。

每一个 Python 文件都是一个模块，我们知道 `import` 关键字可以导入系统模块，也可以用 `import module_name` 的写法导入别的模块。

假设文件路径如下：

```python
package
|-- main.py
|-- module.py
```

那么可以这样引用别的模块

```python
# In module.py
def add(a, b):
	return a + b

# In main.py
import module
modul1.add(1, 2) # 返回 3
```
