---
Type:
  - Page
aliases: 
tags: []
Status: 
modifiedDate: 2025/06/12, 16:29:37
---

# Linux

[一文教你快速修改ubuntu终端显示的主机名和用户名 - 小麦大叔 - 博客园 (cnblogs.com)](https://www.cnblogs.com/unclemac/p/12783332.html)
[bash-如何在Linux命令行提示符下显示主机名-Unix和Linux堆栈交换 --- bash - How to show the host name in Linux commandline prompt - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/218174/how-to-show-the-host-name-in-linux-commandline-prompt)

- 修改hostname
	- 查看当前终端输出格式
		- `echo $PS1`
	-  修改显示
		- 在bashrc文件中加入此行  
		- ```export PS1='\[\033[0;32m\]\u@{你想显示的}:\[\033[36m\]\w\[\033[0m\] \$a```
		- 应用修改`. ~/.bashrc`

[【原创】Syncthing搭建自己的中继服务和发现服务_syncthing中继服务器-CSDN博客](https://blog.csdn.net/qq_35150992/article/details/128792276)
