# 1.3.2 字符串的常用方法

字符串的 `split(sep, maxsplit)` 方法可以以指定的分隔符进行分割，有点类似于 Shell 中的 `awk -F ' '`'，第一个 `sep` 参数表示分隔符，不填则为空格：

```python
s = 'a b c d e'
a = s.split()
# a = ['a', 'b', 'c', 'd', 'e']
```

第二个参数 `maxsplit` 表示最多分割多少次，因此返回数组的长度是 `maxsplit + 1`。举个例子说明下：

```python
s = 'a;b;c;d;e'
a = s.split(';')
# a = ['a', 'b', 'c', 'd', 'e']

b = s.split(';', 2)
# b = ['a', 'b', 'c;d;e']
```

如果想批量替换，则可以用 `replace(old, new[, count])` 方法，由中括号括起来的参数表示选填。

```python
old = 'a;b;c;d;e'
new = old.replace(';', ' ', 3)
# new = 'a b c d;e'
```
`strip[chars]` 用于移除指定的字符们：

```python
old = "*****!!!Hello!!!*****"
new = old.strip('*')  # 得到 '!!!Hello!!!'
new = old.strip('*！')  # 得到 'Hello'
```

如果不传参数，则默认移除空格。其实 `strip` 等价于分别执行 `lstrip()` 和 `rstrip()`，即分别从左侧和右侧进行移除。比如 `lstrip()` 表示从左侧第一个字符开始，移除空格，直到第一个非空格字符为止，所以字符串中间的空格，无论是 `lstrip` 还是 `strip()` 都是无法移除的。

```python
old = '  Hello world  '
new = old.strip()   # 得到 'Hello wrold'
new = old.lstrip()  # 得到 'Hello world  '
```

最后一个常用方法是 `join`，其实这个可以理解为字符串的构造方法，它可以把数组转换成字符串：

```python
array = 'a b c d e'.split() # 之前说过，结果是 ['a', 'b', 'c', 'd', 'e']
s = ';'.join(array) # 以分号为连接符，把数组中的元素连接起来
# s = 'a;b;c;d;e'
```

所以 `join` 可以理解为 `split` 的逆操作，这个公式始终是成立的：

> c.join(string.split(c)) = string

上面这些字符串处理的函数，大多返回的还是字符串，因此可以链式调用，避免使用临时变量和多行代码，但也要避免过长（超过 3 个）的链式调用，以免影响可读性。