---
title: git manipulate
date: 2019-11-17 11:05:25
tags: git
---

# 一、Related your site to github

Create repository on github named xxx.github.io(xxx according to your github account)

# 二、binding you domain

canme and A record

# 三、config ssh key

```bash
ssh-keygen -t rsa -C "email address"
```

adding your ~/.ssh/id_rsa.pub to your github SSH and GPG keys -> New SSH Key

Test if git config success

```bash
ssh -T git@github.com
```

> Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.

git config on you os

```bash
$ git config --global user.name "xxx"// github name
$ git config --global user.email  "xxx@gmail.com"// github register email
```
