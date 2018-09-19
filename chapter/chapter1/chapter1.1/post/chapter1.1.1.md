# 1.1.1 列表推导

如果要对数组中的所有内容做一些修改，可以用 for 循环或者 map 函数：


```python
array = [1, 2, 3, 4, 5, 6]
small = []

for n in array:
	if n < 4:
		small.append(n * 2)
print(small) # [2,4,6]
```

比较地道的 python 写法是使用列表推导：


```python
array = [1, 2, 3, 4, 5, 6]
small = [n * 2 for n in array if n < 4]
```

`for in` 可以写两次，类似于嵌套的 for 循环，会得到一个笛卡尔积：


```python
signs = ['+', '-']
numbers = [1, 2]

ascii = ['{sign}{number}'.format(sign=sign, number=number)
		for sign in signs for number in numbers]
# 得到：[‘+1’, ‘+2’, ‘-1’, ‘-2’]
```