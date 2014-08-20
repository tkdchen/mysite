---
layout: post
title: 在VIM中常用的正则表达式
date: 2014-08-20 15:25:20
category: blog
tags: VIM Regex
---

{% highlight vim %}

" Tip 1: Add extra characters before the first character of each line
s/^\(.\)/- \1/

" Tip 1.1: Comment lines of code
s/^\(.\)/# \1/

" Tip 1.2: Comment lines by adding # before the first letter
s/^\( \+\)\(.\)/\1# \2/

" Tip 2: remove all trailing white space characters
s/\s\+$//

" Tip 3: search class and method definition
^\(class\| *def\) \+\w\+

{% endhighlight %}
