给Python软件包打RPM包用的SPEC模板
================================

这个模板可作为为Python软件包制作RPM包的一个起点。它提供了能够成功完成一个RPM包的所有必要的信息。
同时，预留了一些需要根据使用者的实际情况替换的信息。我使用了$var的格式来表明哪些需要用真实值来替
换。

模板遵循Fedora社区的RPM包打包规范。例如，Python软件包对应的RPM包通常加上 ``python-`` 前缀。
如果你是在为一个django框架的扩展包打包，那么前缀应该使用 ``django-`` 。

.. index::
	single: rpm
	single: spec
	single: gist
