---
layout:       post
title:        Deploying Python Flask to Ubuntu Server 17.10
tags:         python python-web flask 
permalink:    deploying-python-flask-to-ubuntu-server-17
---

<p align="center" class="warn">WARNING: PLEASE DO NOT USE THIS IN PRODUCTION</p>

First, create a new VM using [**Oracle's VirtualBox**](https://www.virtualbox.org/wiki/downloads) and [**Ubuntu Server 17.10**](https://www.ubuntu.com/download/server). When configuring the VM, ensure the network adapter type is set to bridged rather than NAT otherwise Ubuntu will not have network access by default. Once the server is up and running, install updates:
{% highlight text %}
sudo apt update
sudo apt upgrade -y
{% endhighlight %}

Now install Apache and [**mod-WSGI**](https://en.wikipedia.org/wiki/mod_wsgi):
{% highlight text %}
sudo apt install -y apache2 libapache2-mod-wsgi-py3
{% endhighlight %}

Next, use a web browser or curl to check Apache is working and serving pages. If you do this on your local machine use **localhost** or **http://127.0.0.1/** as the address, otherwise use **ifconfig** to show the server's IP address. If all is well and Apache is working correctly, the default Apache/Ubuntu page will be served.

Install [**Flask**](http://flask.pocoo.org) for **Python3**. To ensure Flask installed correctly, check the version:
{% highlight text %}
sudo apt install -y python3-flask
flask --version
{% endhighlight %}

Configure Apache to serve the Flask app. Under Apache's **www** directory create a new folder for the app, in this case named **flaskapp** and change into the new directory. Now create an **init.py** file along with a **.wsgi** file for the app, in this case **flaskapp.wsgi**: 
{% highlight text %}
cd /var/www
sudo mkdir flaskapp && cd flaskapp
sudo nano init.py
sudo nano flaskapp.wsgi
{% endhighlight %}

The **init.py** file is used to initialise an instance of Flask and handles routing. In this instance, the only route is for the index page which returns **'flaskapp'** so you know the app is working correctly:
{% highlight js %}
from flask import Flask

app = Flask(__name__)

@app.route("/")
def index():
  return "<h1>flaskapp</h1>"

if __name__ == "__main__":
  app.run()
{% endhighlight %}

Add the following to **flaskapp.wsgi**. The **flaskapp.wsgi** file allows **WSGI** to grab an instance of the app. Note, the app needs to be imported with an alias of **application**, as shown below:
{% highlight js %}
import sys

sys.path.insert(0, "/var/www/flaskapp")

from init import app as application
{% endhighlight %}

Now the app is present, it needs to be enabled in **Apache**. First, create an [Apache config file](https://apache.org/docs/2.4/vhosts/) under Apache's **sites-available** directory:
{% highlight text %}
cd /etc/apache2/sites-available/
sudo nano flaskapp.conf
{% endhighlight %}

Add the following to **flaskapp.conf**:
{% highlight text %}
<VirtualHost *:80>
  WSGIScriptAlias / /var/www/flaskapp/flaskapp.wsgi

  <Directory /var/www/flaskapp>
    Require all granted
  </Directory>
</VirtualHost>
{% endhighlight %}

To check the configuraion is valid, use:
{% highlight text %}
apachectl configtest
{% endhighlight %}

Lastly, disable the default Apache site and enable the Flask app. Once the app has been enabled, check Apache's **sites-enabled** directory to confirm it contains a symlink and the app has indeed been enabled. Reload the Apache service and everything should be good to go:
{% highlight text %}
sudo a2dissite 000-default
sudo a2ensite flask.conf
sudo systemctl reload apache2
{% endhighlight %}

The final step is to refresh your browser and confirm Apache serves the Flask app.
