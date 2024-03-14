## Your First Git Repo
I am sure for most of you this is not your first git repo, but to make sure
that no one is confused, lets spend 5 minutes going over the basic commands of
a git repo.

## Configuring your git experience
Before we begin, we need to configure git to contain _your_ information.  Every
commit comes with the author's name and email.  So to ensure you get proper
credit and/or blame for all the wonderful code you add, we need to set our name
and email!

### git config
Before you do anything with git you want to be able to identify yourself to
git.  The reason for this is that commits save information that includes who
made the change.  That way if your most excellent code accidentally has a bug
we will all know who to inquire when a fix is needed.

Git comes with a config that is global and project level.  project level config
overrides global level config as its more specific.

* All git config keys are in the following shape: `<section>.<key>`.
* `--global` flag will ensure you set this key value for all future uses of git
* `user.name` and `user.email` are the key's used in creating a commit tied to you
* to add a key value, execute `git config --add --global <key> "<value>"`
* you can view any value of git config by executing `git config --get <key>`

### Problem
Add your `user.name` and `user.email` _IF_ you don't already have values for
them.

### Solution
Lets pretend that when i execute:

```bash
➜  personal git config --get user.name
➜  personal
```

I get nothing out, therefore i need to add both my `user.name` and `user.email`

```bash
git config --add --global user.name "ThePrimeagen"
git config --add --global user.email "the.primeagen@aol.com"
```

### Creating a new rep
The very first step of any project is to create a repo.  To create a repo we
use `git init`.  There are some options to creating repos but they are out of
the scope of this and largely will never be used in your entire career

### Problem
Create a new git repo at `/path/to/new/my-first-git-repo`.  Replace
`/path/to/new` to your preferred project location.

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

### Note
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
Initialized empty Git repository in /home/mpaulson/personal/my-first-git/.git/
```

We will address config more holistically later on.

### We did 2 things here
1. configured git user name and email.
1. we initialized a git repo in a custom location

### Validate we created a git repo
Every git repo comes with a directory that contains all of the state of the git
repo.  This repo is found in `.git`

### Problem
Validate that you have created a new git repo by listing out the files and
directories in the git state directory.

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

This is the empty git repo with all the files and sample code needed to get
your config up and running

### Note
Git keeps its memory by using a series of files and directories.  We will go
into more detail shortly

### The Basics
The next couple commands will be 80% of the commands you will execute when
using git.  80% of git is exceptionally easy.  Its the 20% that can get you
into trouble :7

* `git add <path-to-file | pattern>` will add zero or more files to _the index_
(staging area)
* `git commit -m '<message>'` will commit what changes are present in _the
index_.
* `git status` will describe the state of your git repo which will include
tracked, staged, and untracked changes.

### Problem
We want to trace the steps of git from untracked to tracked.

* create a file `first.md`
* check the status of git to see that its untracked.
* add first.md to the staging area
* check the status of git to see that its now tracked in the staging area
* commit with a friendly message
* check the status of git to see there are no changes pending or untracked

### Solution
#### 1. Create a new file
```bash
vim first.md # or whatever editor you would like to use
```

add a simple change

#### 2. Check the status

```bash
git status
```

You should see something similar to the following.  Noticed that we only have
`untracked` files.  Meaning our git repo has no information on `first.md`
```bash
➜  my-first-git git:(master) ✗ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        first.md

nothing added to commit but untracked files present (use "git add" to track)
```

Lets stage first.md using the `git add` command

```bash
git add first.md
```

With add you can use any glob to add files. Be careful, you may end up adding
files you do not want to.  This will require you to unstage them before
committing.

Lets reexecute `git status` and see the result

```bash
➜  my-first-git git:(master) ✗ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   first.md
```

Commit the file with git commit

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

### What Changes Are In A Repo
A common activity of the 20% is to see what the past changes are within a repo.
We will dive deeper into this operation later, but for now, lets scratch the
surface.

`git log` has many options to make viewing of history a pleasant experience.

Review `man git-log` and search for `--graph` and `--decorate`.  Quickly find
and read the description of each option

### Problem
Display the history of this repo with the `--graph` and `--decorate` option.

### Solution
```bash
git log --graph --decorate

* commit 33d6d9645ac045c6d99eba9d6706d0d48421e582 (HEAD -> master)
  Author: mpaulson <the.primeagen@gmail.com>
  Date:   Sun Jan 21 10:52:45 2024 -0700

      my first commit

```

The decorate option, if you didn't understand from description, means that it
puts branches / HEAD in the commit logs so you can see in a friendly way where
your branches are pointed at.

If you don't know what a branch is, that is ok, we will get there.

Unfortunately we only have one commit, therefore our `--graph` option is less
impressive.  Don't worry we will make extensive use.  What does show up is the
`*` showing the graph has only one node in the output
