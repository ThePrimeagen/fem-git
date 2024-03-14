---
title: "Introduction to Git"
description: "This is git, say hello"
---

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
![Cycle](./00-no-cycles.png)

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

