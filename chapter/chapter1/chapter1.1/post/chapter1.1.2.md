# 1.1.2 元组

元组可以简单的理解为不可变的数组，也就是没有 `append`、`del` 等方法，一旦创建，就无法新增或删除，元素自身的值也不能改变，但元素内部的属性是否可变并不受元组的影响，这一点符合其他语言中的常识。

```python
t =  (1, [])
t[0] = 3 # 抛出错误 TypeError：'tuple' object does not support item assignment
t[1].append(2) # 正常运行，现在的 t 是 (1, [2])
```

除了不可变性以外，有时候元组也会被当作不具名的数据结构，这时候元素的位置就不再是可有可无的了：


```python
coordinate = (33.9425, -118.408056)
# coordinate 的第一个位置用来表示精度，第二个位置标书纬度
```

在解析元组数据时，可以一一对应的写上变量名：

```python
t = (1, 2)
a, b = t #a = 1, b = 2
```

有时候变量名比较长，但我只关心其中某一个，可以这样写：

```python
t = (1, 2)
a, _ = t # a = 1
```

如果元组中元素特别多，即使挨个写下划线也比较累，可以用\* 来批量解包：

```python
t = (1, 2, 3, 4, 5)
first, *middle, last = t
# first = 1
# middle = [2, 3, 4]
# last = 5
```

当然，如果元素数量较多，含义较复杂，我还是建议使用具名元组：

```python
import collections

People = collections.namedtuple('People', ['name', 'age'])
p = People('JarvisMa', '22')
p.name #JarvisMa
```

具名数组更像一个不能定义方法的简化版的类，能提供友好的数据展示。

元组的一个小技巧是可以避免用临时变量来交换两个数的值：

```python
a = 1
b = 2
a, b = b, a
# a = 2, b = 1
```
