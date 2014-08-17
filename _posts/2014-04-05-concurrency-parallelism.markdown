---
layout: post
title: Concurrency和Parallelism
date: 2014-04-05
category: blog
tags: concurrency parallelism computation
---

这两天特别趁着假期再次学习一下并发（Concurrency）和并行（Parallelism）。学习的主要内容是这两
者之间的区别。很多人，包括我自己在内，对并发和并行的区别了解的并不是很清晰和准确。需要说明的一点是，
这篇短文不是阐述并发和并行的理论文章。

Concurrency完全不同于Parallelism。计算机系统本身就是一个Concurrency模型。各种中断充斥在计算机系统的运行过程中，这是一个典型的基于事件的处理模型。在一个CPU上并行地执行多个任务。人的大脑是一个Concurrency模型。

像SETI@home项目，把一个巨大的任务分成一系列小的可以被独立的在世界各地的个人计算机上运算的数据包。这个模型是Parallelism。特点是，

- 它需要多个独立存在的运算处理单元（每一台个人计算机）
- 计算可以在每一个运算处理单元中重复的计算并得到相同的结果
- 每一个运算单元之间不需要任何形式的通信以完成运算
- 每一个运算单元都有自己的完成运算任务的环境（或者叫上下文环境）

http://www.haskell.org/haskellwiki/Parallelism_vs._Concurrency
http://stackoverflow.com/a/15596277/968262
http://blog.golang.org/concurrency-is-not-parallelism
http://en.wikipedia.org/wiki/Concurrent_computing
