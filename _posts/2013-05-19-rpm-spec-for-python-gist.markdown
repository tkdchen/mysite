---
layout: post
title: 给Python软件包打RPM包用的SPEC模板
date: 2013-05-19
category: blog
tags: RPM
---

这个模板可作为为Python软件包制作RPM包的一个起点。它提供了能够成功完成一个RPM包的所有必要的信息。
同时，预留了一些需要根据使用者的实际情况替换的信息。我使用了$var的格式来表明哪些需要用真实值来替
换。

模板遵循Fedora社区的RPM包打包规范。例如，Python软件包对应的RPM包通常加上``python-``前缀。
如果你是在为一个django框架的扩展包打包，那么前缀应该使用``django-``。

{% highlight spec %}

%{!?python_sitelib: %define python_sitelib %(%{__python} -c "from distutils.sysconfig import get_python_lib; print get_python_lib()")}
%global pkg_name $name

Name: python-%{pkg_name}
Version: $version
Release: 1%{?dist}
Summary: $summary

Group: Development/Languages
License: $license
URL: $url
Source0: %{pkg_name}-%{version}.tar.gz
BuildArch: noarch

# BuildRequires section here if necessary

# Requires section here if necessary

%description

%prep
%setup -q -n %{pkg_name}-%{version}

%build
%{__python} setup.py build

%install
rm -rf $RPM_BUILD_ROOT
%{__python} setup.py install -O1 --skip-build --root $RPM_BUILD_ROOT

%clean
rm -rf $RPM_BUILD_ROOT

%files
%defattr(-,root,root,-)
%doc CHANGES.txt LICENSE.txt MANIFEST.in README.rst TODO.txt VERSION.txt
%{python_sitelib}/%{pkg_name}/
%{python_sitelib}/%{pkg_name}-%{version}-py*.egg-info/

%changelog

{% endhighlight %}
