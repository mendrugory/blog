---
title: "Intel Edison and Python"
date: 2015-03-07
url: /post/intel-edison-and-python
index: true
---

<p style="text-align:center">
  <img alt="Intel Edison" src="/img/5029974.jpeg" style="max-width:300px;width:100%"/>
</p>

I have begun to work with [Intel Edison](https://www-ssl.intel.com/content/www/us/en/do-it-yourself/edison.html) in order to adapt machines with limited or even no connectivity to the movement of [IoT](http://en.wikipedia.org/wiki/Internet_of_Things).

This tiny platform is already prepared to work with Python but, if you want to work with Python, you want to work with [pip](http://en.wikipedia.org/wiki/Pip_%28package_manager%29). Pip is an amazing package manager where we can find almost all the Python libraries that we are going to use.

## Getting Started

Firstly we connect two micro-USB cables to the mini breakout board and we run a Linux terminal.

The easiest way to communicate with the board is through the serial port:

```
user@machine:~$ sudo screen /dev/ttyUSB0 115200
```

Press Enter and you'll get a login prompt. Type **root** and no password is needed. Now you are connected to the Edison through the serial port.

To activate the WiFi is very easy, type:

```
root@edison:~# configure_edison --wifi
```

Wait for a few seconds, select your network and type your password.

## Add Unofficial Repository

Intel Edison is powered by [Yocto](https://www.yoctoproject.org/) but pip is not in the official repositories, therefore we need to add the unofficial repositories.

We begin by getting the latest [Intel® IoT Developer Kit libraries](https://software.intel.com/en-us/articles/managing-devkit-libraries-intel-edison-or-intel-galileo-board).

```
root@edison:~# echo "src intel-iotdk http://iotdk.intel.com/repo/1.1/intelgalactic" > /etc/opkg/intel-iotdk.conf
root@edison:~# opkg update
root@edison:~# opkg upgrade
```

Open `vi` and add the unofficial repositories:

```
root@edison:~# vi /etc/opkg/base-feeds.conf
```

Paste the following:

```
src/gz all http://repo.opkg.net/edison/repo/all
src/gz edison http://repo.opkg.net/edison/repo/edison
src/gz core2-32 http://repo.opkg.net/edison/repo/core2-32
```

Close the file and update:

```
root@edison:~# opkg update
```

## Install Pip

```
root@edison:~# opkg install python-pip
```

After running the above command, everything seems to be fine. You already have your pip and you will be able to install everything that you want, for example [Flask](http://flask.pocoo.org/).

```
root@edison:~# pip install flask
Traceback (most recent call last):
  File "/usr/bin/pip", line 5, in <module>
    from pkg_resources import load_entry_point
ImportError: No module named pkg_resources
```

But we get an error. This error means that we do not have [setuptools](https://pypi.python.org/pypi/setuptools), so we try to install it. If we try to use `wget`, the Edison will not be able to download it because it cannot download from https. Therefore, we could download it to our PC and use `scp` to copy the library to the board.

```
root@edison:~# scp user@machine:/home/user/Download/setuptools-12.2.tar.gz /home/root
root@edison:~# tar zxf setuptools-12.2.tar.gz
root@edison:~# python setuptools-12.2/ez_setup.py
```

Or use the insecure (but faster and easier) way:

```
root@edison:~# wget --no-check-certificate https://bootstrap.pypa.io/ez_setup.py
root@edison:~# python ez_setup.py --insecure
```

After having installed setuptools we can install whatever we need:

```
root@edison:~# pip install flask
...
Successfully installed flask
Cleaning up...
```
