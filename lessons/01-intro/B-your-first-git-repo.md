---
title: "Your First Git Repo"
description: "Lets create and checkout git!"
---

## Your First Git Repo
Its time to git with it

<br>
<br>

I am sure for most of you this is not your first git repo, but to make sure
that no one is confused, lets spend 5 minutes going over the basic commands of
a git repo.

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

### Draw Git
(use excalidraw)

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

## Configuring your git experience
Before we begin, we need to configure git to contain _your_ information.  Every
commit comes with the author's name and email.  So to ensure you get proper
credit and/or blame for all the wonderful code you add, we need to set our name
and email!

<br>
<br>

### git config
Git comes with a config that is global, for all projects, and local, project
level.  There are other git config levels, but they are irrelevant for the
average git experience.  project level config overrides global level config as
its more specific.

This would be akin to javascript's `Object.assign`

```javascript
const config = Object.assign({}, globalConfig, localConfig);
```

<br>
<br>

#### Key Facts
* All git config keys are in the following shape: `<section>.<key>`.
* `--global` flag will ensure you set this key value for all future uses of git and repos
* `user.name` and `user.email` are the key's used in creating a commit tied to you
* to add a key value, execute `git config --add --global <key> "<value>"`
* you can view any value of git config by executing `git config --get <key>`

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

### Problem
Add your `user.name` and `user.email` _IF_ you don't already have values for
them.

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

### Solution
Lets pretend that when i execute:

```bash
➜  git config --get user.name
➜
```

I get nothing out, therefore i need to add both my `user.name` and `user.email`

```bash
git config --add --global user.name "ThePrimeagen"
git config --add --global user.email "the.primeagen@aol.com"
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

## Creating A New Repo
The very first step of any project is to create a repo.  To create a repo we
use `git init`.  There are some options to creating repos but they are out of
the scope of this and largely will never be used in your entire career

To create a new git repo you first have to create a new directory or use an
existing one with no version control.

Once inside the directory type `git init` and it will create a `.git` folder

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

### Problem
Create a new git repo at `/path/to/new/my-first-git-repo`.  Replace
`/path/to/new` to your preferred project location.

<br>
<br>

If you know what you are doing and have done this before i encourage you to
follow along, we will be doing some exercises in this repo

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

### Solution
The higher level steps are:
* `cd` to `/path/to/new`
* `mkdir` a new directory `my-first-git-repo`
* `cd` into the new directory
* call `git init`

Terminal commands:
```bash
cd /path/to/new
mkdir my-first-git-repo
cd my-first-git-repo

git init
```

#### Note
If you have an unconfigured git default branch you may see the following notice

```bash
➜  my-first-git git init
hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint:
hint:   git config --global init.defaultBranch <name>
hint:
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint:
hint:   git branch -m <name>
Initialized empty Git repository in /home/ThePrimeagen/personal/my-first-git/.git/
```

We will address config more holistically later on.

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

## The Files Are IN the Git Repo
Every git repo comes with a directory that contains all of the state of the git
repo.  This repo is found in `.git`

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

### Problem
Validate that you have created a new git repo by listing out the files and
directories in the git state directory.

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

### Solution
```bash
➜  my-first-git git:(master) find .git
.git
.git/info
.git/info/exclude
.git/HEAD
.git/branches
.git/description
.git/refs
.git/refs/tags
.git/refs/heads
.git/objects
.git/objects/info
.git/objects/pack
.git/config
.git/hooks
.git/hooks/pre-rebase.sample
.git/hooks/prepare-commit-msg.sample
.git/hooks/post-update.sample
.git/hooks/commit-msg.sample
.git/hooks/pre-commit.sample
.git/hooks/pre-applypatch.sample
.git/hooks/pre-merge-commit.sample
.git/hooks/applypatch-msg.sample
.git/hooks/pre-push.sample
.git/hooks/push-to-checkout.sample
.git/hooks/fsmonitor-watchman.sample
.git/hooks/update.sample
.git/hooks/pre-receive.sample
```

<br>
<br>

You may not like this, but this is peak empty git repo with all the files and
sample code needed to get your config up and running

<br>
<br>

#### Note
Git keeps its state by using a series of files and directories.  We will go
into more detail shortly

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

### The Basics
The next couple commands will be 80% of the commands you will execute when
using git.  80% of git is exceptionally easy.  Its the 20% that can get you
into trouble :7

<br>
<br>

* `git add <path-to-file | pattern>` will add zero or more files to _the index_
(staging area)
* `git commit -m '<message>'` will commit what changes are present in _the
index_.
* `git status` will describe the state of your git repo which will include
tracked, staged, and untracked changes.

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

### Problem
We want to trace the steps of git from untracked to tracked.

<br>
<br>

#### Command reminder
```bash
git add <path-to-file | pattern>
git commit -m '<message>'
git status
```

<br>
<br>

* create a file `first.md`
* check the status of git to see that its untracked.
* add first.md to the staging area
* check the status of git to see that its now tracked in the staging area
* commit with a friendly message
* check the status of git to see there are no changes pending or untracked

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

### Solution
#### 1. Create a new file
```bash
vim first.md # or whatever editor you would like to use
```

add a simple change

<br>
<br>

#### 2. Check the status

```bash
git status
```

You should see something similar to the following.  Noticed that we only have
`untracked` files.  Meaning our git repo has no information on `first.md`

```bash
➜  my-first-git git:(master) git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        first.md

nothing added to commit but untracked files present (use "git add" to track)
```

<br>
<br>

#### 3. stage first.md

```bash
git add first.md
```

With add you can use any glob to add files. Be careful, you may end up adding
files you do not want to.  This will require you to unstage them before
committing.


<br>
<br>

#### 4. validate the file is now staged

```bash
➜  my-first-git git:(master) git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   first.md
```

<br>
<br>

#### 5. commit

```bash
git commit -m 'my first commit'
[master (root-commit) 33d6d96] my first commit
 1 file changed, 2 insertions(+)
 create mode 100644 first.md

git status
On branch master
nothing to commit, working tree clean
```

We have officially created our first commit!  Congrats if this is your first
time.

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

### What Changes Are In A Repo
A common activity of the 20% is to see what the past changes are within a repo.
We will dive deeper into this operation later, but for now, lets scratch the
surface.

<br>
<br>

`git log` has many options to make viewing of history a pleasant experience.

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

### Review or Explore
Review `man git-log` and search for `--graph` and `--decorate`.  Quickly find
and read the description of each option

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

### Problem
Display the history of this repo with the `--graph` and `--decorate` option.

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

### Solution
```bash
git log --graph --decorate

* commit 33d6d9645ac045c6d99eba9d6706d0d48421e582 (HEAD -> master)
  Author: ThePrimeagen <the.primeagen@gmail.com>
  Date:   Sun Jan 21 10:52:45 2024 -0700

      my first commit

```

<br>
<br>

The decorate option, if you didn't understand from description, means that it
puts branches / HEAD in the commit logs so you can see in a friendly way where
your branches are pointed at.

<br>
<br>

If you don't know what a branch is, that is ok, we will get there.

<br>
<br>

Unfortunately we only have one commit, therefore our `--graph` option is less
impressive.  Don't worry we will make extensive use.  What does show up is the
`*` showing the graph has only one node in the output

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

