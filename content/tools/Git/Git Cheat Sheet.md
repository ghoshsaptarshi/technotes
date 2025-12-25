---
title: Git Cheat Sheet
description: 
permalink: 
aliases: 
tags:
  - Git
draft: true
date: 2025-08-01
cssclasses:
---

## Setup & Config
Set a name that is identifiable for credit when review version history
```sh
git config --global user.name “[firstname lastname]”
```
Set an email address that will be associated with each history marker
```sh
git config --global user.email “[valid-email]”
```
Set automatic command line coloring for Git for easy reviewing
```sh
git config --global color.ui auto
```
System wide ignore pattern for all local repositories
```sh
echo "pattern" >> [file]
git config --global core.excludesfile [file]
```

## Staging & Commit

As a reminder -
![[tools/Git/index#Git Workflow]]

## Branch Handling


## Resources
- [GitHub Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)
- [GitLab Cheat Sheet](https://about.gitlab.com/images/press/git-cheat-sheet.pdf)