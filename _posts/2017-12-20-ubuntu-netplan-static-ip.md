---
layout:       post
title:        Assigning a Static IP on Ubuntu Server 17.10
tags:         ubuntu netplan 
permalink:    ubuntu-netplan-static-ip
---

The latest version of Ubuntu Server now uses **NetPlan** to configure networking. To assign a static IP address, backup the original file and open the `.yaml` in nano:

{% highlight text %}
cd /etc/netplan
sudo cp 01-netcfg.yaml 01-netcfg.yaml.orig
sudo nano 01-netcfg.yaml 
{% endhighlight %}

Update with the relevant details:
{% highlight text %}
# This file describes the network interfaces available on your system
# For more information, see netplan(5).
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: no
      addresses: [192.168.x.x/24]
      gateway4: 192.168.x.x
      nameservers:
        addresses: [8.8.8.8,8.8.4.4]
{% endhighlight %}

Once complete, use **ctrl+o** and **ctrl-x** to save changes and exit. To apply the changes, use:
{% highlight text %}
sudo netplan apply
{% endhighlight %}