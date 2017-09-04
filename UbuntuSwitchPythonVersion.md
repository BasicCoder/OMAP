# Ubuntu Switch Python Version
## 一.查看所有 Python 安装版本
Ubuntu系统可能为你提供多个可用的 `Python` 版本，因此系统中会存在多个 `python` 的可执行二进制文件。可以使用 `ls` 命令来查看系统中有哪些Python二进制文件可供使用。

	1.> # ls /usr/bin/python*
	2.> /usr/bin/python2.7  /usr/bin/python3  /usr/bin/python3.5  /usr/bin/python3.5m  /usr/bin/python3m

## 二.系统中修改 Python 版本
使用 `update-alternatives` 命令来更改系统 `python` 版本,
首先罗列所有可用的 `python` 替代版本信息：

	1.> # update-alternatives --list python
	2.> update-alternatives: error: no alternatives for python

如果出现以上错误信息，则表示 Python 的替代版本尚未被 `update-alternatives` 命令识别。
我们更新一下代替列表，将 python2.7 和 python3.5 加入其中

	1.> # update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1  
	2.> update-alternatives: using /usr/bin/python2.7 to provide /usr/bin/python (python) in auto mode  
	3.> # update-alternatives --install /usr/bin/python python /usr/bin/python3.5 2  
	4.> update-alternatives: using /usr/bin/python3.5 to provide /usr/bin/python (python) in auto mode

`--install` 选项使用了多个参数用于创建符号链接。最后一个参数指定了此选项的优先级，如果我们没有手动来设置替代选项，那么具有最高优先级的选项就会被选中。这个例子中，我们为 `/usr/bin/python3.5` 设置的优先级为2，所以 `update-alternatives` 命令会自动将它设置为默认 Python 版本。

现在开始，我们就可以使用下方的命令随时在列出的 Python 替代版本中任意切换了。

	1.> # update-alternatives --config python  
	2.> # python --version  
	3.> Python 2.7.12