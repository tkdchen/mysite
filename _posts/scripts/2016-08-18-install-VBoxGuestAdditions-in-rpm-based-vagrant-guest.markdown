---
layout: post
title: Install VBoxGuestAdditions in RPM based Vagrant Guest
date: 2016-08-18
category: scripts
tags: Vagrant VirtualBox
---

{% highlight bash %}
function install_vb_additions
{
    local VB_VERSION="$1";
    local download_url=http://download.virtualbox.org/virtualbox/${VB_VERSION}/VBoxGuestAdditions_${VB_VERSION}.iso
    curl -C - -O $download_url
    sudo mkdir /mnt/iso
    sudo mount -o loop VBoxGuestAdditions_${VB_VERSION}.iso /mnt/iso
    sudo yum install -y kernel-devel-\`uname -r\` gcc make perl bzip2
    pushd /mnt/iso
    sudo ./VBoxLinuxAdditions.run
    popd
    sudo umount /mnt/iso
}
{% endhighlight %}

# How to use

## Get version of VirtualBox

```
VBoxManager --version
```

## Steps to install VBoxGuestAdditions

Let's assume version of VirtualBox is ``5.0.12``.

{% highlight bash %}
vagrant ssh
# Paste function install\_vb\_additions in shell
install_vb_additions 5.0.12
exit
vagrant reload
{% endhighlight %}
