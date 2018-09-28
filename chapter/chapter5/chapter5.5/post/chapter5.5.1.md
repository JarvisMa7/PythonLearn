# 5.5.1 什么是 venv

假设我们开发程序 A 是用到了 `pip install module1==1.0`，也就是安装了 `module1` 这个第三方库的 1.0 版本，同时开发程序 B 用到了这个第三方库的 2.0 版本，但是在 `/usr/local/lib/python3.5/site-packages` 这个目录下只能留一份，那么 A 和 B 就无法分别使用两个版本的第三方库了。

于是诞生了虚拟环境（virtualenv，简称 venv）的概念，它会为每个应用单独提供一份 Python 的运行环境，从而起到隔离的效果。

执行以下命令：

```shell
pip3 install virtualenv
virtualenv test
```

这首先会安装 `virtualenv` 这个工具模块，然后在当前目录下新建一个虚拟环境 `test`，其实也就是一个目录。
