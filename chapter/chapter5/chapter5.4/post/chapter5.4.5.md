# 5.4.5 运行 Python 脚本

每个 Python 脚本既可以直接用 `python xxx.py` 命令执行，也可以被别的 Python 文件当做模块引入。对于一个项目来说，入口文件只有一个（就像 C 语言的 `main.c` 文件一样），其他的文件都作为模块对外提供功能。

我们也可以用 `python` 命令执行一个文件夹，此时会自动运行文件夹中的 `__main__.py` 文件。

假设我们写了一个函数，可以爬取给定 URL 的标题：

```python
def getTitle(url):
	# ...
	# return html.title

getTitle('https://baidu.com')
```

直接执行或者在别的模块中导入这个文件都会调用 `getTitle` 函数，然而我们希望的效果是只有直接执行这个文件时才执行函数，被导入时只要提供方法即可。或者可以通过判断全局变量 `__name__` 来完成：

```python
def getTitle(url):
	# ...
	# return html.title

if __name__ == '__main__':
	print(getTitle('https://baidu.com'))
```
	
只有当直接运行文件时，全局变量 `__name__` 的值才是 `__main__`，因此当这个模块被导入时，`if` 语句中的代码就不会被调用。

`python` 作为一个 Shell 命令，可以和其他系统命令通过管道联系在一起，比如实现一个 `show_file.py`：

```python
import sys

for file in sys.stdin:
	print(file)
```
	
然后执行命令行脚本：

```python
ls | python3 show_file.py
```

将会打印出当前目录中所有文件的名字。每个 Shell 脚本都有返回值， 用于表示脚本是否成功，通过命令 `echo $?` 可以查看上一条命令的返回结果，比如：

```python
./create_file.sh
if [ $? = 0 ] ; then  # 只有成功创建文件，才会写入内容
	./write_to_file.sh
else
	echo "error"
fi
```

由于脚本命令的返回值具有重要的参考价值，因此我们的 Python 脚本也要遵循这一规范，这样别的命令可以很容易的知道 Python 脚本的执行情况。因此，标准的入口文件总是应该套用这个模板：

```python
import sys

def main():
	# 执行某些逻辑
	# 如果发生错误
	# sys.exit(1)
	return 0

if __name__ == '__main__':
	sys.exit(main())
```
