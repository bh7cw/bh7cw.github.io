---
title: Github practical skills
date: 2020-08-06 18:33:49
author: bh7cw
#top: true
cover: false
coverImg: /images/4.jpg
toc: true
summary: This post concludes pratical skills to use Github as a team member.
categories: Github
tags:
  - Github
---
## set up Github
- edit `.gitconfig`:
```
[user]
        email = myemail
        name = myname
[credential "https://github.com"]
        username = myusername
        helper = store
```
- edit `.git-credentials`:
```
https://<YOU>:<TOKEN>@github.com
```
## basic steps
```
git clone gitrepo
git status
git add . #add all
git add hello/* #Git add to subdirectory
git commit -m "commit message"
git pull origin master
git push origin master
```
## set remote branches
After fork a repository, we can set the push url, so we can push to our repo and pull from the orginal repo.
check the remote branches:
`git remote -v`
set the push url:
`git remote set-url --push origin mmyrepo`
Check set correctly:
``git remote -v``

## rebase
Before submit PR, it's always good to rebase and squash all the commits into one commit, so it's more convenient to be reviewed.
`git rebase -i HEAD~8` #combine the last 8 commits as one commit
use editor, like vim to edit:
  :i -> edit the rebase, pick the first one, and s the left
  :wq -> save and exit
keep those commits message to `rebase`, and others to `squash` or `s`
If we didn't get into the commit message automatically, we can continue:
`git rebase --continue`
  :i -> edit the whole commit mesage
  :wq -> save and exit
Keep those required message.
If we want to abort:
`git rebase --abort`

## Discard all local changes
Discard all local changes, but save them for possible re-use later: `git stash`
Discarding local changes (permanently) to a file: 
`git checkout -- <file>`
Discard all local changes to all files permanently: 
`git reset --hard`

## Sync forked repo to update
If forked repo is behind the orginal repo, we need to sync:
```
git remote add upstream https://github.com/upstream/repo.git
git pull --rebase upstream master
git push --force-with-lease origin master
```
## amend commits
If we need to amend our commits:
`git commit --amend`

Update more later.
