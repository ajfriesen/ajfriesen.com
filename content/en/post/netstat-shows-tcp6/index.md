---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Netstat shows tcp6 on ipv4 only host"
subtitle: ""
summary: ""
authors: [admin]
tags: [networking,netstat, tcp6, ipv6,linux]
categories: []
date: 2020-05-19T08:28:59+02:00
lastmod: 2020-05-19T08:28:59+02:00
featured: false
draft: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

Why netstat shows tcp6 sockets even though you only have ipv4 configured.

<!--more-->

The other day I was building an AWS AMI (Amazon Machine Image) with packer which should run some java software.
The software itself does not matter.

While testing something I just wanted to check for the ports with netstat.
I got this output and was wondering why the ports where of type tcp6.
This was in AWS within a VPC which did not have ipv6 enabled.

```bash
[ec2-user@ip-172-17-9-250 ~]$ sudo netstat -natp
tcp6       0      0 :::8080                 :::*                    LISTEN      3861/java
tcp6       0      0 :::8082                 :::*                    LISTEN      3860/java
tcp6       0      0 127.0.0.1:1527          :::*                    LISTEN      3859/java
tcp6       0      0 :::2181                 :::*                    LISTEN      3858/java
```

I was also able to get a curl running against some web interface from anther machine with curl which only had ipv4 enabled as well.
That means die application itself was answering on ipv4.
Also, I know that ipv4 and ipv6 are not campatible at all.
If you wanted ipv4 server to speak with ipv6 and vice versa you need NAT64[^1], which translates ipv4 and ipv6 traffic.

I was confused...

It turns out the socket itself is an ipv6 socket. I also did not know that there is a special ipv6 address range which maps to ipv4 addresses.[^2]
That means all ipv4 adresses are also ipv6 adresses.[^3]

So these sockets work with ipv4 but since they are really ipv6 sockets they are listed as tcp6 in netstat.

The more you know.

[^1]: [Wikipedia - NAT64](https://en.wikipedia.org/wiki/NAT64)
[^2]: [Wikipedia - IPv4-mapped IPv6 addresses](https://en.wikipedia.org/wiki/IPv6#IPv4-mapped_IPv6_addresses)
[^3]: [StackExchange - netstat — why are IPv4 daemons listening to ports listed only in -A inet6?](https://unix.stackexchange.com/questions/152612/netstat-why-are-ipv4-daemons-listening-to-ports-listed-only-in-a-inet6)