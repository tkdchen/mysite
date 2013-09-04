---
layout: post
title: rejectUnauthorized的默认值变了
date: 2013-06-19
category: blog
tags: node tls
---

这个变化发生在0.9.2版本中。下面的代码片段摘录自[tls.js](https://github.com/joyent/node/blob/v0.9.2-release/lib/tls.js)

{% highlight javascript %}
var defaults = {
  rejectUnauthorized: '0' !== process.env.NODE_TLS_REJECT_UNAUTHORIZED
};
{% endhighlight %}

从与[0.9.1](https://github.com/joyent/node/blob/v0.9.1-release/lib/tls.js)版本的对比来看，在新版本中，允许使用环境变量来控制参数`rejectUnauthorized`。上面的代码则使得默认值从`false`变更为`true`。在tls模块的文档中记录了这个变化，遗憾的是Node的Changelog则没有记录。

为了确保代码能够在Node的各个版本间保持兼容性，在`options`参数中明确地给`rejectUnauthorized`设置所期望的值，而不使用默认值。
