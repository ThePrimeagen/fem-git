---
title: "HEAD"
description: "Why do we keep seeing this?"
---

## HEAD
Yes.. that weird word... potentially scary that you see everywhere in git

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

Throughout this course you may have notice that whenever we call `git log`
there a `HEAD` within the logs.

What is `HEAD`?

The `HEAD` is current checked out branch's latest commit.

```bash
➜  hello-git git:(foo-rebase-trunk) git checkout trunk
Switched to branch 'trunk'
➜  hello-git git:(trunk) git log --oneline --decorate --graph

* b23e632 (HEAD -> trunk, bar) Y
...         ^--- we currently have trunk checkedout
```

swapping branches and doing another `git log` will result in similar results.

```bash
➜  hello-git git:(trunk) git checkout foo
Switched to branch 'foo'
➜  hello-git git:(trunk) git log --oneline --decorate --graph

* 16984cb (HEAD -> foo) C
...         ^--- we currently have foo checkedout
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

## Reflog
Its not just a wizards tool

Introducing `git reflog`.  The default command of `git reflog` allows you to
see where head has been.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Thought Exercise
Give `reflog` command a try.  What do you see.  Think about what you have had
checked out.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Answers

```bash
➜  hello-git git:(trunk) git reflog

# You should see something similar if you have been following along
16984cb (HEAD -> foo) HEAD@{0}: checkout: moving from trunk to foo
b23e632 (trunk, bar) HEAD@{1}: checkout: moving from foo-rebase-trunk to trunk
3ade655 (foo-rebase-trunk) HEAD@{2}: rebase (finish): returning to refs/heads/foo-rebase-trunk
3ade655 (foo-rebase-trunk) HEAD@{3}: rebase (pick): C
d810248 HEAD@{4}: rebase (pick): B
...
```

<br>
<br>

If you look carefully you will see every checkout you have done of any branch
is listed, in time based ordering

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
This will be an interesting set of tasks to do, but can be quite useful in
understanding reflog.

1. create a new branch off of trunk, call it `baz`.
1. add one commit to `baz`.  Do it in a new file `baz.md`
1. switch back to `trunk` and delete `baz` (`git branch -D baz` from earlier)
1. can you bring back from the dead the commit sha of `baz`?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
First thing we will do is create the baz branch then add a commit to it
```bash
➜  hello-git git:(foo) git checkout trunk
Switched to branch 'trunk'
➜  hello-git git:(trunk) git checkout -b baz
Switched to a new branch 'baz'
➜  hello-git git:(baz) echo "baz" > baz.md
➜  hello-git git:(baz) ✗ git add baz.md
➜  hello-git git:(baz) ✗ git commit -m 'Baz'
[baz f330d23] Baz
 1 file changed, 1 insertion(+)
 create mode 100644 baz.md
```

Next, lets switch back to trunk and delete the branch `baz`

```bash
➜  hello-git git:(baz) git checkout trunk
Switched to branch 'trunk'
➜  hello-git git:(trunk) git branch -D baz
Deleted branch baz (was f330d23).
```

Now we recover the branch via `reflog` (should be obvious because i just talked
about it)

```bash
➜  hello-git git:(baz) git reflog
b23e632 (HEAD -> trunk, bar) HEAD@{0}: checkout: moving from baz to trunk
f330d23 HEAD@{1}: commit: Baz # <--- there is our target commit
b23e632 (HEAD -> trunk, bar) HEAD@{2}: checkout: moving from trunk to baz
...
```

Now that we see `f330d23` is our target commit, what can we do to recover the
work?  There are a lot of possibilities.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Use our knowledge of how git plumbing works to retrieve the contents of a
commit, use this super power to grab out the file `baz.md`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
To get the information we need we will have to use the commit to get the tree
sha and the tree sha for the blob, file `baz.md`, contents

```bash
# Step 1, get the file tree try cat-file'ing the commit sha
➜  hello-git git:(trunk) git cat-file -p f330d23
tree d2d8e10a88b4e985003930d45c5c488abe712e6b # <-- the tree
parent b23e6320e6fba64d93338543dcbcdcc9caadb71e
author mpaulson <mpaulson@netflix.com> 1707069560 -0700
committer mpaulson <mpaulson@netflix.com> 1707069560 -0700

Baz

# Step 2, print out the trees content.  baz.md is our taget file
➜  hello-git git:(trunk) git cat-file -p d2d8e10
100644 blob d3045e2c2d21fcf774700b3d5fa681cf26b300ad    README.md
100644 blob 16858db7afb62f3e027d8f9379085d3567bcac62    bar.md
100644 blob 76018072e09c5d31c8c6e3113b8aa0fe625195ca    baz.md # <-- target file

# Step 3, print the file and copy the contents!
➜  hello-git git:(trunk) git cat-file -p 7601807 > baz.md
```

Pure wizardry

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Lets not use the internals, what is another way, using the commands we have
gone over thus far to get the same information.

### Solution
Remember a branch is just a pointer to a commit

```bash
➜  hello-git git:(trunk) git merge f330d23
Updating b23e632..f330d23
Fast-forward
 baz.md | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 baz.md
```

I can just merge that change directly into trunk!

#### Warning
If histories have diverged far enough, this could cause some problems as you
wouldn't just be merging the one commit, but all the commits in betwixt f330d23
and trunk

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Cherry Pick
from `man git-cherry-pick`

```
       Given one or more existing commits, apply the change each one introduces, recording a new
       commit for each. This requires your working tree to be clean (no modifications from the
       HEAD commit).
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
Git cherry-pick a try and see if you can also get the changes from baz into trunk

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  hello-git git:(trunk) git cherry-pick f330d23
[trunk e6d9d4b] Baz
 Date: Sun Feb 4 10:59:20 2024 -0700
 1 file changed, 1 insertion(+)
 create mode 100644 baz.md
```

I have used cherry pick a _ton_ of times throughout my career as a software dev

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

