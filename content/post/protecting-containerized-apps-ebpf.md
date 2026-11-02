---
title: "Protecting Containerized Applications using eBPF"
date: 2022-10-19
url: /post/protecting-containerized-apps-ebpf
---

Most applications are moving to containers because of the big benefits this approach gives us: scalability, immutability, efficiency, and so on. But adding abstraction layers can make other tasks much more difficult. Security is one of them.

Today, I would like to imagine a scenario where our web server is running in a container. It is running smoothly until we detect that we are under a [DDoS attack](https://www.cloudflare.com/learning/ddos/what-is-a-ddos-attack/). While our server is under attack, users can experience long latencies or may not be able to access the service at all. In fact, if the attack is really heavy for a long period of time, our server could go down.

The first idea that may come to mind is to work with [netfilter](https://www.netfilter.org/projects/iptables/) (iptables). But we would like a solution that performs better and, in this case, only affects our containerized application. What about eBPF?

> [eBPF](https://ebpf.io/) is a revolutionary technology with origins in the Linux kernel that can run sandboxed programs in an operating system kernel. It is used to safely and efficiently extend the capabilities of the kernel without requiring to change kernel source code or load kernel modules (source [https://ebpf.io/](https://ebpf.io/)).

Thanks to eBPF, we will be able to run our programs in the Linux kernel, improving performance considerably. As we are suffering a DDoS attack, whose purpose is to consume server resources, our goal is to drop the attackers' packets as early as possible, so we do not waste resources and they stay available for our users.

In order to execute our program as early as possible, there is a framework called [XDP](https://en.wikipedia.org/wiki/Express_Data_Path), which allows us to hook our program into the network interface controller (NIC), so we can check the received packets and decide whether each one should be dropped or not.

> Some tests show that XDP allows us to drop up to 26 million packets per second per core with commodity hardware ([source](https://github.com/tohojo/xdp-paper/blob/master/benchmarks/bench01_baseline.org#initial-data-from-jespers-runs)).

## Experiment

### Server

Let's build a simple web server. First, we need an `index.html`:

```html
<html><head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<title>Web Server</title>
</head>
<body cz-shortcut-listen="true">
<h1>Hello !!</h1>
</body></html>
```

The application will run in a [user-defined docker bridge network](https://docs.docker.com/network/bridge/#manage-a-user-defined-bridge):

```bash
$ docker network create mynetwork
8838734f993f
```

Now, we can launch our server:

```bash
$ docker run \
    --rm \
    --name server \
    --net mynetwork \
    -p 8000:8000 \
    -w /html \
    -v `pwd`:/html:ro \
    python:slim \
    python -m http.server  
```

Apart from the logs, we can also check the web server on port 8000:

```bash
$ curl localhost:8000
<html><head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<title>Web Server</title>
</head>
<body cz-shortcut-listen="true">
<h1>Hello !!</h1>
</body></html>
```

## Attackers

To simulate a (really small) DDoS attack, we are going to open several shells and run the following in each one:

```bash
docker run \
    --net mynetwork \
    --rm \
    busybox /bin/sh -c "while true; do /bin/wget -qO- http://server:8000; /bin/sleep 1; done"
```

The attackers will be connected to the same network as the server, which is why the server is reachable just by using its container name, `server`. The attack consists of each attacker sending a request to the server every second.

At this point, the server will show the following logs:

```bash
172.29.0.3 - - [17/Oct/2022 14:16:56] "GET / HTTP/1.1" 200 -
172.29.0.3 - - [17/Oct/2022 14:16:57] "GET / HTTP/1.1" 200 -
172.29.0.4 - - [17/Oct/2022 14:16:58] "GET / HTTP/1.1" 200 -
172.29.0.3 - - [17/Oct/2022 14:16:58] "GET / HTTP/1.1" 200 -
172.29.0.4 - - [17/Oct/2022 14:16:59] "GET / HTTP/1.1" 200 -
```

In our case, we have a couple of attackers whose IPs are `172.29.0.3` and `172.29.0.4`.

## Protector

Our protector code will use the [BCC](https://github.com/iovisor/bcc) library, which lets us write our eBPF program in C and our loader in Python. This loader will also be able to interact with the eBPF program to exchange data.

First, we are going to code the frontend in Python.

```python
interface = "eth0"

b = BPF(src_file="ip_blocking.c")
fn = b.load_func("block", BPF.XDP)
b.attach_xdp(dev=interface, fn=b.load_func("block", BPF.XDP))
```

The first part is related to the loader. We decide which interface we are going to hook our eBPF program into, `eth0`. The eBPF program is in a file called `ip_blocking.c`, and the callback that will receive the event when a new packet arrives at the NIC is a function called `block`.

```python
def _ip_to_bin(dotted_ip):
    ip = ipaddress.ip_address(dotted_ip)
    return ctypes.c_uint(int.from_bytes(ip.packed, "little"))


def _load_cache(dotted_ips):
    print("loading IPs...")
    cache = b.get_table("cache")
    for dotted_ip in dotted_ips:
        ip = ip_to_bin(dotted_ip)
        print(f"{dotted_ip}: {ip}")
        cache[ip] = ctypes.c_char(1) 
```

We need two helper functions: `_ip_to_bin` and `_load_cache`.

The first one transforms a dotted-string IPv4 address into a little-endian binary integer, which is the format the address will have in the received packet.

`_load_cache`, as its name says, loads the attackers' IPs into a cache. This cache will also be accessed by the eBPF program, giving us a mechanism to communicate between the kernel namespace (eBPF program) and user namespace (frontend written in Python).

```python
def run(ips):
    _load_cache(ips)

    try:
        print("running...")
        b.trace_print()
    except KeyboardInterrupt:
        print("going out !!")
    finally:
        b.remove_xdp(interface, 0)
```

`run` contains the high-level logic:

* Load cache with the given IPs
* Print traces from eBPF program
* Unload eBPF program after finishing

```python
if __name__ == "__main__":
    if len(sys.argv) == 1:
        print("No IPs have been provided")
        exit(1)
    run(sys.argv[1:])
```

Finally, in the main block, we check that IPs have been passed as arguments and call `run`.

*Full code*:

```python
import ctypes
import ipaddress
import sys

from bcc import BPF


interface = "eth0"

b = BPF(src_file="ip_blocking.c")
fn = b.load_func("block", BPF.XDP)
b.attach_xdp(dev=interface, fn=b.load_func("block", BPF.XDP))


def _ip_to_bin(dotted_ip):
    ip = ipaddress.ip_address(dotted_ip)
    return ctypes.c_uint(int.from_bytes(ip.packed, "little"))


def _load_cache(dotted_ips):
    print("loading IPs...")
    cache = b.get_table("cache")
    for dotted_ip in dotted_ips:
        ip = ip_to_bin(dotted_ip)
        print(f"{dotted_ip}: {ip}")
        cache[ip] = ctypes.c_char(1)    


def run(ips):
    _load_cache(ips)

    try:
        print("running...")
        b.trace_print()
    except KeyboardInterrupt:
        print("going out !!")
    finally:
        b.remove_xdp(interface, 0)


if __name__ == "__main__":
    if len(sys.argv) == 1:
        print("No IPs have been provided")
        exit(1)
    run(sys.argv[1:])
```

Now, the eBPF program.

```c
BPF_HASH(cache, unsigned int, char, 128);
```

The first step is declaring the cache that we already talked about while coding the frontend.

```c
int block(struct xdp_md *ctx) {
    void *data = (void *)(long)ctx->data;
    void *data_end = (void *)(long)ctx->data_end;
    struct ethhdr *eth = data;
    
    if ((void *)eth + sizeof(*eth) <= data_end) {
        struct iphdr *ip = data + sizeof(*eth);
        if ((void *)ip + sizeof(*ip) <= data_end) {
            if (ip->protocol != IPPROTO_TCP) {
                return XDP_PASS;
            }

            unsigned int address = ip->saddr;
            char *value = cache.lookup(&address);
            if (value != NULL) {
                bpf_trace_printk("blocking %d...\n", address);
                return XDP_DROP;
            }
        }
    }

    return XDP_PASS;
}
```

The `block` function (the event callback) is very straightforward. It receives a `struct xdp_md` as an argument which, after some casting, we use to extract the Ethernet data. Some data length checks are required, especially to avoid compiler complaints, and then we can proceed to work with the IP data. The blocking logic only affects `TCP`, so if the protocol is not `TCP`, the packet can go on. Then, we check whether the received `source address` is in the cache. If it is, we drop the packet. In any other case, the packet is allowed to go on.

*Full code*:

```c
#include <linux/bpf.h>
#include <linux/if_ether.h>
#include <linux/ip.h>
#include <linux/in.h>


BPF_HASH(cache, unsigned int, char, 128);

int block(struct xdp_md *ctx) {
    void *data = (void *)(long)ctx->data;
    void *data_end = (void *)(long)ctx->data_end;
    struct ethhdr *eth = data;
    
    if ((void *)eth + sizeof(*eth) <= data_end) {
        struct iphdr *ip = data + sizeof(*eth);
        if ((void *)ip + sizeof(*ip) <= data_end) {
            if (ip->protocol != IPPROTO_TCP) {
                return XDP_PASS;
            }

            unsigned int address = ip->saddr;
            char *value = cache.lookup(&address);
            if (value != NULL) {
                bpf_trace_printk("blocking %d...\n", address);
                return XDP_DROP;
            }
        }
    }

    return XDP_PASS;
}
```

Once the code is ready, we only need to execute it. To make this easier, I have prepared a [docker image](https://hub.docker.com/r/mendrugory/bcc).

When a new container connected to a user-defined bridge network is created, a pair of virtual network interfaces is created as well: one in the container and the other in the host. Their only job is to communicate between the different network namespaces.

![user defined bridge](/img/user-defined-bridge.png)

The protector, which is going to be a Docker container, needs access to the network interface of the server container, so we will connect it to the server's network namespace (`--net container:server`).

```bash
docker run \
    -it \
    --rm \
    -v `pwd`:/code \
    -v /lib/modules:/lib/modules:ro \
    -v /sys:/sys:ro \
    -v /usr/src:/usr/src:ro \
    --net container:server \
    --privileged \
    -w /code \
    mendrugory/bcc python3 ip_blocking.py <IPs of attackers>
```

This container has to run as `privileged` and share some directories: `/lib/modules`, `/sys` and `/usr/src`. Then, after passing our code through a volume, we can just run it.

Although our code could be added to a custom image, we will use a volume to share it with the container. That way, we can adapt it if necessary.

## Demo

<div style="margin-top: 5px;max-width:100%;text-align: center;">
<iframe width="560" height="315" style="margin-top: 5px;max-width:100%" src="https://www.youtube.com/embed/EDkCUXRpV5o" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

## Code

All the code can be found [here](https://github.com/mendrugory/containerized-application-ddos-protector).
