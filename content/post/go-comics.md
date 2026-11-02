---
title: "Go Comics !!"
date: 2019-08-20
url: /post/go-comics
---

Do you like comics? I am sure you do. You can find a lot of them on [The Eye website](https://the-eye.eu/public/Comics/), so I have been playing with a [Go](https://golang.org/) project, *Comiccon*, to download them so you can read them whenever you wish.

**Comiccon** is a toy project to download comics and keep them up to date. It takes advantage of goroutines to download several comics at the same time, limited by the number of CPUs you have. The CLI is built with the Go library [Cobra](https://github.com/spf13/cobra).


## Code

You can find the repository on [GitHub](https://github.com/mendrugory/comiccon).

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

If a comic has already been downloaded, it will not be downloaded again.

The command's configuration is saved in a file called `config.json`.

### Optional flags

* *Base Folder*: Directory where the comics will be downloaded. It is created if it does not exist (default: current directory).
* *Extensions*: Extensions of the files to download, separated by commas (default: cbr, jpg and pdf).
* *Link*: Sub-link of the route, if you only want to download part of the collection (check the [list of collections](https://the-eye.eu/public/Comics/))

```bash
$ comiccon download --basefolder /tmp/comics --extensions cbr --link "DC Chronology"
```

## Docker

To run it with Docker, the only thing we must keep in mind is mapping the volumes so the comics are kept.

```bash
$ docker run --rm -v /tmp/comics:/tmp/comics mendrugory/comiccon download --basefolder /tmp/comics --extensions cbr --link "DC Chronology"
```


Enjoy!!