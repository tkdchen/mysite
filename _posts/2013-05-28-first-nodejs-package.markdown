---
layout: post
title: 我的第一个Node软件包nsignal
category: blog
tags: nsignal npm node javascript
---

# 我的第一个Node软件包nsignal

{% excerpt %}

写于2013年5月28日晚

用Javascript写程序已经有段时间了。在Node的环境下，Javascript充分发挥了起长期以来被忽视的强大力量。随着写的Javascript代码越来越多，对这门语言有了更加深入的理解。与此同时，在不断地使用和阅读第三方Node软件包的过程中，越发的感觉到Javascript的潜力无限和未来发展的各种无限可能。你愿意使用Javascirpt写日常的脚本程序吗，写GUI程序吗，开发网络应用吗？

今天花了一点时间，把目前项目中的一个简单的组件剥离了出来，把它放到了[github](https://github.com/tkdchen/nsignal)上，同时也快速体验了一把Node软件包分发。

使用下面的命令从Node软件仓库npm中的安装，

    npm install nsignal

{% endexcerpt %}

[nsignal](https://npmjs.org/package/nsignal)借鉴了django的Signal机制，与mongoose的模型对象结合使用。使得开发人员能够在某个对象模型发生特定事件的时候，通知注册的处理函数，以完成某些任务。这样，可以将这部分代码和处理模型对象的逻辑，例如create，update和delete，分离开。具体的使用方法，可以参照[README.md](https://github.com/tkdchen/nsignal/blob/master/README.md)。

Node软件包发布过程很简单。下面两个方面的内容都做了，成功发布那就是必须的了。

首先，准备源代码。这没什么特别的。其他的项目怎么做，这还怎么做。只不过，需要一些npm所需的文件。其中，package.json是最重要的一个。此外，除了源代码文件，还应该添加以下文件。

- README.md
- LICENSE

nsignal使用的目录结构非常简单，只有两层。第一层，也就是根目录，包含了所有了文件，包括源代码文件。第二层是一个名为tests的目录，所有包含测试代码的源文件都会放在这里。

其次，发布。[Getting Started with NPM (as a developer)](https://gist.github.com/coolaj86/1318304) 给出了非常详细的步骤，可助你一臂之力。步骤很简单。仔细地，一步一步地跟着做就okay。需要提到提到一点是，``npm init``不会覆盖你写的package.json。它会向你提几个问题，然后会连同你写的package.json中的所有内容一同创建一份新的出来。

几点感受。

Javascript的简单性充分的体现到了软件包的分发上。一个带有充分信息的package.json是多么的重要。

Javascript的简单性是极具欺骗性的。一旦你上了船，不久就会发现处处被埋伏。一不小心就踩下去。随处可见的回调处理，对传统的程序的顺序执行的思维逻辑形成挑战。当然，这同时也是非常有意思的，在你逐渐熟悉了这种行事方式后。幸好，Node社区的大牛门开发了[async](https://github.com/caolan/async)这样一把神器。

在Javascript中写面向对象的代码是多么的难受。很多探路者们已经为像我这样的后来者留下了丰富的、有价值的资产。Node的``utils.inherits``能够方便的实现类的继承概念。但是，千万不要用在Python、Java、c++里进行OOP经验完全搬到这里。

基于事件的异步模型是现在炙手可热的编程方式，Python有[gevent](http://gevent.org/)，Java、Scala、ruby都有各自的实现方案。而V8给予了node先天的能力。只要你开始在node中写Javascript，跑Javascript，你就在享受这种特性。期待更加完备的Javascript。

如果你正沉浸在时髦的Node和Javascript中，祝你好运气，少踩点坑。:)

