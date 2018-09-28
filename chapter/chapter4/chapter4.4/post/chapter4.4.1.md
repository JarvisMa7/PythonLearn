# 4.4.1 with 块

`with` 代码块的一个常见用法是用于打开文件，有经验的 Python 程序员不会建议你写出这样的代码：

```python
f = open(file_path)
data = f.readlines()
# 处理 data
f.close()
```

用 `with` 块的写法则是：

```python
with open(file_path) as f:
	data = f.readlines()
	# 处理 data
```

这样写的好处不仅仅是不用在最后关闭文件。试想一下，如果在处理文件的过程中有多个地方有可能会抛出异常，那么在所有 `try catch` 语法的最后都要写上 `finally` 以便关闭文件。如果放到 `with` 块中写，则不需要在这么多代码。