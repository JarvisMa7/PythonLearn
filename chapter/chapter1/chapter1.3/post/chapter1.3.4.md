# 1.3.4 HereDoc

Heredoc 不是 Python 特有的概念， 命令行和各种脚本中都会见到，它表示一种所见即所得的文本。

假设我们在写一个 HTML 的模板，绝大多数字符串都是常量，只有有限的几个地方会用变量去替换，那这个字符串该如何表示呢？一种写法是直接用单引号去定义：

```python
s = '<HTML><HEAD><TITLE>\nFriends CGI Demo</TITLE></HEAD>\n<BODY><H3>ERROR</H3>\n<B>%s</B><P>\n<FORM><INPUT TYPE=button VALUE=Back\nONCLICK=\'window.history.back()\'></FORM>\n</BODY></HTML>'
```

这段代码是自动生成的还好，如果是手动维护的，那么可读性就非常差，因为换行符和转义后的引号增加了理解的难度。如果用 heredoc 来写，就非常简单了：

```python
s = '''<HTML><HEAD><TITLE>
Friends CGI Demo</TITLE></HEAD>
<BODY><H3>ERROR</H3>
<B>%s</B><P>
<FORM><INPUT TYPE=button VALUE=Back
ONCLICK='window.history.back()'></FORM>
</BODY></HTML>
'''
```

Heredoc 主要是用来书写大段的字符串常量，比如 HTML 模板，SQL语句等等。