---
title: "Working with different GIT users"
date: 2019-10-15
url: /post/working-with-different-git-users
description: How to deal with permission problems when you have different GIT users for the same GIT service.
---

Having several users in a GIT hosting service is really normal: a user for your professional projects, a user for a company that you work with/for, a user for your pet projects and so on. Different users need different registered SSH keys in the same GIT service provider, so your PC can raise some messages like the following when you try to clone a project:

```bash
Permission denied, please try again.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

You know that you have the right access, because you can check it in their web client after being logged with the right user. But your git cli is using another client. How can you solve it without changing your global GIT settings?

We are going to indicate the right *user*, *email* and especially the right *SSH key* that has to be used for this project:

> I assume that you work with Linux/Mac

#### Project folder

```bash
$ mkdir <Project Name>
$ cd <Project Name>
```

#### Creation of the GIT repository

```bash
$ git init
```

#### Right user association

```bash
$ git config user.name <Your name>
$ git config user.email <Your email>
$ git config core.sshCommand "ssh -i <Path to your SSH key> -F /dev/null"
```

#### Adding the remote

```bash
$ git remote add origin <Project Remote URL>
```

#### Pull the master branch

```bash
$ git pull origin master
```


Using this solution you will directly associate this GIT repository to the right GIT user without affecting other GIT configuration in your PC.