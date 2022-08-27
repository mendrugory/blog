---
title: "No Ethernet after upgrading to Ubuntu 22.04 (Jammy Jellyfish)"
date: 2022-08-27
url: /post/no-ethernet-ubuntu
index: true
---

Some days ago, my Ubuntu warned me that could be upgraded to 22.04, the new LTS version, called *Jammy Jellyfish*. I thought that was the moment and I proceeded. I had to say that I faced several issues, like some issues with *Python3.10* that affected many applications (you know, Ubuntu uses Python a lot) but it was easy to solve just making some re-installation. But I was really upset when I saw that my Ethernet interface was not working.

Ubuntu settings even did not detect the wired connection, but the interface was there:

```bash
$ sudo lshw -C network
  *-network DISABLED
       description: Ethernet interface
       ....
       logical name: enxe4b98
       ...
```

Therefore, Ubuntu had to start the interface at boot time and setting it up using DHCP.


*/etc/network/interfaces*

```diff
# interfaces(5) file used by ifup(8) and ifdown(8)
auto lo
iface lo inet loopback

+# workaround for ethernet
+auto enxe4b98
+iface enxe4b98 inet dhcp
```

And restart *NetworkManager*:

```bash
$ sudo systemctl restart NetworkManager
```

The interface was already detected by Ubuntu settings, but it was unmanaged:

```bash
$ nmcli device
DEVICE           TYPE      STATE        CONNECTION 
enxe4b98         ethernet  unmanaged    --
```

We could apply a workaround to *NetworkManager* in order to make it work:

*/etc/NetworkManager/conf.d/allow-unmanaged-devices.conf*

```
[keyfile]
unmanaged-devices=*,except:type:wifi,except:type:gsm,except:type:cdma,except:type:ethernet
```

And restart *NetworkManager*

```bash
$ sudo systemctl restart NetworkManager
```

Now, your Ubuntu will be connected to Internet using Ethernet.

```bash
$ nmcli device
DEVICE           TYPE      STATE        CONNECTION 
enxe4b98         ethernet  connected    enxe4b98
```