---
layout:       post
title:        VirtualBox Shared Folders on Ubuntu Server 17.10
permalink:    ubuntu-server-virtualbox-shared-folders
tags:         ubuntu virtualbox
categories:   [web-dev, virtualbox]
---

Information courtesy of [this thread](https://forums.virtualbox.org/viewtopic.php?f=3&t=38891).

This is specifically about mounting a shared folder for use as the **DocumentRoot** folder for **Apache2**. The folder name we will use is **folder_name** and the mount point will be **/media/folder_name**.

First, with the VM powered down, configure the shared folder in VirtualBox. Configure the folder as a machine level folder, not read-only and not auto-mounted. Auto-mounting the folder will see VirtualBox (vboxsf) owning the folder and screw up Apache. Add the user to the vboxsf group (the number will likely differ and replace **username** with the username you are using):
{% highlight text %}
sudo nano /etc/group
vboxsf:x:1000:username
{% endhighlight %}

To ensure the folder mounts automatically when the system reboots, add an entry to `/etc/fstab`. First, the user's **UID** and **GID** are required. These can be found in `/etc/passwd` for the user in question (the **1000** and **1001** shown below), and will look something like:
{% highlight text %}
user:x:1000:1001:User,,,:/home/user:/bin/bash
{% endhighlight %}

Mount the share:
{% highlight text %}
cd /media
sudo mkdir folder_name
sudo mount -t vboxsf -o rw,uid=1000,gid=1001 folder_name /media/folder_name
{% endhighlight %}

Update `fstab` and add an entry with the mount details at the bottom of the file (use **tabs, not spaces**):
{% highlight text %}
sudo nano /etc/fstab
folder_name	/media/folder_name	vboxsf	defaults	  0	0
{% endhighlight %}

Reboot and the folder should mount automatically at `/media/folder_name` and be owned by **root** instead of **vboxsf**.





