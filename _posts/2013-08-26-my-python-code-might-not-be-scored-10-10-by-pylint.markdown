---
layout: post
title: 我的Python代码或许在多数时候都得不了100分
date: 2013-08-26
category: blog
tags: Python pylint pep8
---

# 我的Python代码或许在多数时候都得不了100分

{% excerpt %}

在Python社区中，开发人员能够使用[pep8](https://pypi.python.org/pypi/pep8)，[pyflakes](https://pypi.python.org/pypi/pyflakes)和[pylint](http://www.pylint.org)来检查自己的代码是否符合[PEP8所描述的指导规范](http://www.python.org/dev/peps/pep-0008/)，同时还能够检查出来哪些变量是我们定义了，但是没有用到的，等等。在这些工具中，pylint无非是（似乎是）最受社区推崇的一个。OpenStack使用它做代码检查，Sonar除了支持自己默认的检查规则外，也就只支持pylint了。

话说pylint功能强大，在默认配置的控制下，它检查Python代码的方方面面。最后得到的评分也许会让自己大跌眼镜。我第一次用的时候就是这个感受。难道我写代码的水平这么差劲吗？！:)

pylint只是一个工具而已。不要让它束缚了我们的思想，我们写代码不是为了在pylint中得到高分而爽一把。用pylint需要稍微考虑一下：

第一，我们需要一个什么样的代码风格。

第二，让pylint控制到什么程度。

第三，使用pylint为了什么。

{% endexcerpt %}

看这些文字的你肯定还有你自己更富有创意和特点的点子。不管怎样，我会首选pep8和pyflakes。在项目需要，不得不用pylint的时候，才会使用它，同时遵循的原则就是，不要为了得到pylint的赞赏而去写代码。例如，绝对不会在代码里写`program`。在基于django的项目中，也不会为了避免检查`request`而额外的付出什么。

或许我的Python代码在很长时间内，都不会得到pylint满分成绩。9分是我能够接受。然而，这并不意味着我在回避代码质量。相反，我会尽可能地确保我的Python代码风格地一致性，使用诸如pylint等的工具帮我找出改进之处。
