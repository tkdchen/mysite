---
layout: post
title: 这个函数定义会让你抓狂吗？
date: 2013-09-04
tags: C libsolv
---

这个函数定义是从[libsolv](https://github.com/openSUSE/libsolv)中看到的。当你看到它的时候，会有中抓狂的感觉吗？

{% highlight c++ %}

char*
pool_tmpjoin(Pool *pool, const char *str1, const char *str2, const char *str3)
{
}

{% endhighlight %}

str[1..3]这三个参数是干什么用的，传什么参数，函数连个注释都没有。我去，不得不从代码里面找答案。