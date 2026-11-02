---
title: "The Rise Of Microservices"
date: 2016-01-03
url: /post/the-rise-of-microservices
---

Who hasn't heard about [microservices](https://en.wikipedia.org/wiki/Microservices)?

<p style="text-align:center">
  <img alt="Microservices" src="/img/8000030_orig.png" style="max-width:300px;width:100%"/>
</p>

Microservices is a software architecture approach that tries to decouple functionalities, which are then served by very focused, small systems. When I say small, I mean smaller. These systems provide their functionalities through a well-known [API](https://en.wikipedia.org/wiki/Application_programming_interface).

Therefore, this approach helps us scale our software systems by breaking them up (goodbye, monolithic system problems!) into several independent systems, each very focused on its task (hello, distributed computing problems!). It is important to say that every approach has its advantages and disadvantages.

Why are microservices so popular, even for non-heavy systems, if they mean having to look after more systems? In my opinion, there are two factors: Internet service providers and container technology.

## Internet Service Providers

Nowadays it is very easy to get a server from any of the available Internet service providers: [Amazon Web Services](https://aws.amazon.com), [Digital Ocean](https://www.digitalocean.com/), [OVH](https://www.ovh.com/), ... (easier with some of them, like AWS or Digital Ocean). It is also very cheap (an OVH instance for €2.99!). We don't need to buy very expensive servers and have skilled people only to set them up and look after them. This greatly reduces our IT infrastructure costs and gives us the opportunity to use our skilled people to create amazing things instead of wasting their time on boring tasks.

## Container Technology

Linux Containers, or [LXC](https://linuxcontainers.org/lxc/introduction/), give us the possibility of running several isolated Linux systems on a single Linux machine. The idea alone is awesome. We can keep our applications in images and create as many containers (running applications) as we need; hence, the behavior we get on our development machine will be the same behavior we see on the production machine.

However, it was the birth of [Docker](https://www.docker.com/) that opened the world of containers to everybody. Docker makes handling images and containers very easy, and it even offers an open image store, [Docker Hub](https://hub.docker.com), for free. But we all agree that we need a private image store, because not all our code is open source. In that case, we can install a [Docker registry](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-private-docker-registry-on-ubuntu-14-04) on our private server.

## Conclusion

Nowadays it is very easy to have small, focused applications running in containers, created from images, on very cheap servers (usually a [VPS](https://en.wikipedia.org/wiki/Virtual_private_server)). That is all we need to build great things using a microservices architecture; the rest depends on you!
