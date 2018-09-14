# 1.1.4 循环与遍历

一般来说，在 Python 中我们不会写出 `for(int i = 0; i < len(array); ++i)` 这种风格的代码，而是用 `for in` 这种语法：

```python
for i in[1, 2, 3]
	print(i)
```

虽然大家都知道 `for in` 语法，但它的某些灵活用法或许就不是那么众所周知了。有时间，我们会在 `if` 语句中对某个变量的值做多次判断，只要满足一个条件即可：

```python
name = 'bs'
if name == 'hello' or name == 'hi' or name == 'bs' or name =='admin':
	print('Volid')
``` 

这种情况推荐用 `in` 来代替：

```python
name = 'bs'
if name in ('hello', 'hi', 'bs', 'admin'):
	print('Volid')
```

有时候，如果我们想要把某件事重复固定的次数，用 `for in` 会显得有些啰嗦，这时候可以借助 `range` 类型：

```python
for i in range(5)
	print('Hi') # 打印五次 'Hi'
```

`range` 的语法和切片类似，比如我们需要访问数组所有基数下标的元素，可以这么写:

```python
a = [1, 2, 3, 4, 5]
for i in range(0, len(a), 2):
	print(a[i])
```

这种写法中，我们不仅能获得元素，还能知道元素的下标，这与使用 `enumerate(iterable [, start ])` 函数类似：

```python
a = [1, 2, 3, 4, 5]
for i, n in enumerate(a):
	print(i, n)
```
