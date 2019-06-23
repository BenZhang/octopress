---
layout: post
title: "Git checkout"
date: 2012-08-27 11:06
comments: false
categories: GIT
---
Git is a very powerfully version control tool. Here is a summary how to make use of git checkout.
## Switch branch
When we type 
```bash
git branch
```
in a git repo, we will get result like this

```bash
* master
  develop
```
If we want to switch to develop, we can simple checkout it.
```bash
git checkout develop
```

## Switch commit
When we check the git log, it will list commits we have made so far. For example
```bash
commit 20f9981e35eee840207bfcab423a8056e648ae3f
Author: Ben Zhang <bzbnhang@gmail.com>
Date:   Thu Aug 9 12:34:41 2012 +0930

    add new feature #436
```
We can refer to this commit using the hash code, in addition partial of hash code, such as 20f99. Therefore we can checkout whatever commit we like using hash code.
```bash
git checkout 20f99

* (no branch)
  master
```
When we checkout a commit, we are actually not in any branch.

## Checkout tag
We can give name to our commits by using git's tag feature. We can set a tag to the current commit by using 
```bash
git tag -a tag_name
```
It would be so handy if we want to go to a specific tag without checking the history. We can simply checkout this commit by using the tag name
```bash
git checkout tag_name
```

## Checkout partial files

What if we want to checkout partial files from other branches or commits? Cherry-pick would be an common option, but we can make use the checkout command. Suppose we are in master branch, and we want to bring the index.html file from develop branch. We can do this
```bash
git checkout develop index.html
```
Typing in git branch, we will find out we are still in master branch, but index.html has been updated.

## Summary
Git checkout is very handy. Feel free to shoot me an Email if you know any other cool features.