---
layout: post
title: Different SSH Keys for Git
categories: [Git]
description:
keywords: git
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

The solution is original from https://superuser.com/a/1519694 , I copied it here for convenience.

# Using SSH keys for multiple accounts

If you need to connect to the same host with different keys then you can achieve it by:

Configure the ```~/.ssh/config``` with different Hosts but same HostNames.
Clone your repo using the appropriate host.

Example:

```
Host work
 HostName bitbucket.org
 IdentityFile ~/.ssh/id_rsa_work
 User git

Host personal
 HostName bitbucket.org
 IdentityFile ~/.ssh/id_rsa_personal
 User git
```

Then instead of cloning repos like:

```
git clone git@bitbucket.org:username/my-work-project.git
git clone git@bitbucket.org:username/my-personal-project.git
```

The host name should be used for cloning:

```
git clone git@work:username/my-work-project.git
git clone git@personal:username/my-personal-project.git
```


if the repository is already cloned (a public one) but would like to change to ssh key for commit, 

```bash
git remote set-url origin git@work:username/my-work-project.git
```

```bash
git config user.name "Mona Lisa"
git config user.email "MonaLisa@example.com"
```


```bash
git config --global core.editor "vim"
```
or 

```bash
export GIT_EDITOR=vim
```

[homepage](/)
