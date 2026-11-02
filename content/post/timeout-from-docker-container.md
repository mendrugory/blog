---
title: "Request's time-outs from Docker Containers"
date: 2022-10-14
url: /post/timeout-from-docker-container
---

While working on a really cool project with [Clear ML](https://clear.ml/), I had to figure out why it was not possible to install some Python packages from [pypi](https://pypi.org/). The first feedback I received was "the Docker image has a bug", but when I dug deeper, I saw that the behaviour was not related to the image, [nvidia/cuda](https://hub.docker.com/r/nvidia/cuda), or to the site, [pypi](https://pypi.org/). The strange behaviour mainly (but not only) affected `https`, and it also happened with other Docker images: [ubuntu](https://hub.docker.com/u/ubuntu), [nginx](https://hub.docker.com/u/nginx), ... Therefore, something was going on between the Docker daemon and the virtual machine.

The containers were running on [Genesis Cloud](https://www.genesiscloud.com/) Ubuntu instances, so as a first step, I checked the network interfaces:

```bash
$ ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc mq state UP group default qlen 1000
    link/ether 1b:36:33:cb:a5:f1 brd ff:ff:ff:ff:ff:ff
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether 0e:22:c2:16:c4:7f brd ff:ff:ff:ff:ff:ff
```

As we can see, the network interface `eth0` has an `mtu` (Maximum Transmission Unit) of 1450, while Docker's network bridge has an `mtu` of 1500 by default (a common value). For this to work, the network bridge's `mtu` cannot be greater than the physical one, so we can set `docker0` to an `mtu` of 1450 just by customizing `/etc/docker/daemon.json`.

*/etc/docker/daemon.json*

```json
{
    // Other parameters
    "mtu": 1450
}
```

Then restart the Docker service:

```bash
$ sudo systemctl restart docker
```

Now your containers will work properly.

Why does [Genesis Cloud](https://www.genesiscloud.com/) set the `mtu` to 1450 on their Ubuntu instances?
That's a question for them ☺️.
