---
layout:       post
title:        Running OWASP Juice Shop from Docker
permalink:    docker-owasp-juice-shop
tags:
  - cyber
  - docker
---

To pull the **docker** image:
{% highlight text %}
docker pull bkimminich/juice-shop
{% endhighlight %}

To run **Juice Shop** in **Docker**:
{% highlight text %}
docker run --rm -p 3000:3000 bkimminich/juice-shop
{% endhighlight %}

With the **Juice Shop** image running in docker, the site is available at:
{% highlight text %}
http://localhost:3000
{% endhighlight %}
