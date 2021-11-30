---
layout: page
title: GIT CMDS
date: 2020-04-24T12:00:00.000+05:30
categories:
- misc
- git
lang: en
ref: git
published: false

---
## Git cmds.

Git commands I use a lot.

### Cleanup before a merge request

#### Squash commits (interactive)

    git rebase -i HEAD~3 

Effect: rerun the 3 last commits.  
Do: follow the interactive indication. 

#### Rewrite commits

    git reset (--soft) HEAD~3

Effect: remove the last 3 commits, but keep the changes  
use _--soft to_ keep the changes in the staging area  
Do: create new commits

#### Change last commit message

    git commit --amend -m "New Message"

### Cheery pick some changes

    git cherry-pick <SHA-1>

Use `--no-commit`  or `-n` to pick the change without committing. 

### Quick stash

`--autostash` works with most of the commands.

Does not work with checkout, so: 

    git stash && git fetch && git checkout my_branch && git stash pop

#### Download a single file

    git fetch --all
    git checkout origin/myBranch -- my/file.cpp