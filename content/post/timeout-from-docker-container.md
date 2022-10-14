---
title: "Request's time-outs from Docker Containers"
date: 2022-10-14
url: /post/timeout-from-docker-container
---

Working in a really cool project with [Clear ML](https://clear.ml/), I had to figure out why it was not possible to install some Python packages from [pypi](https://pypi.org/). First feedback I received was "docker image has a bug", but when I went deeper, I saw that behaviour was not related to the image, [nvidia/cuda](https://hub.docker.com/r/nvidia/cuda), or to the site, [pypi](https://pypi.org/). The strange behaviour affected, mainly but not always, to `https`, but also from other docker images: [ubuntu](https://hub.docker.com/u/ubuntu), [nginx](https://hub.docker.com/u/nginx), ... Therefore, something was happening between the docker daemon and the virtual machine.

Containers were running into [Genesis Cloud](https://www.genesiscloud.com/) Ubuntu instances, so as first step, I check out the network interfaces:

```bash
$ ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc mq state UP group default qlen 1000
    link/ether 1b:36:33:cb:a5:f1 brd ff:ff:ff:ff:ff:ff
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether 0e:22:c2:16:c4:7f brd ff:ff:ff:ff:ff:ff
```

As we can see, the network interface `eth0` has a `mtu` (Maximum Transmission Unit) of 1450 when the Docker's network bridge has a `mtu` of 1500 by default (and it is a common value). In order to make it work, network bridge's `mtu` can not be greater than the physical one, therefore, we could set up `docker0` to have a mtu of 1450 just customizing `/etc/docker/daemon.json`.

*/etc/docker/daemon.json*

```json

{
    // Other parameters
    "mtu": 1450
}
```

And restart your docker service:

```bash
$ sudo systemctl restart docker
```

Now, your containers will properly work.

Why does [Genesis Cloud](https://www.genesiscloud.com/) configure `mtu` to 1450 in their ubuntu instances?
That's a question for them ☺️.
