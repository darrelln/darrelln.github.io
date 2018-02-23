---
layout:       post
title:        Sniffing DNS Requests with Python and Scapy
permalink:    sniffing-dns-with-python-and-scapy
tags:
  - cyber
  - python
  - dev
---

**Scapy** is a **Python** networking library used for network investigation and diagnostics. It's extremely comprehensive and saying it can be used to generate and sniff network packets nowhere near does it justice. The source is available on [**Github**](https://github.com/secdev/scapy) and the documentation is available [**here**](https://scapy.readthedocs.io/). Below is a quick **Python** script to **sniff DNS requests** on the local machine and print the requested hostname to the console:

{% highlight python %}
from scapy.all import sniff, DNS

def callback(packet):
    try:
        if packet.haslayer(DNS):
            dns = packet.getlayer(DNS)
            qname = dns.qd.qname[:-1]

        print(qname.decode())
    except Exception as ex:
        print(ex)

sniff(iface=None, filter="udp dst port 53", prn=callback, count=0, store=0)
{% endhighlight %}
<br />
Run the script and request a site in a web browser or call **Python's socket gethostbyname()** method from the terminal:

![Sniffing DNS requests with Scapy](/assets/2018-02-22-sniffing-dns-with-python-and-scapy/socket_gethostbyname.png){:class="responsive-img center-image"}
<br />
Output is a simple print to the console:

![Sniffing DNS requests with Scapy](/assets/2018-02-22-sniffing-dns-with-python-and-scapy/Dns_sniff_output.png){:class="responsive-img center-image"}