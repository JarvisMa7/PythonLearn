# 4.1.1 for else

for 语句的末尾可以加上 else，仅当 for 循环没有因为 break 而终止，顺利运行完以后才运行。这个用法看起来怪怪的，毕竟其他 for 循环后面的代码，也会正常执行。所以这个规则应该反过来理解：**如果 for 循环因为 break 而终止，else 代码块就不会执行。**

有过一些编程经验的读者应该经常会写出这样的代码：

```python
found = False
for i in array:
	if some_judge(i):  # 只要找到一个满足条件的，就把 found 置为 True
		found = True
		break
if not found:          # 如果全都不符合条件，执行某个逻辑
	print('Nothing found')
```
如果用 else 语句，代码就会简化：

```python
for i in array:
	if some_judge(i):
		break
else:
	print('Nothing found')
```

如果 `if` 判断成立了，就会进入 `break`，于是 `else` 的代码就不会执行，否则就会输出 Nothing found。和上面的例子相比，使用 `else` 的代码更简洁，而且不需要再用一个变量来标记了。
