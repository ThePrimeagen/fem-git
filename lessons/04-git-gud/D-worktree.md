---
title: "Worktree"
description: "worktrees are amazing and you should know them"
---

## Stash is not that great
Worktrees can prevent one of the most annoying problems of using git.
Stashing.

But wait... did we not just learn stashing?  Yes.  So is it bad?  No.  ... I am
confused

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Worktree
You are working on feature branch `foo_bar`.  You are making great progress and
you are in flow state.

Just then, slack pings, lo and behold an emergency investigation is needed on
`main`.

1. You can `git add .` any non tracked files to the index (staging area) and then
   stash this change then change branches to do the investigation.
1. You can commit this change and change branches
1. Use Worktrees

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### What is a worktree?
Generally when we say `worktree` we mean a `linked working tree`

When you `git init` a repo it creats the `main working tree`.  A `linked
working trees` is a just another tree much like main working tree just without
all the git history within the `.git` directory.  In fact, the `.git` directory
is not a directory at all, but just a file pointing to the `main working tree`
directory.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>


### Worktree Operations
#### Add
```bash
➜  git worktree add <path>
```

The `basename` of the `path` will be used as the branch name.

#### List
```bash
➜  git worktree list
```

#### Delete
There are a couple ways to go about this
1. you can use `git` and
```bash
git worktree remove ../foo-bar
```
2. you can use `rm -rf` and `git worktree prune`

<br>
<br>

#### Benefits
they are cheap to make since they don't need `.git` history.  They are just a
pointer.  They are just slightly more expensive than a branch.  But you get a
commit to work on that is outside your main working tree.  That means if you
need to pivot quickly you don't have to commit, stash, or anything, just create
a new linked working tree!

<br>
<br>

#### Cons
if each branch has a high setup cost.  like if your npm install takes 100
minutes due to everything-js dependency, worktrees can be more of a pain.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
create a worktree of `hello-git` with the path name ending in `foo-bar`.
Suggestion, `../foo-bar`

when completed check out `.git` found in the linked working tree. What's
different about this .git vs a main working tree .git?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  hello-git git:(trunk) git worktree add ../foo-bar
Preparing worktree (new branch 'foo-bar')
HEAD is now at 7488b35 Revert "E"
```

you will notice that when you change into `../foo-bar` that the branch name is
the path's basename

```bash
➜  hello-git git:(trunk) cd ../foo-bar
➜  foo-bar git:(foo-bar)
```

Checking out .git

```bash
➜  foo-bar git:(foo-bar) ls -la
total 24
drwxrwxr-x   2 ThePrimeagen ThePrimeagen 4096 Feb 29 19:00 .
drwxrwxr-x 128 ThePrimeagen ThePrimeagen 4096 Feb 29 19:00 ..
-rw-rw-r--   1 ThePrimeagen ThePrimeagen   59 Feb 29 19:00 bar.md
-rw-rw-r--   1 ThePrimeagen ThePrimeagen   65 Feb 29 19:00 .git
-rw-rw-r--   1 ThePrimeagen ThePrimeagen   41 Feb 29 19:00 README.md
-rw-rw-r--   1 ThePrimeagen ThePrimeagen   24 Feb 29 19:00 upstream.md
```

.git is a file! what!

```bash
➜  foo-bar git:(foo-bar) cat .git
gitdir: /home/ThePrimeagen/personal/hello-git/.git/worktrees/foo-bar
```

.git file shows that it is just a pointer to the main working tree.  That is
how it knows how to find all of the information and why you can switch branches
in a linked working tree.  Your edits are making edits to the main working
tree's .git state

In other words, working trees are "light weight clones"

This means you _NEVER_ have to stash again, if you don't want to.

You can also check out an existing branch with `git worktree add <path> <branch_name>`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Go back to `hello-git` and list out the your linked working tree

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  foo-bar git:(foo-bar) git worktree list
/home/ThePrimeagen/personal/hello-git  7488b35 [trunk]
/home/ThePrimeagen/personal/foo-bar    7488b35 [foo-bar]
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


### Problem
Delete the working tree with `rm -rf` and see if you can discover through
`.git` that your worktree no longer is active.  Then delete the worktree

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  hello-git git:(trunk) rm -rf ../foo-bar
➜  hello-git git:(trunk) git worktree list
/home/ThePrimeagen/personal/hello-git  7488b35 [trunk]
/home/ThePrimeagen/personal/foo-bar    7488b35 [foo-bar] prunable
```

```
[foo-bar] prunable
```

git tells you that `foo-bar` is prunable (its been deleted outside of git!)

That means to delete, just run prune

```bash
➜  hello-git git:(trunk) git worktree prune
➜  hello-git git:(trunk) git worktree list
/home/ThePrimeagen/personal/hello-git  7488b35 [trunk]
```

<br>
<br>

### How did git know it was prunable?
```bash
➜  hello-git git:(trunk) ls -la .git/worktrees
total 12
drwxrwxr-x 3 mpaulson mpaulson 4096 Mar 20 12:27 .
drwxrwxr-x 9 mpaulson mpaulson 4096 Mar 20 12:27 ..
drwxrwxr-x 3 mpaulson mpaulson 4096 Mar 20 12:27 foo-bar
➜  hello-git git:(trunk) ls -la .git/worktrees/foo-bar
total 32
drwxrwxr-x 3 mpaulson mpaulson 4096 Mar 20 12:27 .
drwxrwxr-x 3 mpaulson mpaulson 4096 Mar 20 12:27 ..
-rw-rw-r-- 1 mpaulson mpaulson    6 Mar 20 12:27 commondir
-rw-rw-r-- 1 mpaulson mpaulson   37 Mar 20 12:27 gitdir
-rw-rw-r-- 1 mpaulson mpaulson   24 Mar 20 12:27 HEAD
-rw-rw-r-- 1 mpaulson mpaulson  289 Mar 20 12:27 index
drwxrwxr-x 2 mpaulson mpaulson 4096 Mar 20 12:27 logs
-rw-rw-r-- 1 mpaulson mpaulson   41 Mar 20 12:27 ORIG_HEAD
```

the `gitdir` no longer exists, therefore we can prune this worktree!


<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Recreate a worktree and delete it through .git this time.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  hello-git git:(trunk) git worktree add ../foo-bar
Preparing worktree (checking out 'foo-bar')
HEAD is now at 7488b35 Revert "E"
➜  hello-git git:(trunk) git worktree list
/home/ThePrimeagen/personal/hello-git  7488b35 [trunk]
/home/ThePrimeagen/personal/foo-bar    7488b35 [foo-bar]
➜  hello-git git:(trunk) git worktree remove ../foo-bar
➜  hello-git git:(trunk) git worktree list
/home/ThePrimeagen/personal/hello-git  7488b35 [trunk]
➜  hello-git git:(trunk) ls .. | grep foo-bar
➜  hello-git git:(trunk)
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


### I love worktrees
I think they are one of the best ways to use git as they allow you to keep
state based on each branch rather than on directory.  This allows for partial
changes just to remain partial changes instead of doing the commit, stash, or
rebase squash dance.

I would highly recommend getting use to working with them.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>


### Other Downsides
Its not all upside when it comes to git worktrees.

* rust can easily eat up a couple gigs per branch
* npm installing on each branch can take a minute
* go is great
* .env files can be a bit of a pain

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>


