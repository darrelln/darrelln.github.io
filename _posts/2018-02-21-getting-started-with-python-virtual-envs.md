---
layout:       post
title:        Getting Started with Python Virtual Environments
permalink:    getting-started-with-python-virtual-envs
tags:
  - python
  - dev
  - web-dev
  - virtualization
---

**Python virtual environments** are a good way to improve flexibility and prevent projects and packages from interfering with each other. Each virtual env is created with it’s own Python binary and set of site packages. This means projects can reference different Python interpreters and installing packages in virtual environments also prevents polluting the global space with unwanted packages. The two approaches we will look at here are the **virtualenv** package and Python 3’s **venv** module.
<br />
<br />
## The virtualenv Package
Virtualenv is based on Install **virtualenv** using **Pip** (**pip3** assumes you're using **Python3**):

{% highlight text %}
pip3 install virtualenv
{% endhighlight %}

Pip will retrieve and install the package automatically:

![Installing virtualenv using pip](/assets/2018-02-21-getting-started-with-python-virtual-envs/pip3_install_virtualenv.png){:class="center-image"}

With virtualenv installed, **creating a new project** is simple:

{% highlight text %}
mkdir project1
virtualenv project1
{% endhighlight %}

To create a project using a specific interpreter, use the **-p** flag with virtualenv:

{% highlight text %}
virtualenv project1 -p $(which python3)
{% endhighlight %}

To start the project, **cd** into the project folder and **activate** the environment:

{% highlight text %}
cd project1
source bin/activate
{% endhighlight %}

Once the project is active, the command prompt shows the name of the project in parenthesis:

{% highlight text %}
(project1) MacBook:project1 user$ _
{% endhighlight %}

With the project running, **pip** will install packages locally to the virtual environment. To exit the virtual environment, use **deactivate** at the prompt:

{% highlight text %}
(project1) MacBook:project1 user$ deactivate
{% endhighlight %}

To remove the project, just delete the **project1** folder.
<br />
<br />
## Python's venv Module
Python also has the **venv** module. To create a new project named **project2** in the current directory:

{% highlight text %}
python3 -m venv project2
{% endhighlight %}

This will create a new virtual environment in the **project2** folder. As with **virtualenv**, **cd** into the **project2** folder and manage the environment with the **activate** and **deactivate** commands:

{% highlight text %}
MacBook:virtual-envs user$ cd project2
MacBook:project2 user$ source bin/activate
...
(project2) MacBook:project2 user$ deactivate
MacBook:project2 user$ _
{% endhighlight %}
<br />
<br />
## PyCharm Supports Virtual Environments
**PyCharm** supports working with existing virtual environments as well as creating new projects using **virtualenv**:
![Installing virtualenv using pip](/assets/2018-02-21-getting-started-with-python-virtual-envs/PyCharm_preferences.png){:class="responsive-img center-image"}
![Installing virtualenv using pip](/assets/2018-02-21-getting-started-with-python-virtual-envs/PyCharm_new_project.png){:class="responsive-img center-image"}
