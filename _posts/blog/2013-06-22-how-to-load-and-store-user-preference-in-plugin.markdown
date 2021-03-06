---
layout: post
title: 如何在GTG插件中读取和存储用户偏好数据
date: 2013-06-22
category: blog
tags: GTG Python
---

每一个插件都可以管理自己的用户偏好数据。这些数据存储在插件自己的配置目录中，彼此独立，互不影响。在GTG中，用户偏好数据被存储到一个字典对象中，使用pickel将其序列化到指定的文件中，此文件位于插件自己的专属目录中，其名称由开发者决定，并通过参数传递给用户偏好数据操作接口。插件API提供了两个方法用于读取和存储用户偏好数据，它们分别是`load_configuration_object` 和`save_configuration_object`，接口定义如下。

{% highlight Python %}
def load_configuration_object(self, plugin_name, filename,
                              basedir=xdg_config_home,
                              default_values=None):
    '''Load preference'''

def save_configuration_object(self, plugin_name, filename, item,
                              basedir=xdg_config_home):
    '''Store preference'''
{% endhighlight %}

这两个接口均接受插件名称（plugin_name）和文件名（filename）两个参数。GTG不提供默认的文件名。推荐使用名称`preference`。存储用户偏好数据的字典对象传递给参数`item`。

接口非常简单，写到这就可以结束了。Notification Area插件实现提供了很好的代码参考。
