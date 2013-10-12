---
layout: post
title: The Git Add Command's Update Option
by: Mike Foster
type: post
category: vcs
date: 2013-07-29 20:00
--- 

The discovery of [hunk staging in Git](/vcs/2013/07/27/Git-Hunk-Staging.html) resulted in me spending some time looking through the other options available with [Git's add command](http://git-scm.com/docs/git-add).

The only option that jumped out at me was the update command (git add -u). The update command allows you to stage all tracked files for commit, but not any newly created, untracked files.

The situation that I found this command useful for was when deleting a lot of files or directories. The Terminal's auto-complete shortcuts tend to fail when the file no longer exists, which means the remove command (`git rm`) can be a bit tedious. The add command's update option allows you to stage all removed files at once.

It's not as paradigm shifting as hunk staging, but the add command's update option can certainly save you a bit of time in the right situation.