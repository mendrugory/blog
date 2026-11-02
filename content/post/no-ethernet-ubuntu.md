---
title: "No Ethernet after upgrading to Ubuntu 22.04 (Jammy Jellyfish)"
date: 2022-08-27
url: /post/no-ethernet-ubuntu
index: true
---

A few days ago, Ubuntu told me it could be upgraded to 22.04, the new LTS version called *Jammy Jellyfish*. I thought it was the right moment, so I went ahead. I have to say that I faced several issues, such as some problems with *Python3.10* that affected many applications (you know, Ubuntu uses Python a lot), but they were easy to solve with a few reinstalls. However, I was really upset when I saw that my Ethernet interface was not working.

The Ubuntu settings did not even detect the wired connection, but the interface was there:

```bash
$ sudo lshw -C network
  *-network DISABLED
       description: Ethernet interface
       ....
       logical name: enxe4b98
       ...
```

So Ubuntu had to bring up the interface at boot time and configure it using DHCP.


*/etc/network/interfaces*

```diff
# interfaces(5) file used by ifup(8) and ifdown(8)
auto lo
iface lo inet loopback

+# workaround for ethernet
+auto enxe4b98
+iface enxe4b98 inet dhcp
```

Then restart *NetworkManager*:

```bash
$ sudo systemctl restart NetworkManager
```

The interface was now detected by the Ubuntu settings, but it was unmanaged:

```bash
$ nmcli device
DEVICE           TYPE      STATE        CONNECTION 
enxe4b98         ethernet  unmanaged    --
```

We can apply a workaround to *NetworkManager* to make it work:

*/etc/NetworkManager/conf.d/allow-unmanaged-devices.conf*

```ini
[keyfile]
unmanaged-devices=*,except:type:wifi,except:type:gsm,except:type:cdma,except:type:ethernet
```

And restart *NetworkManager* again:

```bash
$ sudo systemctl restart NetworkManager
```

Now your Ubuntu machine will be connected to the Internet over Ethernet.

```bash
$ nmcli device
DEVICE           TYPE      STATE        CONNECTION 
enxe4b98         ethernet  connected    enxe4b98
```