---
title: "Merge and Rebase"
description: "The most important topic to any engineer to argue about"
---

## Prereq:
You have to have the previous section finished and have the same state

```
   B --- C   foo
 /
A            trunk
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
Lets start off by creating some more commits and adding them to `trunk`.  This
way our branches diverge.  Meaning they have commits that are unique in both
and have a common ancestor, `A`.


Please add `D` and `E` in the same way we added `B` and `C` to `trunk`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  hello-git git:(foo) git checkout trunk
Switched to branch 'trunk'
➜  hello-git git:(trunk) echo "D" >> README.md
➜  hello-git git:(trunk) git add .
➜  hello-git git:(trunk) git commit -m 'D'
[trunk 79c5076] D
 1 file changed, 1 insertion(+)
➜  hello-git git:(trunk) echo "E" >> README.md
➜  hello-git git:(trunk) git add .
➜  hello-git git:(trunk) git commit -m 'E'
[trunk a665b08] E
 1 file changed, 1 insertion(+)
```

Now we have the following setup

```
   B --- C    foo
 /
A --- D --- E trunk
```

and our logs have commit messages that should make it easy to track.  So now we
can talk about git merge and rebase

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Git Merge and Rebase

### Remember
A commit is a point in time with a specific state of the entire code base.

We have work done, but its on another branch.  We need to get it back into our
main branch, `trunk`... but how?

There really is only one strategy to `merge` code from one branch to another, but
depending on the state different outcomes can happen.  `rebase` can help put a
branch into a pristine state

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### What is a merge?

A merge is attempting to combine two histories together that have diverged at
some point in the past.  There is a common commit point between the two, this
is referred to as the best common ancestor ("merge base" is often the term used
in the docs).

To put it simply, the first in common parent is the best common ancestor.

git then merges the sets of commits onto the merge base and creates a new
commit at the tip of the branch that is being merged on with all the changes
combined into one commit.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### How to merge
Merging is quite simple.

The branch your on is the `target` branch and the branch you name in
`<branchname>` will be the `source` branch.

```bash
git merge <branchname>
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
Given the following state, merge `foo` onto `trunk`.  Since we want to keep
`foo` and `trunk` in its current state for other problems later in this lesson.
Please start by branching _off_ of `trunk`.  I named mine `trunk-merge-foo`

Finally, when you are done use `git log` to see the resulting state of
`trunk-merge-foo`

#### State of your git
```
   B --- C    foo
 /
A --- D --- E trunk-merge-foo
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


### Solution

```bash
➜  hello-git git:(trunk) git checkout -b trunk-merge-foo
Switched to a new branch 'trunk-merge-foo'
```

`git checkout -b trunk-merge-foo` is the same thing as the following:

```bash
git branch trunk-merge-foo
git checkout trunk-merge-foo
```

#### Fun fact
From `man git-switch` you can see the following:
```
       git switch [<options>] (-c|-C) <new-branch> [<start-point>]
```

Meaning we could of created a new branch by using `git switch -c
trunk-merge-foo` and switched to it.

<br>
<br>

lets merge in `foo` into `trunk-merge-foo`

```bash
➜  hello-git git:(trunk-merge-foo) git merge foo
```

At this point you are presented with the `EDITOR`.  Git will use your system's
default editor for you to accept/make any changes to commit messages.  You are
presented with the message that will be in the logs after merging these two
branches together.

I typically do not edit this merge message

```bash
Merge branch 'foo' into trunk-merge-foo
# Please enter a commit message to explain why this merge is necessary,
# especially if it merges an updated upstream into a topic branch.
#
# Lines starting with '#' will be ignored, and an empty message aborts
# the commit.
```

If vim is your default git editor `:wq` to save the message and confirm the
commit

```bash
➜  hello-git git:(trunk-merge-foo) git merge foo
Merge made by the 'ort' strategy.
 second.md | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 second.md
```

Lets use `log` to check out what has happened here

* `--parents` adds 1 to two extra shas signifying the parent chain.  This is
  duplicated by `--graph` but instead of a graphical representation, its with
  shas.

```bash
➜  hello-git git:(trunk-merge-foo) git log --oneline --decorate --graph --parents

*   ccf9a73 a665b08 16984cb (HEAD -> trunk-merge-foo) Merge branch 'foo' into trunk-merge-foo
|\
| * 16984cb 4ad6ccf (foo) C
| * 4ad6ccf cb75afe B
* | a665b08 79c5076 (trunk) E
* | 79c5076 cb75afe D
|/
* cb75afe A
```

How to read the lines

```bash
| * 16984cb 4ad6ccf (foo) C
```

commit `16984cb` has a parent of `4ad6ccf` with a named branch of `foo` with a
commit message of `C`

The only commit that has a different look is the first line of the logs, which
is the latest commit.

```bash
*   ccf9a73 a665b08 16984cb (HEAD -> trunk-merge-foo) Merge branch 'foo' into trunk-merge-foo
```

`ccf9a73` has two parents, `a665b08` and `16984cb`.  If you look at those
commits, they are the commits of `trunk` and `foo`.  `git merge` merged those
two commits together by finding the best common ancestor (merge base `cb75afe`)
and playing the commits one at a time, start at `cb75afe`, creating a new
commit, a merge commit, `ccf9a73`.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Create the following git setup:

```
              X - Y     bar
             /
A --- D --- E           trunk
```

That means:
* branch `bar` off `trunk`
* add two commits with message `X` then `Y`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
First checkout `trunk` then use `trunk` to create branch `bar`

```bash
➜  hello-git git:(trunk-merge-foo) git checkout trunk
➜  hello-git git:(trunk) git checkout -b bar
```

Create the two commits, `X` and `Y`, on `bar`

```bash
➜  hello-git git:(bar) echo "X" > bar.md
➜  hello-git git:(bar) git add bar.md
➜  hello-git git:(bar) git commit -m "X"
[bar 2f43452] X
 1 file changed, 1 insertion(+)
 create mode 100644 bar.md
➜  hello-git git:(bar) echo "Y" >> bar.md
➜  hello-git git:(bar) git add bar.md
➜  hello-git git:(bar) git commit -m "Y"
[bar b23e632] Y
 1 file changed, 1 insertion(+)
```

we can verify that we have similar histories by using `git log` again.

```bash
➜  hello-git git:(bar) git log --oneline --decorate --graph

# You should see the same ordering
* b23e632 (HEAD -> bar) Y
* 2f43452 X
* a665b08 (trunk) E
* 79c5076 D
* cb75afe A
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

## Fast Forward Merges
A fast forward merge (denoted ff-merge) happens when the branch you are merging
has the same history as the current branch.  In other words, the merge base is
the tip of the branch you are merging onto.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
merge `bar` onto `trunk`.  Do not create a separate branch this time.  Once you
merge see if you can spot the difference between a "3 way merge" vs a "fast
forward" merge.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
First we simply checkout `trunk` then execute `git merge bar`

```bash
➜  hello-git git:(bar) git checkout trunk
M       README.md
Switched to branch 'trunk'
➜  hello-git git:(trunk) git merge bar
Updating a665b08..b23e632
Fast-forward
 bar.md | 2 ++
 1 file changed, 2 insertions(+)
 create mode 100644 bar.md
```

Now lets suss out the difference between the two merges

#### First difference
In the merge notes we see the following line:

```bash
Fast-forward
```

That means we have a fast forward merge

#### Second, the logs
You can view this FF merge with `git log`
```bash
➜  hello-git git:(trunk) git log --oneline --decorate --graph

* b23e632 (HEAD -> trunk, bar) Y
* 2f43452 X
* a665b08 E
* 79c5076 D
* cb75afe A
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

## Rebase
Rebasing often gets a bad wrap. Part of this is because people don't really
know why or when to use rebase and will end up using it incorrectly and thus
yelling on the twitters that it "ruins their entire life."  Gotta love twitter.

With the following repo setup

```
   B --- C                foo
 /
A --- D --- E --- X --- Y trunk
```

We can demonstrate the power of rebase.  What rebase will do is update `foo` to
point to `Y` instead of `A`.

```
                           B --- C                foo
                         /
A --- D --- E --- X --- Y                         trunk
```

### THAT IS IT
That is all rebase does here.  It updates the commit where the branch
originally points to

This also means that in we decide to merge `foo` into `trunk` we can do a
ff-merge!


<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### What Rebase Does
The basic steps of rebase is the following:

1.  execute `git rebase <targetbranch>`.  I will refer to the current branch as
    `<currentbranch>` later on
1.  checkout the latest commit on `<targetbranch>`
1.  play one commit at a time from `<currentbranch>`
1.  once finished will update `<currentbranch>` to the current commit sha

It is important to think about what goes on in a rebase.  Why it does what it
does and how there may be problems with it down the road.

We will address that later

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
rebase `foo` with `trunk`.  Please create a separate branch `foo-rebase-trunk`.
This branch should be created off of `foo`.  Once you have rebased
`foo-rebase-trunk` with `trunk` check out the git logs with `--graph` and
`--decorate` to see what has happened

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
First we will create a new branch off of `foo` and call it `foo-rebase-trunk`

```bash
➜  hello-git git:(trunk) git checkout foo
Switched to branch 'foo'
➜  hello-git git:(foo) git checkout -b foo-rebase-trunk
Switched to a new branch 'foo-rebase-trunk'
```

Now we need to perform the rebase

```bash
➜  hello-git git:(foo-rebase-trunk) git rebase trunk
Successfully rebased and updated refs/heads/foo-rebase-trunk.
```

`git log` time to view what has happenend

```bash
➜  hello-git git:(foo-rebase-trunk) git log --oneline --decorate --graph

# you should have same history
* 3ade655 (HEAD -> foo-rebase-trunk) C
* d810248 B
* b23e632 (trunk, bar) Y
* 2f43452 X
* a665b08 E
* 79c5076 D
* cb75afe A
```

You will see that `trunk` (which contains `bar` after our fast forward merge)
is now the new base for `foo-rebase-trunk`.  In other words, `foo` is no longer
diverging from `trunk`.  If we choose to merge `foo` into `trunk` we can do so
via ff-merge (no merge commit).

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Some pros
When using rebase you can have a clean history with no merge commits.  If you
are someone who uses `git log` a lot this can really help with searching.

### Some cons
It alters history of a branch.  That means that if you already had `foo` on a
remote git, you would have to `force` push it to the remote again.  We will go
over this more it shortly

### Some cautionary words
NEVER CHANGE HISTORY OF A PUBLIC BRANCH.  In other words, don't ever change
history of `trunk`.  But your own personal branch?  I don't think it matters
and i think having a nice clean history can be very beneficial if you use git
to search for changes through time.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

