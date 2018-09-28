# 4.3.3 协程

上一节中介绍的生成器有两个小缺点：

1. 这段代码无法正常退出，因为 `fib` 函数是个死循环，最终会停在第 11 个 yield 上，等待外部的 `next 函数。
2. 现在的数据传递都是单向的，只有生成器给调用方传值，调用方无法给生成器传值。

实际上，生成器函数都是协程，我们可以利用协程的特性解决这两个问题：

```python
fibs = fib()
for i, n in enumerate(fibs):
	if i < 10:
		print(n)
	else:
		fibs.close()
```

只要在遍历完以后调用生成器的 `close` 方法，就可以结束生成器并正确的退出了。如果需要向生成器中传值，需要调用生成器对象的 `send` 方法：

```python
def fib():
	a = 0
	b = 1
	i = 0
	while True:
		i = yield '第{0}个数是: {1}'.format(i, b)
		a, b = b, a + b

fibs = fib()
next(fibs)
for i in range(10):
	print(fibs.send(i))
```

此时，`yield` 表达式左边的值就是 `send` 函数中的参数，而 `send` 函数的返回值则是 `yield` 关键字右边的结果。

协程的用法并不复杂，但它是异步编程的基础，比如 ES 7 中的 async/await 语法，能将异步回调变成同步的写法，它就是依靠协程实现的。

