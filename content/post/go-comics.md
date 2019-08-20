---
title: "Go Comics !!"
date: 2019-08-20
url: /post/go-comics
---

Do you like comics? I am sure that you do. You can find a lot of them on [The Eye web](https://the-eye.eu/public/Comics/), so I have been playing with a project using [Go](https://golang.org/) in order to download the comics to read them when you whish, *Comiccon*.

**Comiccon** is a toy project to download and keep updated comics. It takes advantage of the goroutines to download several comics at the same time. The limitation is the number of your CPUs. [Cobra](https://github.com/spf13/cobra) is the Go library utilized to build the CLI.


## Code

You can find the repository in [Github](https://github.com/mendrugory/comiccon).

## Installation

```bash
go get -u github.com/mendrugory/comiccon
```

## How to use it

### Help

```bash
$ comiccon help
```

### Download
```bash
$ comiccon download
```

If a comic is already downloaded, it will not be downloaded it again.

The configuration of the command is saved in a file called `config.json`.

### Optional flags

* *Base Folder*: Directory where the comics will be downloaded. It is created if it does not exist (default: current directory)
* *Extensions*: Extensions of the files which will be downloaded, separated by comma (default: cbr, jpg and pdf)
* *Link*: Sub link of the route if you only want to download a part of the collection (check the [list of collections](https://the-eye.eu/public/Comics/))

```bash
$ comiccon download --basefolder /tmp/comics --extensions cbr --link "DC Chronology"
```

## Docker

To run it using Docker, the only thing that we must have in mind is mapping the volumes in order to keep the comics.

```bash
$ docker run --rm -v /tmp/comics:/tmp/comics mendrugory/comiccon download --basefolder /tmp/comics --extensions cbr --link "DC Chronology"
```


Enjoy it !!