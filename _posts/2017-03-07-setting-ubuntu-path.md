---
title: "linux 环境变量设置方法总结"
date: 2017-03-05 22:30:09
categories:
- Linux
- Ubuntu
---

linux 环境变量设置方法总结（PATH / LD_LIBRARY_PATH）

PATH和LD_LIBRARY_PATH本质都是变量，所谓变量的意思就是由别人赋值产生的，直觉往往会让我们添加和减少这个变量本身的某些路径，实际上这是不正确的。正确的做法是我们要去修改赋予这个变量数值的那些配置文件，加一条路径或者减一条。说到底变量只关乎显示，不关乎其用于显示的内容。

##PATH:  可执行程序的查找路径
查看当前环境变量:
`echo $PATH`

设置: 

方法一：`export PATH=PATH:/XXX` 但是登出后就失效

方法二：修改~/.bashrc或~/.bash_profile或系统级别的/etc/profile

1. 在其中添加例如`export PATH=/opt/ActivePython-2.7/bin:$PATH`
2. source .bashrc  (Source命令也称为“点命令”，也就是一个点符号（.）。source命令通常用于重新执行刚修改的初始化文件，使之立即生效，而不必注销并重新登录)


##LD_LIBRARY_PATH: 动态库的查找路径
设置：

方法一： `export  LD_LIBRARY_PATH=LD_LIBRARY_PATH:/XXX` 但是登出后就失效。

方法二：  修改~/.bashrc或~/.bash_profile或系统级别的/etc/profile。

1. 在其中添加例如`export PATH=/opt/ActiveP/lib:$LD_LIBRARY_PATH`

2. source .bashrc  (Source命令也称为“点命令”，也就是一个点符号（.）。source命令通常用于重新执行刚修改的初始化文件，使之立即生效，而不必注销并重新登录)。

方法三：这个没有修改LD_LIBRARY_PATH但是效果是一样的实现动态库的查找， 

1. /etc/ld.so.conf下面加一行`/usr/local/mysql/lib`

2. 保存过后ldconfig一下（ldconfig 命令的用途,主要是在默认搜寻目录(/lib和/usr/lib)以及动态库配置文件/etc/ld.so.conf内所列的目录下,搜索出可共享的动态链接库(格式如前介绍,lib*.so*),进而创建出动态装入程序(ld.so)所需的连接和缓存文件.缓存文件默认为/etc/ld.so.cache,此文件保存已排好序的动态链接库名字列表.）
方法三设置稍微麻烦，好处是比较不受用户的限制。
