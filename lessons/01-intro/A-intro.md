---
title: "Introduction to Git"
description: "This is git, say hello"
---

## WHOAMI
The name is ThePrimeagen

<a href="https://twitch.tv/ThePrimeagen">
    Twitch
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 256 268" width="32" height="32">
        <path fill="#6441A4" d="M47.181 0L0 89.616v134.559h63.719V268l57.758-43.825h43.341L256 163.311V0H47.181zm178.947 147.268l-39.596 39.596h-29.343L108.839 215.2v-39.596H63.719V29.837h162.409v117.431zM89.616 89.616h29.343v59.075H89.616V89.616zm88.3 0h29.343v59.075h-29.343V89.616z"/>
    </svg>
</a>

<br>

<a href="https://youtube.com/ThePrimeTimeagen">
    ThePrimeagen - Engineering News
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 176 124" width="32" height="32">
        <path fill="#FF0000" d="M172.267 20.22c-2.029-7.717-7.974-13.786-15.69-15.814C143.915 1.135 88 1.135 88 1.135s-55.914 0-68.576 3.271c-7.717 2.029-13.662 8.097-15.691 15.814C1.135 32.882 1.135 62 1.135 62s0 29.118 2.598 41.78c2.029 7.717 7.974 13.786 15.691 15.814C32.086 122.865 88 122.865 88 122.865s55.915 0 68.577-3.271c7.716-2.028 13.661-8.097 15.69-15.814C174.865 91.118 174.865 62 174.865 62s0-29.118-2.598-41.78zM70.711 85.573V38.427l36.106 23.573-36.106 23.573z"/>
    </svg>
</a>

<br>

<a href="https://youtube.com/ThePrimeagen">
    ThePrimeagen - Hand Crafted
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 176 124" width="32" height="32">
        <path fill="#FF0000" d="M172.267 20.22c-2.029-7.717-7.974-13.786-15.69-15.814C143.915 1.135 88 1.135 88 1.135s-55.914 0-68.576 3.271c-7.717 2.029-13.662 8.097-15.691 15.814C1.135 32.882 1.135 62 1.135 62s0 29.118 2.598 41.78c2.029 7.717 7.974 13.786 15.691 15.814C32.086 122.865 88 122.865 88 122.865s55.915 0 68.577-3.271c7.716-2.028 13.661-8.097 15.69-15.814C174.865 91.118 174.865 62 174.865 62s0-29.118-2.598-41.78zM70.711 85.573V38.427l36.106 23.573-36.106 23.573z"/>
    </svg>
</a>

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## The Goal
The goal of this course is to be a course for anyone with no knowledge or a
decent amount.  We will step by step walk through quite a few operations and
use `log` and `reflog` and other parts of git to understand it.

Its designed to be a course for all levels of git knowledge

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Before we git we man
One of the best parts of using git is that all the documentation and all of the
features are extremely well documented.  This wasn't always the case.  In fact
git was known as being the most obtusely documented tool of all time, but alas
times have changed.

That means if you ever forget the exact command, you want to peruse for new
ways to use git, or you just need a good bed time reading, git's full
documentation is a few keyboard clicks away.

Lets execute the following command.  This will open up the man page for man.
If you don't know, man stands for manual.

```bash
man man
```

<br>
<br>

There is a surprisingly long manual for how to read the friendly manual (RTFM),
so to keep things short i'll give you some short cuts you need to know and that
is it.

* `j` goes one line down
* `k` goes one line up
* `d` goes one half page down
* `u` goes one half page down
* `/<term>` will search for term
* `n` goes to next search term
* `N` goes to prev search term

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### EXERCISE
In man pages you see **bold** terms.  Search for the term bold and find out
what it means.  Here is an example, from `man man`, of bolded terms

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

```bash
SYNOPSIS
       man [man options] [[section] page ...] ...
       man -k [apropos options] regexp ...
       man -K [man options] [section] term ...
       man -f [whatis options] page ...
       man -l [man options] file ...
       man -w|-W [man options] page ...
```

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## What is git?
Git is a distributed version control system, VCS.  Instead of the traditional
centralized control system where even checking out a file required admin
priviledges, git allows any work to be locally and may diverge as much as you
want.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Git
In Git, commands are divided into high level ("porcelain") commands and low
level ("plumbing") commands.

We will primarily use the high level commands, but will dip down into the low
level commands to really understand how git works.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Key Terms
* repo: a git tracked project
<br>
<br>

* commit: A point in time representing the project in its entirety

<br>
<br>
* index: I will use this term or staging area interchangeably.  From the github
  blog

> The Git index is a critical data structure in Git. It serves as the “staging area” between the files you have on your filesystem and your commit history. When you run git add , the files from your working directory are hashed and stored as objects in the index, leading them to be “staged changes”.

<br>
<br>
* squash: to take several commits and turn it into one commit
  - technically a squash would be taking N commits and turning it into N - 1 to
  1 commit.  but typically its N commits to 1 commit

<br>
<br>
* work tree, working tree, main working tree:  This is your git repo.  This is
the set of files that represent your project.  Your working tree is setup by
`git init` or `git clone`.

<br>
<br>
* untracked, staged, and tracked:
1. untracked files.  this means files that are not staged for the first time
   (indexed) or already committed / tracked by the repo.  These files are the
   easiest to accidentally lose work on since git does not have any information
   about these files.
<br>
<br>
1. indexed / staged: this is where the changes are _to be committed_.  You must
   stage before you commit and you stage changes by using the `git add`
   command.  see `man git-add` for more information
<br>
<br>
2. tracked.  These are files that are already tracked by git.  Now a file could
   be tracked AND have staged changes (changes stored in the index) ready to be
   committed.

<br>
<br>

* remote: The same git repo on another computer or directory.  You can accept
changes from or potentially push changes too.  Think github

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Key Facts About Git

<br>
<br>

* git is an acyclic graph, meaning the following cannot exist

<br>
<br>

  * in git, each commit is a node in the graph, and each pointer is the child
    to parent relationship

<br>
<br>

* if you delete _untracked_ files they are lost forever.  commit early, commit
often, you can always change history to make it one commit (squashing)

<br>
<br>

* `man git-<op>` for the friendly manual

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

