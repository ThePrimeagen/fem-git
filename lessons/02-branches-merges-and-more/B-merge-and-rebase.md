---
title: "Merge and Rebase"
description: "The most important topic to any engineer to argue about"
---

## Prereq:
You have to have the previous section finished and have the same state

```bash
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

<br>
<br>

Please add `D` and `E` in the same way we added `B` and `C` to `trunk`

<br>
<br>

Desired State:

```bash
   B --- C    foo
 /
A --- D --- E trunk
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

```bash
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

<br>
<br>

We have work done, but its on another branch.  We need to get it back into our
main branch, `trunk`... but how?

<br>
<br>

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

<br>
<br>

To put it simply, the first in common parent is the best common ancestor.

<br>
<br>

git then merges the sets of commits onto the merge base and creates a new
commit at the tip of the branch that is being merged on with all the changes
combined into one commit.

<br>
<br>

there is also extra information in the logs about these types of commits

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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

<br>
<br>

The branch your on is the `target` branch and the branch you name in
`<branchname>` will be the `source` branch.

<br>
<br>

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

<br>
<br>

Finally, when you are done use `git log` to see the resulting state of
`trunk-merge-foo`

<br>
<br>

#### State of your git

```bash
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
<br>
<br>

`git checkout -b trunk-merge-foo` is the same thing as the following:

<br>
<br>

```bash
git branch trunk-merge-foo
git checkout trunk-merge-foo
```

<br>
<br>

#### Fun fact
From `man git-switch` you can see the following:

```bash
       git switch [<options>] (-c|-C) <new-branch> [<start-point>]
```

<br>
<br>

Meaning we could of created a new branch by using `git switch -c
trunk-merge-foo` and switched to it.

<br>
<br>

lets merge in `foo` into `trunk-merge-foo`

```bash
➜  hello-git git:(trunk-merge-foo) git merge foo
```

<br>
<br>

At this point you are presented with the `EDITOR`.  Git will use your system's
default editor for you to accept/make any changes to commit messages.  You are
presented with the message that will be in the logs after merging these two
branches together.

<br>
<br>

I typically do not edit this merge message

<br>
<br>

```bash
Merge branch 'foo' into trunk-merge-foo
# Please enter a commit message to explain why this merge is necessary,
# especially if it merges an updated upstream into a topic branch.
#
# Lines starting with '#' will be ignored, and an empty message aborts
# the commit.
```

<br>
<br>

If vim is your default git editor `:wq` to save the message and confirm the
commit

<br>
<br>

```bash
➜  hello-git git:(trunk-merge-foo) git merge foo
Merge made by the 'ort' strategy.
 second.md | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 second.md
```

<br>
<br>

Lets use `log` to check out what has happened here

<br>
<br>

* `--parents` adds 1 to two extra shas signifying the parent chain.  This is
  duplicated by `--graph` but instead of a graphical representation, its with
  shas.

<br>
<br>

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

<br>
<br>

How to read the lines

<br>
<br>

```bash
| * 16984cb 4ad6ccf (foo) C
```

<br>
<br>

commit `16984cb` has a parent of `4ad6ccf` with a named branch of `foo` with a
commit message of `C`

<br>
<br>

The only commit that has a different look is the first line of the logs, which
is the latest commit.

<br>
<br>

```bash
*   ccf9a73 a665b08 16984cb (HEAD -> trunk-merge-foo) Merge branch 'foo' into trunk-merge-foo
```

<br>
<br>

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

```bash
              X - Y     bar
             /
A --- D --- E           trunk
```

That means:
* branch `bar` off `trunk`
* add two commits with message `X` then `Y`
* create your changes, `X` and `Y`, in `bar.md`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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

<br>
<br>

```bash
➜  hello-git git:(trunk-merge-foo) git checkout trunk
➜  hello-git git:(trunk) git checkout -b bar
```

<br>
<br>

Create the two commits, `X` and `Y`, on `bar`

<br>
<br>

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

<br>
<br>

we can verify that we have similar histories by using `git log` again.

<br>
<br>

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

<br>
<br>

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

<br>
<br>

#### First difference
In the merge notes we see the following line:

<br>
<br>

```bash
Fast-forward
```

<br>
<br>

That means we have a fast forward merge

<br>
<br>

#### Second, the logs
You can view this FF merge with `git log`

<br>
<br>

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

Excalidraw what happens

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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

<br>
<br>

With the following repo setup

<br>
<br>

```bash
   B --- C                foo
 /
A --- D --- E --- X --- Y trunk
```

<br>
<br>

We can demonstrate the power of rebase.  What rebase will do is update `foo` to
point to `Y` instead of `A`.

<br>
<br>

```bash
                           B --- C                foo
                         /
A --- D --- E --- X --- Y                         trunk
```

<br>
<br>

### THAT IS IT
That is all rebase does here.  It updates the commit where the branch
originally points to

<br>
<br>

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

<br>
<br>

How rebase works is important, it will lead to complication down the road if
you don't understand this key fact

<br>
<br>

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
`--decorate` to see what has happened (`--oneline` can make it easier to read)

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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

<br>
<br>

```bash
➜  hello-git git:(trunk) git checkout foo
Switched to branch 'foo'
➜  hello-git git:(foo) git checkout -b foo-rebase-trunk
Switched to a new branch 'foo-rebase-trunk'
```

<br>
<br>

Now we need to perform the rebase

<br>
<br>

```bash
➜  hello-git git:(foo-rebase-trunk) git rebase trunk
Successfully rebased and updated refs/heads/foo-rebase-trunk.
```

<br>
<br>

`git log` time to view what has happenend

<br>
<br>

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

<br>
<br>

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

<br>
<br>

### Some cons
It alters history of a branch.  That means that if you already had `foo` on a
remote git, you would have to `force` push it to the remote again.  We will go
over this more it shortly

<br>
<br>

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

### Wait.. isn't there more?
Yes, there is a lot more, but we are not going to go over it without more
foundational knowledge

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

