---
title: "Sharing code to different Git Repositories"
date: 2021-12-22
url: /post/sharing-to-different-repos
---

[Git](https://git-scm.com/) is probably the most used version control system out there. It is full of features and gives a lot of flexibility to the people involved with the code. One of these features is the possibility of sharing the same code to different remote repositories. The use case can be really different: different audience targets, private and public repos and so on. Trying to share the same code in different places *by hand* can be a nightmare, instead, it is really easy thanks to git.

Let's imagine that we already have a local repository, therefore the first step is to add a remote repository. Usually, the main remote repository uses the ID `origin`.

```bash
$ git remote add origin git@github.com:mendrugory/myrepo.git
```

Next step is to add a secondary remote repositories with a different ID, in this case `upstream`:

```bash
$ git remote add upstream git@github.com:mendrugory/myotherrepo.git
```

Let's check the remote repositories:

```bash
$ git remote -v
origin          git@github.com:mendrugory/myrepo.git (fetch)
origin          git@github.com:mendrugory/myrepo.git (push)
upstream        git@github.com:mendrugory/myotherrepo.git (fetch)
upstream        git@github.com:mendrugory/myotherrepo.git (push)
```

From this moment, it will be possible to push code to every repository setting the right remote id (`-u` option)

```bash
$ git push -u <origin or upstream> <branch name>
```

But we would like to push code to both repositories at the same time. A new remote will be created for this purpose and the two repositories will be added to it with `push` capability:

```bash
$ git remote add both git@github.com:mendrugory/myrepo.git
$ git remote set-url --add --push both git@github.com:mendrugory/myrepo.git
$ git remote set-url --add --push both git@github.com:mendrugory/myotherrepo.git
```

Let's check the remote repositories again:

```bash
$ git remote -v
both            git@github.com:mendrugory/myrepo.git (fetch)
both            git@github.com:mendrugory/myrepo.git (push)
both            git@github.com:mendrugory/myotherrepo.git (push)
origin          git@github.com:mendrugory/myrepo.git (fetch)
origin          git@github.com:mendrugory/myrepo.git (push)
upstream        git@github.com:mendrugory/myotherrepo.git (fetch)
upstream        git@github.com:mendrugory/myotherrepo.git (push)
```

Now it would be possible to push to both repositories at the same time:

```bash
$ git push -u both <branch name>
```

>**NOTE**: *Pulling* code (`git pull`) from different remote repos at once is not possible, but you can always *fetch* the code from them (`git fetch --all`)
