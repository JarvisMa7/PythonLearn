# 4.1.3 try else

仅当 `try` 代码块中没有抛出异常时才会执行 `else`。为了理解 `try ... else` 的使用场景，我们先看一个常见的场景。

一个很常见的错误是为了处理异常，把一大段代码都放在 `try` 语句中：

```python
try:
	# 写了几十行毫无问题的代码
	# ...
	some_dangarous_operation()
	# ...
	# 下面跟了几十行毫无问题的代码
except Exception as e:
	# 处理异常
```

这种写法非常不负责任，`try` 不是防止崩溃的银弹，而是应该用在真正可能导致异常的函数上，所以要保证 `try` 的代码块尽可能简单，突出要尝试执行的代码。以这段代码为例，`some_dangarous_operation` 函数之前的代码可以放在 `try` 代码块上面写，但如果 `some_dangarous_operation` 函数之后的代码依赖于这个函数的正确执行，就不太好独立出来了。这时候就该 `try ... else` 发挥作用了：

```python
# some_dangarous_operation 之前的安全代码写在这里
# ...
try:
	some_dangarous_operation()
except Exception as e:
	# 处理异常
else:
	# ...
	# 依赖于 some_dangarous_operation 的安全代码
```
