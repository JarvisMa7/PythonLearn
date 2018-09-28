# 5.3.1 文件读写

读取文件时，一般会这样写代码：

```python
with open('path_to_file', 'r') as f:
	for line in f:
		print(line)
# first line

# second line

# third line
```

`open` 函数的第二个参数表示打开模式，`r 表示读取，`w`表示写入，会删除原来的所有内容，`a` 表示在文件后面追加写入。

一般来说不要用 `readlines` 读取文件，因为如果文件特别大， 读出来的数组可能会非常占用内存。我们会看到输出结果一般都有多个空行，这是因为每行的结尾都有 '\n' 换行符，而且 print 函数自己就会换行。如果想要更美观的输出，可以用 `replace` 或者 `rstrip` 函数干掉换行符。

如果我们用 `open('path_to_file'. 'rb')` 来打开文件，就可以读取到原来的二进制：

```python
with open('file_to_path', 'rb') as f:
	for line in f:
		print(line)
# b'first line\n'
# b'second line\n'
# b'third line\n'
```

这里我们本来应该看到的是各个字母的 UTF-8 编码后的二进制，不过在打印的时候被系统自动转成字母了。在文件不是UTF-8 编码时，一定要用二进制格式去打开文件并且自行解码，否则 Python 会尝试用 UTF-8 去解码，极有可能会因为无法解码而导致报错。

我们可以试着把二进制写入到文件中：

```python
s = 'hello world'
b = [x.encode('utf16') for x in s]

with open('path_to_file', 'wb') as f:
	f.writelines(b)
```

`writelines` 函数的参数是数组，相当于对数组中的每个元素调用了 `write()` 方法。

感兴趣的读者可以试着分别用 `r` 和 `rb` 去打开文件，感受其中的区别。
