---
title: "Sharing code to different Git Repositories"
date: 2021-12-22
url: /post/sharing-to-different-repos
---

[Git](https://git-scm.com/) is probably the most used version control system out there. It is full of features and gives a lot of flexibility to the people working with the code. One of these features is the possibility of pushing the same code to different remote repositories. The use cases can vary a lot: different target audiences, private and public repos and so on. Trying to share the same code in different places *by hand* can be a nightmare, but it is really easy thanks to git.

Let's imagine that we already have a local repository, so the first step is to add a remote repository. Usually, the main remote repository uses the ID `origin`.

```bash
$ git remote add origin git@github.com:mendrugory/myrepo.git
```

The next step is to add a secondary remote repository with a different ID, in this case `upstream`:

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

From now on, it is possible to push code to either repository by setting the right remote ID (`-u` option):

```bash
$ git push -u <origin or upstream> <branch name>
```

But we would like to push code to both repositories at the same time. We create a new remote for this purpose and add both repositories to it as `push` URLs:

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

Now we can push to both repositories at the same time:

```bash
$ git push -u both <branch name>
```

>**NOTE**: *Pulling* code (`git pull`) from different remote repos at once is not possible, but you can always *fetch* the code from all of them (`git fetch --all`).
