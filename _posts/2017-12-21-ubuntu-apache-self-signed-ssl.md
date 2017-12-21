---
layout:       post
title:        Self-signed SSL Certificates for Apache2
tags:         ubuntu apache2 ssl
permalink:    ubuntu-apache-self-signed-ssl
---

<p align="center" class="warn">WARNING: PLEASE DO NOT USE THIS IN PRODUCTION</p>

This is more a reminder rather than a full tutorial to setting up Apache with a self-signed SSL certificate. First, enable Apache's **SSL module**:
{% highlight text %}
sudo a2enmod ssl
{% endhighlight %}

Using **OpenSSL**, generate a self-signed SSL certificate:
{% highlight text %}
cd /etc/apache2
sudo mkdir ssl
cd ssl
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/site_name.key -out /etc/apache2/ssl/site_name.crt
{% endhighlight %}

Configure an SSL-enabled version of the site by copying the `default-ssl.conf` file to a site-name equivalent:
{% highlight text %}
cd /etc/apache2/sites-available
sudo cp default-ssl.conf site_name-ssl.conf
{% endhighlight %}

*As part of the certificate generation a number of questions are asked including company name and location. Ensure the names are set to the site name, such as `site_name.com`, otherwise the certificate will not be applicable to the domain and an error will be shown by browsers.*

Update the `site_name-ssl.conf` file with the necessary SSL settings:
{% highlight text %}
<VirtualHost _default_:443>
  ServerAdmin admin@site_name.com
  ServerName site_name.com
  ServerAlias www.site_name.com
  …
  SSLEngine on
  SSLCertificateFile /etc/apache2/ssl/site_name.crt
  SSLCertificateKeyFile /etc/apache2/ssl/site_name.key
  ...
</VirtualHost>
{% endhighlight %}

Enable the new site and check it's present in `/etc/apache2/sites-enabled`:
{% highlight text %}
sudo a2ensite site_name-ssl.conf
{% endhighlight %}

Restart Apache:
{% highlight text %}
sudo systemctl restart apache2
{% endhighlight %}

Finally, test with a browser or curl using **https://**. As the certificate is self-signed browsers will require a **security exception** to accept the cert. To debug, check the **Apache logs** using `tail /var/log/apache2/{access|error}.log`