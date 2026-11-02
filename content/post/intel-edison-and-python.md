---
title: "Intel Edison and Python"
date: 2015-03-07
url: /post/intel-edison-and-python
index: true
---

<p style="text-align:center">
  <img alt="Intel Edison" src="/img/5029974.jpeg" style="max-width:300px;width:100%"/>
</p>

I have started working with the [Intel Edison](https://www-ssl.intel.com/content/www/us/en/do-it-yourself/edison.html) to bring machines with limited or no connectivity into the [IoT](http://en.wikipedia.org/wiki/Internet_of_Things).

This tiny platform comes ready to run Python, but if you work with Python, you want [pip](http://en.wikipedia.org/wiki/Pip_%28package_manager%29): an amazing package manager that gives us access to almost every Python library we will need.

## Getting Started

First, connect two micro-USB cables to the mini breakout board and open a Linux terminal.

The easiest way to talk to the board is through the serial port:

```bash
user@machine:~$ sudo screen /dev/ttyUSB0 115200
```

Press Enter and you'll get a login prompt. Log in as **root**; no password is needed. You are now connected to the Edison through the serial port.

Enabling WiFi is very easy. Just type:

```bash
root@edison:~# configure_edison --wifi
```

Wait a few seconds, select your network and enter your password.

## Add Unofficial Repository

The Intel Edison runs [Yocto](https://www.yoctoproject.org/), but pip is not in its official repositories, so we need to add the unofficial ones.

We begin by getting the latest [Intel® IoT Developer Kit libraries](https://software.intel.com/en-us/articles/managing-devkit-libraries-intel-edison-or-intel-galileo-board).

```bash
root@edison:~# echo "src intel-iotdk http://iotdk.intel.com/repo/1.1/intelgalactic" > /etc/opkg/intel-iotdk.conf
root@edison:~# opkg update
root@edison:~# opkg upgrade
```

Open the feeds file with `vi` and add the unofficial repositories:

```bash
root@edison:~# vi /etc/opkg/base-feeds.conf
```

Paste the following:

```conf
src/gz all http://repo.opkg.net/edison/repo/all
src/gz edison http://repo.opkg.net/edison/repo/edison
src/gz core2-32 http://repo.opkg.net/edison/repo/core2-32
```

Save the file and update:

```bash
root@edison:~# opkg update
```

## Install Pip

```bash
root@edison:~# opkg install python-pip
```

After running this command, everything seems fine. You have pip, so you should be able to install anything you want, for example [Flask](http://flask.pocoo.org/):

```bash
root@edison:~# pip install flask
Traceback (most recent call last):
  File "/usr/bin/pip", line 5, in <module>
    from pkg_resources import load_entry_point
ImportError: No module named pkg_resources
```

But we get an error. It means [setuptools](https://pypi.python.org/pypi/setuptools) is missing, so we need to install it. We can't simply fetch it with `wget`, because the Edison can't download over HTTPS. Instead, we can download it on our PC and copy it to the board with `scp`:

```bash
root@edison:~# scp user@machine:/home/user/Download/setuptools-12.2.tar.gz /home/root
root@edison:~# tar zxf setuptools-12.2.tar.gz
root@edison:~# python setuptools-12.2/ez_setup.py
```

Or take the insecure (but faster and easier) route:

```bash
root@edison:~# wget --no-check-certificate https://bootstrap.pypa.io/ez_setup.py
root@edison:~# python ez_setup.py --insecure
```

Once setuptools is installed, we can install whatever we need:

```bash
root@edison:~# pip install flask
...
Successfully installed flask
Cleaning up...
```
