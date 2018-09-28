# 5.5.2 venv 结构初探

观察目录的组成可以发现，主要是有三个文件夹：`bin`、`include` 和 `lib`。

`bin` 目录下主要是一些可执行文件，比如虚拟环境的激活与退出，以及 Python 和 pip 的可执行文件。比如：

```shell
source bin/active
# 现在开始，虚拟环境已经生效，安装的模块都在这个文件夹内部
pip instal module1
pip instal module2
# ...
deactive
```
`inlcude` 目录下的会通过软连接，导入一些 C 语言的头文件，暂时不清楚作用。

`lib` 目录下引用了 Python 自带的一些模块，以及第三方包 `site-packages` 文件夹的拷贝。如果执行的是 `virtualenv --no-site-packages test` 将会得到一个不含第三方包，纯净的虚拟环境。
