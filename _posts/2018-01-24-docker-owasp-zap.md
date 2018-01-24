---
layout:       post
title:        Running OWASP Zap from Docker
permalink:    docker-owasp-zap
tags:
  - cyber
  - docker
---

Thanks to **[this](https://medium.com/@deshanigeethika/how-to-run-owasp-zap-docker-image-b5338f1a4b2a)** article on **Medium**. To pull the **docker** image:
{% highlight text %}
docker pull owasp/zap2docker-stable
{% endhighlight %}

To run **Zap** in **Docker**:
{% highlight text %}
docker run -u zap -p 8080:8080 -p 8090:8090 -i owasp/zap2docker-stable zap-webswing.sh
{% endhighlight %}

With the **Zap** image running in docker, the Zap UI is accessible from a **browser**:
{% highlight text %}
http://localhost:8080/?anonym=true&app=ZAP
{% endhighlight %}