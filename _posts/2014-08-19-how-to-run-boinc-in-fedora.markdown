---
layout: post
title: 如何在Fedora中运行BOINC
date: 2014-08-19 09:00:10
category: blog
tags: BOINC Fedora
---

在[Fedora](http://fedoraproject.org/)中运行[BOINC](http://boinc.berkeley.edu/)有两个方法，可以从官方网站下载二进制安装包，然后在本地安装。

{% highlight bash %}
sh boinc_7.2.42_x86_64-pc-linux-gnu.sh
{% endhighlight %}

截至写这些文字的时候，`7.2.42`是最新的版本。有一点提示的是，安装过程会在执行此条命令的目录中创建
BOINC子目录，所有必需的文件都将安装在这个目录中。

然而，这第一种方法，我没有成功。在启动`boincmgr`总是出现程序库错误，无法运行。此时，就需要另外
一种，BOINC的Wiki中也记载的方式。即使用特定的Linux发行版本的包管理器安装。

对于Fedora，使用下面的命令。

{% highlight bash %}
sudo yum install boinc-client boinc-manager
{% endhighlight %}

BOINC管理器允许管理多台计算机上的boinc计算进程。首次启动管理器后，需要手动地连接到某一台计算机上。步骤如下，

1. 通过菜单进入计算机选择对话框 `Advanced -> Select Computer`
2. 在`Host name`中，输入`127.0.0.1`。
3. 以root用户身份打开文件`/var/lib/boinc/gui_rpc_auth.cfg`，将其中地完整内容拷贝到`Password`。

如果已经添加了至少一个分布式计算项目，你将会看到类似如下图所示的界面。

![BOINC Manager Sample](/images/blog/boinc-sample.png)
