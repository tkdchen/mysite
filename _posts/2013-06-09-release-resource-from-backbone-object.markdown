---
layout: post
title: 及时释放Backbone对象占用的资源
date: 2013-06-09 10:30:00
category: blog
tags: javascript backbone
---

# 及时释放Backbone对象占用的资源

{% excerpt %}

[Backbone](http://backbonejs.org/)对象？是的，此文中提到的Backbone对象指的就是View，Model和Collection。他们占用什么资源呢？在一个使用了Backbone、[socket.io](http://socket.io/)和[ioBind](http://alogicalparadox.com/backbone.iobind/)的项目中，会在DOM层、Model和Collection，以及socket三个层面注册大量的事件处理函数实现用户功能。这些事件监听就是最大的一个资源占用。不仅仅消耗内存，还会导致在反复创建同一个Model或者Collection的对象的情况下事件响应的泛滥。因此，在什么时候释放这些资源，以及如何释放非常值得关注。为了回答这两个问题，先通过代码来看看典型的应用方式。

{% highlight javascript linenos %}

var BaseModel = Backbone.Model.extend({
  idAttribute: "_id",
  socket: window.socket,
  urlRoot: undefined,

  url: function() {
    return "/" + this.urlRoot + "/" + this.id;
  },

  initialize: function (attributes, options) {
    this.on('modelCleanup', this.modelCleanup, this);
    this.ioBind('update', this.serverChange, this);
    this.ioBind('delete', this.serverDelete, this);
  },

  serverChange: function(data) {}
  serverDelete: function(data) {}
  modelCleanup: function(data) {}
});

var Checklist = BaseModel.extend({
  urlRoot: "checklist",

  initialize: function (attributes, options) {
    this.itemCollection = new ChecklistItemCollection;
    BaseModel.prototype.initialize.apply(this, arguments);
  }
});

var ChecklistItem = Backbone.Model.extend({
  urlRoot: "checklistitem"
});

var BaseCollection = Backbone.Collection.extend({
  url: undefined,
  model: undefined,
  socket: window.socket,

  initialize: function(models, options) {
    _.bindAll(this, "serverCreate");
    this.ioBind("create", this.socket, this.serverCreate, this);
  },

  serverCreate: function(data) {}
});

var ChecklistCollection = BaseCollection.extend({
  url: "/checklist",
  model: Checklist
});

var ChecklistItemCollection = BaseCollection.extend({
  url: "/checklistitem",
  model: ChecklistItem
});

{% endhighlight %}

{% endexcerpt %}

上面的代码定义了一个用于Checklist管理的模型。``Checklist``包含一个``ChecklistIteCollection``的实例来描述一对多的关系。使用ioBind来管理前端和后端之间的socket通信，以实现特定对象的CRUD操作。从代码中我们可以看到，只要创建了其中的任何一个对象，``this.ioBind``方法调用都将会导致响应socket事件的处理函数被注册。接下来是View的定义。

{% highlight javascript linenos %}

var ChecklistView = Backbone.View.extend({
  initialize: function() {
    this.itemViews = [];
    this.model.on("change", this.render, this);
    this.collection.on("add", this.onChecklistItemAdded, this);
  },

  render: function() {},

  onChecklistItemAdded: function(checklistItem) {
    var view = new ChecklistItemView({model: checklistItem});
    this.$el.append(view.render().el);
    this.itemViews.push(view);
  }
});

var ChecklistsView = Backbone.View.extend({
  initialize: function() {
    this.checklistViews = [];
    this.collection.on("add", this.onChecklistAdded, this);
  },

  onChecklistAdded: function(checklist) {
    var view = new ChecklistView({model: checklist});
    this.$el.append(view.render().el);
    this.checklistViews.push(view);
  }
});

var ChecklistItemView = Backbone.View.extend({
  events: {
    "click .js-remove-item": function(event) {}
  },

  initialize: function() {
    this.model.on("change", this.render, this);
  },

  render: function() {}
});

var checklistManageView = new ChecklistsView({
  el: $(".some-selector"),
  collection: new Checklists
});

{% endhighlight %}

到此，管理Checklist的所有前端代码已经齐全。由于此文着重于前端的处理，所以没有给出后端代码实现的参考。如果你对socket.io足够的了解，想必你会知道有很多种技术能够用来实现后端的功能。Node加socket.io Server是其中之一。此外，参照socket.io实现的其它[解决方案](https://github.com/learnboost/socket.io/wiki)也非常丰富。好了，言归正传。让我们回过头来分析一下View层代码都干了什么。

这几个View足够简单，其中每一个的代码结构都很中规中矩，无非干了这么几件事。首先，在初始化阶段创建必要的内部模型对象。其次，在那些模型对象之上监听感兴趣的事件，完成页面上某个区域的用户功能的实现。最后，通过events对象中的声明，在View管理的DOM中绑定DOM级别的事件处理函数。所有的这些是使用Backbone实现前端页面用户功能所必须要做的最小的集合。初始化阶段介绍完了。那么，在一个View的生命结束的时候会发生什么事情呢？绑定事件，创建对象等等这些可都是要占用资源的，另外你肯定也不期望一个View对象不用了，它还在持续的响应事件，造成重复的不必要的事件处理。遗憾的是，Backbone本身并没有提供一种机制或者规范来强制或者提示开发人员应该在View对象的生命即将结束的时候改做写什么。这就造成Backbone使用经验不足的开发人员，经常会忽略这一点。当发现问题的时候，用拐弯抹角的方法去避免不期望的事情的发生。

当然了，Backbone提供了所有必要的函数调用来清理和回收资源，怎么用好这些就需要我们细心思考了。在这里，给出如下解决方案。两点。一是，给View添加close方法。二是，给Model和Collection添加dispose方法。

View的close方法只限于在DOM中释放资源，例如当前View对象中的事件监听。为了告诉所有子View对象它们该关闭了，直接调用它们的close方法即可。

Model和Collection的dispose方法则只关注自身的资源释放。即关闭所有在自身上建立的事件监听，以及通过ioBind或者其他方式建立的socket事件监听。示例代码如下。

{% highlight javascript linenos %}

var BaseView = Backbone.View.extend({
  close: function() {
    this.remove();
  }
});

var BaseMode = Backbone.Model.extend({
  dispose: function() {
    this.off();
    if (!this.noIoBind)
      this.ioUnbindAll();
    return this;
  }
});

var BaseCollection = Backbone.Collection.extend({
  dispose: function() {
    this.forEach(function(item, index) {
      item.dispose();
    });
    this.off();
    if (!this.noIoBind)
      this.ioUnbindAll();
    return this;
  }
});

{% endhighlight %}

具体的实现方式，可以依据项目的实际情况而定。我在目前的项目中，为View、Model、Collection分别定义基类，提供最基本的资源释放实现。在开发过程中约定，所有的自定义的View、Model和Collection都必须从相应的基类继承，同时在其生命的终点做好善后工作。

此解决方案是一种契约式的。无论在Javascript语言层面，还是Backbone层面，都没有一种机制强制实施。这就需要在项目中，开发人员之间达成共识，以一种规范的形式在团队中明确和落实。

尽管Javascript是具有垃圾回收机制的高级的动态语言，但跳出语言来看，重视资源的及时释放可以带来非常大的好处，可以避免一些很诡异又很难找出原因的问题。祝你使用Javascript编程愉快，使用Backbone少踩坑。:)
