---
layout:       post
title:        Installing VirtualBox Guest Additions on Ubuntu Server 17.10
permalink:    ubuntu-server-virtualbox-guest-additions
tags:         ubuntu virtualbox
categories:   [web-dev, virtualbox]
---

In order to use **VirtualBox shared folders** with **Ubuntu Server** first install **VirtualBox Guest Additions**. For this to work, a few additional modules are required:

{% highlight text %}
sudo apt install -y dkms build-essential linux-headers-generic linux-headers-$(uname -r)
{% endhighlight %}

Once the Linux headers have been installed, mount the Guest Additions CD and run:
{% highlight text %}
sudo mount /dev/cdrom /media/cdrom
sudo ./VboxLinuxAdditions.run
{% endhighlight %}

A warning about **X11** is shown, however rebooting shows any configured shared folders under `/media/sf_<folder name>`.