# 2.2.3 多参数传递

#### 2.2.3 多参数传递

当参数个数不确定时，可以在参数名前加一个 `*`：

```python
def foo(*args):
	print(args)

foo(1, 2, 3)  # 输出 [1, 2, 3]
```

如果直接把数组作为参数传入，它其实是单个参数，如果要把数组中所有元素都作为单独的参数传入，则在数组前面加上 `*`：

```python
a = [1, 2, 3]    
foo(a)  # 会输出 ([1,2,3], )   因为只传了一个数组作为参数
foo(*a) # 输出 [1, 2, 3]
```

这里的单个 `*` 只能接收非关键字参数，也就是仅有参数值的哪些参数。如果想接受关键字参数，需要用 `**` 来表示：

```python
def foo(*args, **kwargs):
	print(args)
	print(kwargs)

foo(1,2,3, a=61, b=62)
# 第一行输出：(1, 2, 3)
# 第二行输出：{'a': 61, 'b': 62}
```

类似的，字典变量传入函数只能作为单个参数，如果要想展开并被 `**kwargs` 识别，需要在字典前面加上两个星号 `**`：

```python
a = [1, 2, 3]
d = {'a': 61, 'b': 62}
foo(*a, **d)
```