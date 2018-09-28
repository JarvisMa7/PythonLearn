# 5.5.3 工作原理

其实 venv 的工作原理非常简单，完全集中在 `bin/active` 这个简单的脚本中，它的核心部分如下：

```shell
deactive () {
	# 恢复环境
}

VIRTUAL_ENV="/Users/JarvisMa/Desktop/testvenv"
export VIRTUAL_ENV

_OLD_VIRTUAL_PATH="$PATH"
PATH="$VIRTUAL_ENV/bin:$PATH"
export PATH
```

可见它把当前目录标记为 `VIRTUAL_ENV`，然后添加到系统的 `PATH` 最前面，这样我们执行 `pip` 命令时，第三方的包就会被安装虚拟环境内的 `site-packages` 文件夹中，不会与系统的干扰。
