---
title: "Rebasing is Complicated"
description: "Lets change some history like its 1984"
---

## MOAR Rebasing
We have briefly talked about rebasing as being able to realign where the branch
point exists for one branch onto another.

<br>
<br>

In other words, you make history linear.  Here is a simple visual example

<br>
<br>

```bash
      E - F - G    topic
    /
A - B - C - D      master
```

<br>
<br>

```bash
➜  some-git git:(topic) git rebase master # we are on branch topic
```

<br>
<br>

Assuming everything went off without a hitch, you will have the following state

<br>
<br>

```bash
              E - F - G    topic
             /
A - B - C - D              master
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

## Interactive Rebasing and Squashing
You may find yourself on a team that asks you to "squash" your commits.  What
is meant by this is interactive rebase squash.

<br>
<br>

In other words: the aforementioned diagram we can transform from

<br>
<br>

```bash
              E - F - G    topic
             /
A - B - C - D              master
```

<br>
<br>

To
```bash
              # notice this is one commit
              EFG          topic
             /
A - B - C - D              master
```

<br>
<br>

Along with squashing, interactive rebasing allows you to edit messages and more

<br>
<br>

Lets create a situation where we can interactively squash our commits and
provide some proper messaging!

<br>
<br>

But to get there we need to cover a LOT of ground

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### For us to cover this...
we have to talk about the most dreaded topic in git

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>


## Conflicts
I hate them <br>
You hate them <br>
but its good to know how to resolve them. <br>

<br>
<br>

#### PLEASE
A note for all who have a nice git plugin to make this process easier.  Please
do not use any fancy tools, lets just manually resolve these.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### To create a conflict
The easiest way to create a conflict is when you have two changes to a repo
that cannot be resolved by the merging strategies.  In other words, edit the
same line.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Create a conflict with `remote-git` and `hello-git`.  To do this, please create
a commit in both `hello-git` and `remote-git` editing the same location within
a file.

<br>
<br>

To accomplish this
1. Use `trunk` in both repos
1. change `hello-git`'s README.md first line to `A + 1` and commit
1. change `remote-git`'s README.md first line to `A + 2` and commit
1. pull `hello-git` into `remote-git` to create the conflict

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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

1. Changed `hello-git` `trunk`'s README.md's first line

```bash
cd path/to/hello-git
➜  hello-git git:(trunk) vim README.md

A + 1
D
E
remote-change

```

<br>
<br>

Commit the change

<br>
<br>

```bash
➜  hello-git git:(trunk) git add .
➜  hello-git git:(trunk) git commit -m 'A + 1'
[trunk 9648be0] A + 1
 1 file changed, 1 insertion(+), 1 deletion(-)
```

<br>
<br>

2. Changed `remote-git` `trunk`'s README.md's first line

<br>
<br>

```bash
cd path/to/remote-git
➜  remote-git git:(trunk) vim README.md

A + 2
D
E
remote-change

```

<br>
<br>

Commit the change

<br>
<br>

```bash
➜  remote-git git:(trunk) git add .
➜  remote-git git:(trunk) git commit -m 'A + 2'
[trunk 6eb0a42] A + 2
 1 file changed, 1 insertion(+), 1 deletion(-)
```

<br>
<br>

Time to pull down change from remote

<br>
<br>

```bash
➜  remote-git git:(trunk) git pull origin trunk
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 303 bytes | 303.00 KiB/s, done.
From ../hello-git
 * branch            trunk      -> FETCH_HEAD
 + 8058c68...9648be0 trunk      -> origin/trunk  (forced update)
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

<br>
<br>

We are now officially conflicted!

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### What is in conflict?
Often its not obvious what is in conflict just by the message (if there is a
large set of changes).  So a simple way to see what is conflicted is by
checking out the status

<br>
<br>

```bash
➜  remote-git git:(trunk) git status
On branch trunk
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

<br>
<br>

#### Note
There are some things to take from this status message

<br>

1. Unmerged path's contains README.md and it says `both modified`.  That is
   your key to what needs to be resolved

<br>

2. You can abort the merge due to the conflict by executing `git merge --abort`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Resolving a Conflict
When you cat out the file that is conflicted, `README.md`, you will see some
additional information in the file that was not there before

<br>
<br>

```bash
➜  remote-git git:(trunk) vim README.md

## This is the look of vim
  1   <<<<<<< HEAD
    1 A + 2
    2 =======
    3 A + 1
    4 >>>>>>> 9648be0ae764528ac63759d7e49fc623ae0af373
    5 D
    6 E
    7 remote-change
    8 downstream change

```

<br>
<br>

So some important information is present.

<br>
<br>

1.  Any >>>>, ======, <<<<< denote parts of the conflict.

```bash
  1   <<<<<<< HEAD
```

<br>
<br>

This stats that `HEAD`s conflicted change starts here and continues until the
`=======` line.  You can confirm this with `git log -p -1`

<br>
<br>

You can verify this by noticing that the change in the `HEAD` section is `A +
2` which is the change that is in the `remote-git` `trunk` branch and is the
`HEAD` location of `remote-git`

<br>
<br>

`=======` denotes the separation of the two merges

<br>
<br>

The end of the merge conflict is denoted with >>>> and sha of the incoming
conflicted change

```bash
>>>>>>> 9648be0ae764528ac63759d7e49fc623ae0af373
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
Validate that the sha, mine is 9648be0ae764528ac63759d7e49fc623ae0af373,
belongs to `hello-git`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
You can validate the bottom sha belonging to `hello-git` by using the following
log

<br>
<br>

```bash
➜  hello-git git:(trunk) git log --oneline -1
9648be0 (HEAD -> trunk) A + 1
```

<br>
<br>

`-1` with `git log` says only show `1` commit.  `-3` would show `3` commits of
history.

<br>
<br>

Notice that the hash provided in the conflict is `HEAD` in `hello-git` and it
also matches the change of `A + 1`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
We are conflicted and we need to resolve this.  Use the `status` message to
identify which file to edit and what to do after you edit the file.

<br>
<br>

Lets choose a side to keep as part of the merge.  We _will_ choose my
`remote-git` change.  To choose that commit, delete line `<<<<<<< HEAD` and
delete from `=======` up to and including `>>>>>>>
9648be0ae764528ac63759d7e49fc623ae0af373`

<br>
<br>

In other words we are keeping the HEAD changes and dropping the `9648be0`
changes

<br>
<br>

(for the sake of the course you should choose the same side)

<br>
<br>

After conflict has been resolved (by removing the conflict markers and the code
from `hello-git`) commit the merge.

<br>
<br>

**Before you commit the merge check the status**

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
The desired code state should be:

<br>
<br>

```bash
A + 2
D
E
remote-change
downstream change
```

<br>
<br>

We removed all the `<`, `=`, and `>` lines (conflict markers) and `A + 1`
(change from `hello-git`)

<br>
<br>

#### Side Note
Now there is technically nothing preventing you from choosing both sides, and if you did
that your code would look like

<br>
<br>

```bash
A + 1
A + 2
D
E
remote-change
downstream change
```

<br>
<br>

`git status` tells us the next step

<br>
<br>

```bash
➜  remote-git git:(trunk) git status
On branch trunk
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

<br>
<br>

We need to run `git commit`

<br>
<br>

```bash
➜  remote-git git:(trunk) git add .
➜  remote-git git:(trunk) git status
On branch trunk
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)
```

<br>
<br>

```bash

➜  remote-git git:(trunk) git commit
[trunk d8a2f95] Merge branch 'trunk' of ../hello-git into trunk


# You will be presented with this commit

Merge branch 'trunk' of ../hello-git into trunk

# Conflicts:
#	README.md
#
# It looks like you may be committing a merge.
# If this is not correct, please run
#	git update-ref -d MERGE_HEAD
# and try again.


# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch trunk
# All conflicts fixed but you are still merging.
#
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

### Thought Exercise
There was no status... why?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
We didn't accept the changes from `hello-git`.  We effectively _deleted_ all
the changes from remote so the change was empty.  There is still a merge commit
that is needed.

<br>
<br>

You can validate the merge commit by a quick look at the logs

<br>
<br>

```bash
d8a2f95 (HEAD -> trunk) Merge branch 'trunk' of ../hello-git into trunk
9648be0 (origin/trunk) A + 1
6eb0a42 A + 2
7282922 greatest changes
6849c67 upstream changes
42afc8d A remote change
b23e632 Y
2f43452 X
a665b08 E
79c5076 D
cb75afe A
```

<br>
<br>

Notice that we have a `merge` commit and we also have `A + 1` commit.  The
history is not lost, but the changes are not present

<br>
<br>

**Try looking at the log with --graph**

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Two conflicts are better than one, right?  .... right?

<br>
<br>

Ok, i agree.  Lets not conflict again.  Instead

<br>
<br>

1. create a change in `bar.md` in `hello-git`

<br>
<br>

```bash
➜  hello-git git:(trunk) echo "no conflict" >> bar.md
```

<br>
<br>

2. pull in change in remote

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Make the change

<br>
<br>

```bash
➜  hello-git git:(trunk) echo "no conflict" >> bar.md
➜  hello-git git:(trunk) git add .
➜  hello-git git:(trunk) git commit -m 'no conflict'
[trunk a9cb358] no conflict
 1 file changed, 1 insertion(+)
```

<br>
<br>

Pull in the change to `remote-git`

<br>
<br>

```bash
➜  remote-git git:(trunk) git pull origin trunk
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 297 bytes | 27.00 KiB/s, done.
From ../hello-git
 * branch            trunk      -> FETCH_HEAD
 + 9ee733d...a9cb358 trunk      -> origin/trunk  (forced update)
Merge made by the 'ort' strategy.
 bar.md | 1 +
 1 file changed, 1 insertion(+)

  1   Merge branch 'trunk' of ../hello-git into trunk
    1 # Please enter a commit message to explain why this merge is necessary,
    2 # especially if it merges an updated upstream into a topic branch.
    3 #
    4 # Lines starting with '#' will be ignored, and an empty message aborts
    5 # the commit.
```

<br>
<br>

#### Observation
Notice you do have to have another merge commit because the origin does not
contain your commits, so every merge will cause a merge commit.

<br>
<br>

That means you could get a pretty hairy set of commits.

<br>
<br>

**Check out the graph again with log --graph**

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Takeaway?
* once you resolve a conflict and you don't take upstream's you will get merge
commits until you sync your changes back to the remote

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Conflicts, but with rebase
aren't you excited for more conflicts?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
What is unique about `rebase` that would make conflicts harder?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Recall that rebase will replay all your commits after moving forward the
history, which means what if a conflict happened in the past?

<br>
<br>

Lets say you have the following setup:

<br>
<br>

```bash
      E - F - G    topic
    /
A - B - C - D      master
```

<br>
<br>

And lets pretend that `C` contains a change that creates a conflict with `G`.
We rebase and we resolve the conflict and now our graph looks like the following:

<br>
<br>

```bash
              E - F - G    topic
             /
A - B - C - D              master
```

<br>
<br>

Then master gets another commit, `Y`

<br>
<br>

```bash
              E - F - G    topic
             /
A - B - C - D - Y          master
```

<br>
<br>

Now if we rebase again, we will play `E`, `F`, and `G`.

<br>
<br>

Since we are computer scientists, aka masochists, Lets do this to ourselves!

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
To ensure everything continues on going smooth, lets update `trunk` in
`hello-git` with `push` from `hello-git`

<br>
<br>

We don't want more merge commits...

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  remote-git git:(trunk) git push origin trunk
Enumerating objects: 15, done.
Counting objects: 100% (14/14), done.
Delta compression using up to 12 threads
Compressing objects: 100% (7/7), done.
Writing objects: 100% (9/9), 1.07 KiB | 1.07 MiB/s, done.
Total 9 (delta 1), reused 0 (delta 0), pack-reused 0
remote: error: refusing to update checked out branch: refs/heads/trunk
remote: error: By default, updating the current branch in a non-bare repository
remote: is denied, because it will make the index and work tree inconsistent
remote: with what you pushed, and will require 'git reset --hard' to match
remote: the work tree to HEAD.
remote:
remote: You can set the 'receive.denyCurrentBranch' configuration variable
remote: to 'ignore' or 'warn' in the remote repository to allow pushing into
remote: its current branch; however, this is not recommended unless you
remote: arranged to update its work tree to match what you pushed in some
remote: other way.
remote:
remote: To squelch this message and still keep the default behaviour, set
remote: 'receive.denyCurrentBranch' configuration variable to 'refuse'.
To ../hello-git
 ! [remote rejected] trunk -> trunk (branch is currently checked out)
error: failed to push some refs to '../hello-git'
```
<br>
<br>

### What happened here?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>


### Observation

```bash
...
 ! [remote rejected] trunk -> trunk (branch is currently checked out)
```

<br>
<br>

We cannot push to a branch that is the current branch of the target repo.  This
makes sense as it would cause your current branch to change out of underneath
the repo that is currently being used, and if there are pending changes it
could cause further havoc.

<br>
<br>

### What do we do?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Change branches!

```bash
➜  hello-git git:(trunk) git checkout bar
Switched to branch 'bar'
```

<br>
<br>

Now lets try again

<br>
<br>

```bash
➜  remote-git git:(trunk) git push origin trunk
Enumerating objects: 15, done.
Counting objects: 100% (14/14), done.
Delta compression using up to 12 threads
Compressing objects: 100% (7/7), done.
Writing objects: 100% (9/9), 1.07 KiB | 1.07 MiB/s, done.
Total 9 (delta 1), reused 0 (delta 0), pack-reused 0
To ../hello-git
   a9cb358..b51e34a  trunk -> trunk
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
Lets create another conflict but resolve this via `rebase` instead of `merge`.

<br>
<br>

1. create change in `hello-git` and `A + 2` -> `A + 3`.
1. **Create another change in `bar.md` LAST LINE** in `hello-git`
1. create change in `remote-git` and `A + 2` -> `A + 4`
1. **Create another change in `bar.md` FIRST LINE** in `remote-git`
1. rebase `remote-git`'s `trunk` with `hello-git`'s and create the conflict

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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

`happy-git`

```bash
➜  hello-git git:(bar) git checkout trunk
Switched to branch 'trunk'
➜  hello-git git:(trunk) vim README.md
➜  hello-git git:(trunk) vim bar.md
➜  hello-git git:(trunk) git diff
diff --git a/README.md b/README.md
index e42f7f7..43b4231 100644
--- a/README.md
+++ b/README.md
@@ -1,4 +1,4 @@
-A + 2
+A + 3
 D
 E
 remote-change
diff --git a/bar.md b/bar.md
index 04fca9f..894d904 100644
--- a/bar.md
+++ b/bar.md
@@ -1,3 +1,4 @@
 X
 Y
 no conflict
+adding a line to the end
➜  hello-git git:(trunk) git add .
➜  hello-git git:(trunk) git commit -m 'A + 3'
[trunk 958f33f] A + 3
 2 files changed, 2 insertions(+), 1 deletion(-)
```

`remote-git`

```bash
➜  remote-git git:(trunk) vim README.md
➜  remote-git git:(trunk) vim bar.md
➜  remote-git git:(trunk) git diff
diff --git a/README.md b/README.md
index e42f7f7..76c0a5e 100644
--- a/README.md
+++ b/README.md
@@ -1,4 +1,4 @@
-A + 2
+A + 4
 D
 E
 remote-change
diff --git a/bar.md b/bar.md
index 04fca9f..3dc9259 100644
--- a/bar.md
+++ b/bar.md
@@ -1,3 +1,4 @@
+first line change
 X
 Y
 no conflict
➜  remote-git git:(trunk) git add .
➜  remote-git git:(trunk) git commit -m 'A + 4'
[trunk a1cbe6c] A + 4
 2 files changed, 2 insertions(+), 1 deletion(-)
```

Now that we are all primed and ready for a conflict.

```bash
➜  remote-git git:(trunk) git pull origin trunk --rebase
remote: Enumerating objects: 7, done.
remote: Counting objects: 100% (7/7), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (4/4), 367 bytes | 367.00 KiB/s, done.
From ../hello-git
 * branch            trunk      -> FETCH_HEAD
 + 75e4992...958f33f trunk      -> origin/trunk  (forced update)
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Auto-merging bar.md
error: could not apply a1cbe6c... A + 4
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply a1cbe6c... A + 4
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

#### NOTICE
If you read carefully you will see that bar was able to be `Auto-merged` where
as README was not able to be merged

<br>
<br>

#### NOTICE
We can `git rebase --abort` due to the conflict (much like the `git merge
--abort`).

<br>
<br>

#### NOTICE
This is important.  Once you have resolved the conflict we need to `git rebase
--continue` instead of `git commit`.

<br>
<br>

#### GIT FU
* If you did use `git reset --soft HEAD~1` and then `git rebase --continue`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Checking out the conflict

```bash
➜  remote-git git:(958f33f) git status
interactive rebase in progress; onto 958f33f
Last command done (1 command done):
   pick a1cbe6c A + 4
No commands remaining.
You are currently rebasing branch 'trunk' on '958f33f'.
  (fix conflicts and then run "git rebase --continue")
  (use "git rebase --skip" to skip this patch)
  (use "git rebase --abort" to check out the original branch)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   bar.md

Unmerged paths:
  (use "git restore --staged <file>..." to unstage)
  (use "git add <file>..." to mark resolution)
        both modified:   README.md
```

<br>
<br>

You will notice that `bar.md` is marked (green if you have coloring) committed
while `README.md` is unmerged.  Lets fix our conflict in README.md

<br>
<br>

Opening up README.md shows us the following

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Explain the conflict and why its different than merge

```bash
A + 3
D
E
remote-change
downstream change
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

### Answer
This was pretty tricky question. noticed that A + 3, from `hello-git` now takes
the top spot and `A + 4` takes the bottom.

<br>
<br>

What are the steps of rebase?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Choose `our` conflict, `hello-git`'s change. (A + 3)

<br>
<br>

#### Remember
do not commit. We `git rebase --continue`.  This signals to the rebase command
that we are ready for the next commit to be played on top.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  remote-git git:(958f33f) git add .
➜  remote-git git:(958f33f) git status
interactive rebase in progress; onto 958f33f
Last command done (1 command done):
   pick a1cbe6c A + 4
No commands remaining.
You are currently rebasing branch 'trunk' on '958f33f'.
  (all conflicts fixed: run "git rebase --continue")

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   bar.md

➜  remote-git git:(958f33f) git rebase --continue
[detached HEAD c7b9731] A + 4
 1 file changed, 1 insertion(+)
Successfully rebased and updated refs/heads/trunk.

A + 4

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# interactive rebase in progress; onto 958f33f
# Last command done (1 command done):
#    pick a1cbe6c A + 4
# No commands remaining.
# You are currently rebasing branch 'trunk' on '958f33f'.
#
# Changes to be committed:
#	modified:   bar.md
#
```

<br>
<br>

Since there are no more commits that cause conflicts the rebase is complete.
Lets take a quick look at our logs

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Exercise
check out the history

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Result

```bash
➜  remote-git git:(trunk) git log --oneline -5
c7b9731 (HEAD -> trunk) A + 4
958f33f (origin/trunk) A + 3
b51e34a Merge branch 'trunk' of ../hello-git into trunk
a9cb358 no conflict
d8a2f95 Merge branch 'trunk' of ../hello-git into trunk
```

You will see our A + 4 and you will see origin's A + 3 "underneath" or previous
in history.

<br>
<br>

#### Note
There is no merge commit.  People really seem to like this cleaner history.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## The Problem of Rebase
Don't forget, rebase replays the commits on top of the history change.

Here is the linked `git status` call from above
```bash
➜  remote-git git:(958f33f) git status
interactive rebase in progress; onto 958f33f
Last command done (1 command done):
   pick a1cbe6c A + 4
No commands remaining.
You are currently rebasing branch 'trunk' on '958f33f'.
  (all conflicts fixed: run "git rebase --continue")

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   bar.md
```

### Thought Experiment
Ask yourself, where is README.md?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Answer
Well, since we accepted _theirs_ (marked as `ours` in the resolution) (we will
talk about ours vs theirs later) changeset, we technically removed any change
from `README.md`.  Therefore there was no change to rebase continue on.

Remember, we have origin/trunk effectively checked out.  therefore there is no
change to README.md when we accept _our_ changes during a rebase

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Lets try create the same issue except this time lets accept `ours` (`theirs` by
position (bottom)) change.

1. `A + 5` in `hello-git`
1. `A + 6` in `remote-git`
1. `git pull origin trunk --rebase` in `remote-git` to cause the conflict
1. accept `A + 6` change and `git rebase --continue`
1. check out history to see `A + 6` commit

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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

`remote-git`
```bash
➜  remote-git git:(trunk) git diff
diff --git a/README.md b/README.md
index 43b4231..0c72736 100644
--- a/README.md
+++ b/README.md
@@ -1,4 +1,4 @@
-A + 3
+A + 5
 D
 E
 remote-change
➜  remote-git git:(trunk) git add .
➜  remote-git git:(trunk) git commit -m 'A + 6'
[trunk 6740bc7] A + 5
 1 file changed, 1 insertion(+), 1 deletion(-)
```

`hello-git`
```bash
➜  hello-git git:(trunk) vim README.md
➜  hello-git git:(trunk) git add .
➜  hello-git git:(trunk) git commit -m 'A + 5'
[trunk fac2b82] A + 6
 1 file changed, 1 insertion(+), 1 deletion(-)
```

`rebase`
```bash
➜  remote-git git:(trunk) git pull origin trunk --rebase
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 315 bytes | 315.00 KiB/s, done.
From ../hello-git
 * branch            trunk      -> FETCH_HEAD
 + 094cca8...fac2b82 trunk      -> origin/trunk  (forced update)
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
error: could not apply 6740bc7... A + 5
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply 6740bc7... A + 5
```

```bash
➜  remote-git git:(6a3fb28) vim README.md
➜  remote-git git:(6a3fb28) git status
interactive rebase in progress; onto fac2b82
Last commands done (2 commands done):
   pick c7b9731 A + 4
   pick 6740bc7 A + 5
No commands remaining.
You are currently rebasing branch 'trunk' on 'fac2b82'.
  (fix conflicts and then run "git rebase --continue")
  (use "git rebase --skip" to skip this patch)
  (use "git rebase --abort" to check out the original branch)

Unmerged paths:
  (use "git restore --staged <file>..." to unstage)
  (use "git add <file>..." to mark resolution)
        both modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
➜  remote-git git:(6a3fb28) git add .
➜  remote-git git:(6a3fb28) git rebase --continue

A + 5

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# interactive rebase in progress; onto fac2b82
# Last commands done (2 commands done):
#    pick c7b9731 A + 4
#    pick 6740bc7 A + 5
# No commands remaining.
# You are currently rebasing branch 'trunk' on 'fac2b82'.
#
# Changes to be committed:
#	modified:   README.md
#

[detached HEAD 52bfa5a] A + 5
 1 file changed, 1 insertion(+), 1 deletion(-)
Successfully rebased and updated refs/heads/trunk.
```

check history

```bash
➜  remote-git git:(6a3fb28) git log --oneline -5
52bfa5a (HEAD -> trunk) A + 5
6a3fb28 A + 4
fac2b82 (origin/trunk) A + 6
958f33f A + 3
b51e34a Merge branch 'trunk' of ../hello-git into trunk
```

#### This is where rebase can suck
This is where things get complicated.  We have kept our change in the rebase.
(our being remote-git's trunk)

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Create a change in `hello-git` and pull again.
1. Add a `NewLine` below `A + 6` in `hello-git`
1. rebase pull `hello-git` and cause the conflict
1. DO NOT RESOLVE THE CONFLICT

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  hello-git git:(trunk) vim README.md
➜  hello-git git:(trunk) git diff
diff --git a/README.md b/README.md
index c04f8e6..18b7811 100644
--- a/README.md
+++ b/README.md
@@ -1,4 +1,5 @@
 A + 6
+NewLine
 D
 E
 remote-change
➜  hello-git git:(trunk) git add .
➜  hello-git git:(trunk) git commit -m 'small new line'
[trunk 99df23c] small new line
 1 file changed, 1 insertion(+)
```

Now lets pull from `remote-git` with rebase

```bash
➜  remote-git git:(trunk) git pull origin trunk --rebase
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 328 bytes | 328.00 KiB/s, done.
From ../hello-git
 * branch            trunk      -> FETCH_HEAD
   fac2b82..99df23c  trunk      -> origin/trunk
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
error: could not apply 52bfa5a... A + 5
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply 52bfa5a... A + 5
```

Weird... lets check the conflict

```bash
➜  remote-git git:(abcc4f4) git status
interactive rebase in progress; onto 99df23c
Last commands done (2 commands done):
   pick 6a3fb28 A + 4
   pick 52bfa5a A + 5
No commands remaining.
You are currently rebasing branch 'trunk' on '99df23c'.
  (fix conflicts and then run "git rebase --continue")
  (use "git rebase --skip" to skip this patch)
  (use "git rebase --abort" to check out the original branch)

Unmerged paths:
  (use "git restore --staged <file>..." to unstage)
  (use "git add <file>..." to mark resolution)
        both modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Ok, only README.md is conflicted.

```bash
A + 6
NewLine
D
E
remote-change
downstream change
```

Wait... didn't we already resolve this conflict?  Why are we resolving it again?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Does this now make sense with how rebase works?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Answer
Rebase works by replaying the commits one at a time.  Therefore if we have our
change from a conflict and then we replay the changes we will reconflict on the
same change again and again.

Does that mean rebase sucks?  Well no, it keeps your history very clean, but
does that mean rebase can be annoying?  Yes.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Question
(i know there is an active conflict right now due to rebase)

<br>
<br>

Given what you know now, would you use rebase or merge?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Answer
I would... and there is likely something you don't know about

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## RERERE
rerere is just one of the strangest options in all of git.  There are some
basic commands that can be ran.  Check out the man page, `man git-rerere` to go
into details.   I have never needed them before, i just use the config and live
my best life

From the [The Git Docs](https://git-scm.com/book/en/v2/Git-Tools-Rerere)

> The git rerere functionality is a bit of a hidden feature. The name stands for “reuse recorded resolution” and, as the name implies, it allows you to ask Git to remember how you’ve resolved a hunk conflict so that the next time it sees the same conflict, Git can resolve it for you automatically.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Enable rerere just for this project.  You can enable it globally if you like
it. Make the config option `rerer.enabled` to `true`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
git config rerere.enabled true
```

<br>
<br>

and to validate

<br>
<br>

```bash
git config --list --local
... a few options ...
rerere.enabled=true
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

### What is rerere?
rerere stands for REuse REcorded REsolution.  Or in other words, git will
automagically remember how you handled a specific conflict and will just replay
your decision the next time you run into it.

It is not all sunshine and rainbows.  You can, refer to `man git-rerere`,
delete rerere's in case you incorrectly resolved a conflict

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Resolve the conflict and accept _our_ change, `A + 6`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  remote-git git:(abcc4f4) vim README.md
➜  remote-git git:(abcc4f4) git status
interactive rebase in progress; onto 99df23c
Last commands done (2 commands done):
   pick 6a3fb28 A + 4
   pick 52bfa5a A + 5
No commands remaining.
You are currently rebasing branch 'trunk' on '99df23c'.
  (fix conflicts and then run "git rebase --continue")
  (use "git rebase --skip" to skip this patch)
  (use "git rebase --abort" to check out the original branch)

Unmerged paths:
  (use "git restore --staged <file>..." to unstage)
  (use "git add <file>..." to mark resolution)
        both modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
➜  remote-git git:(abcc4f4) git add .
➜  remote-git git:(abcc4f4) git rebase --continue
[detached HEAD 226add3] A + 5
 1 file changed, 1 insertion(+), 2 deletions(-)
Successfully rebased and updated refs/heads/trunk.
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
To test out our rerere, lets create one more change to `hello-git` and see if
we can auto play the conflict resolution

For this add a small change to `upstream.md`.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  hello-git git:(trunk) vim upstream.md
➜  hello-git git:(trunk) git diff
[trunk 980fe2d] yaya
 1 file changed, 1 insertion(+), 1 deletion(-)

diff --git a/upstream.md b/upstream.md
index b39813a..5691d02 100644
--- a/upstream.md
+++ b/upstream.md
@@ -1 +1 @@
-upstream change
+upstream change -- yaya
➜  hello-git git:(trunk) git add .
➜  hello-git git:(trunk) git commit -m 'yaya'
```

now in `remote-git`

```bash
➜  remote-git git:(trunk) git pull origin trunk --rebase
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 248 bytes | 248.00 KiB/s, done.
From ../hello-git
 * branch            trunk      -> FETCH_HEAD
   99df23c..980fe2d  trunk      -> origin/trunk
➜  remote-git git:(trunk) git log --oneline -5
Successfully rebased and updated refs/heads/trunk.
c28b45c (HEAD -> trunk) A + 5
2107110 A + 4
980fe2d (origin/trunk) yaya
99df23c small new line
fac2b82 A + 6
```

Lets go!  Our conflict this time was auto played for us!

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Ours and Theirs
Sometimes during a conflict you just want to choose the entire file from one
side or the other of a conflict.  Typically in an editor this is a very simple
task, but it can as easily be done from the command line.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Ours and Theirs
"Its just the worst with git" - Some Coding Guy

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Ours Vs Theirs
* Ours is the change of the current branch
* Theirs is the change of the incomming branch

To select theirs or ours use the following checkout command
```bash
git checkout --ours README.md #use "ours" change
git checkout --theirs README.md #use "theirs" change
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

### Problem / Setup
Lets create a conflict again, in README.md
1. `hello-git` make it `A + 7`
1. `remote-git` make it `A + 8`
1. merge from upstream and resolved the conflict with "ours"
1. validate you have merged the changes via git log

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  remote-git git:(trunk) vim README.md
➜  remote-git git:(trunk) git add .
➜  remote-git git:(trunk) git commit -m 'A + 8'
[trunk 6ec352b] A + 7
 1 file changed, 1 insertion(+), 1 deletion(-)
```

```bash
➜  hello-git git:(trunk) vim README.md
➜  hello-git git:(trunk) git add .
➜  hello-git git:(trunk) git commit -m 'A + 7'
[trunk f5b13f5] A + 8
 1 file changed, 1 insertion(+), 1 deletion(-)
```

Lets do a merge instead of a rebase and pull from origin in remote-git

```bash
➜  remote-git git:(trunk) git pull origin trunk
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 323 bytes | 323.00 KiB/s, done.
From ../hello-git
 * branch            trunk      -> FETCH_HEAD
   980fe2d..f5b13f5  trunk      -> origin/trunk
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Recorded preimage for 'README.md'
Automatic merge failed; fix conflicts and then commit the result.
```

Now, lets resolve this by selecting _our_ change.

```bash
➜  remote-git git:(trunk) git checkout --ours README.md
Updated 1 path from the index
➜  remote-git git:(trunk) git status
On branch trunk
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
➜  remote-git git:(trunk) git add .
➜  remote-git git:(trunk) git commit -m "merged"
Recorded resolution for 'README.md'.
[trunk ec6930d] merged
```

We have now officially "merged" with ours, and you can verify this by opening
up README.md and looking at the contents.

```bash
A + 7
D
E
remote-change
downstream change
```

You can even see the change in the log for A + 8, but we still maintain 7

```bash
➜  remote-git git:(trunk) git log --oneline -5 --graph
*   ec6930d (HEAD -> trunk) merged
|\
| * f5b13f5 (origin/trunk) A + 8
* | 6ec352b A + 7
* | c28b45c A + 5
* | 2107110 A + 4
|/
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

### A good reminder
1. you don't want to mix merge and rebase.  I typically just try to stick with
   rebasing my branch and ff-merge on public branches.
2. long lived feature branches just suck. rerere helps, but they still suck


Lets do this again, but use rebase.  But before we do, since have mixed merge
and rebase, lets push our changes to `hello-git` or else things will get hairy
quickly.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

#### Don't forget
Don't forget to change branches in hello-git

```bash
➜  hello-git git:(trunk) git checkout bar
Switched to branch 'bar'
```

push to hello-git
```bash
➜  remote-git git:(trunk) git push origin trunk
Enumerating objects: 16, done.
Counting objects: 100% (15/15), done.
Delta compression using up to 12 threads
Compressing objects: 100% (8/8), done.
Writing objects: 100% (10/10), 1.05 KiB | 1.05 MiB/s, done.
Total 10 (delta 1), reused 0 (delta 0), pack-reused 0
To ../hello-git
   f5b13f5..ec6930d  trunk -> trunk
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
Perform the same task as before except with rebase

1. `hello-git` make it `A + 9`
1. `remote-git` make it `A + 10`
1. rebase from upstream and resolved the conflict with "ours."
1. do not `git rebase --continue`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
remote-git
```bash
➜  remote-git git:(trunk) vim README.md
➜  remote-git git:(trunk) git add .
➜  remote-git git:(trunk) git commit -m 'A + 10'
[trunk 120df5e] A + 9
 1 file changed, 1 insertion(+), 1 deletion(-)
```

hello-git
```bash
➜  hello-git git:(trunk) vim README.md
➜  hello-git git:(trunk) git add .
➜  hello-git git:(trunk) git commit -m 'A + 9'
[trunk d53a122] A + 10
 1 file changed, 1 insertion(+), 1 deletion(-)
```

Now lets pull again from remote but with --rebase enabled

```bash
➜  remote-git git:(trunk) git pull origin trunk --rebase
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 315 bytes | 315.00 KiB/s, done.
From ../hello-git
 * branch            trunk      -> FETCH_HEAD
   ec6930d..d53a122  trunk      -> origin/trunk
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
error: could not apply 120df5e... A + 9
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Recorded preimage for 'README.md'
Could not apply 120df5e... A + 9
```

Perfect, we now can select _ours_ via `git checkout`

```bash
➜  remote-git git:(d53a122) git checkout --ours README.md
Updated 1 path from the index
```

Before we commit, lets take a look at the contents

```bash
A + 9
D
E
remote-change
downstream change
```

wait... a second.  That isn't the contents we wanted!  We wanted _ours_.  What
happened?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Note
Well remember the rebase operations

1. checkout the branch we are _rebasing on_. (that means checkout hello git)
2. replay our changes on top of that updated branch one at a time

So that means when we hit a conflict:
* `ours` IS `hello-git`, the branch we are replaying on.
* `theirs` IS `remote-git`, the commits we are replaying one at a time.

It is the easiest part of git to screw up and i personally remember it as
"rebase is backwards."  Or you can remember the steps of rebase and come to the
same conclusion.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Instead of using `ours` use `theirs` then `--continue` the rebase.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  remote-git git:(d53a122) git checkout --theirs README.md
Updated 1 path from the index
```

Now lets inspect the contents

```bash
A + 10
D
E
remote-change
downstream change
```

nice

git add && rebase --continue to move on

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Interactive Rebase
There is ackshually more to rebase?  Yes.  Lets talk about interactive rebases,
which are quite useful.  The primary use case is squashing which can be very
nice for history

you can also edit individual commit messages and more, but i haven't ever
really done that in my 10+ years of git'ing

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Setup the repo with 3 sample commits on `remote-git`.  To make things easy,
make the commit message something like "added 1 to the end" and add 1 to the
end of `README.md`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  remote-git git:(trunk) vim README.md
➜  remote-git git:(trunk) git add .
➜  remote-git git:(trunk) git commit -m 'Added 1 to the end'
[trunk 9ebedbd] Added 1 to the end
 1 file changed, 1 insertion(+)
➜  remote-git git:(trunk) vim README.md
➜  remote-git git:(trunk) git add .
➜  remote-git git:(trunk) git commit -m 'Added 2 to the end'
[trunk 8456d89] Added 2 to the end
 1 file changed, 1 insertion(+)
➜  remote-git git:(trunk) vim README.md
➜  remote-git git:(trunk) git add .
➜  remote-git git:(trunk) git commit -m 'Added 3 to the end'
[trunk f000c2e] Added 3 to the end
 1 file changed, 1 insertion(+)
```

The contents of README.md

```bash
A + 9
D
E
remote-change
downstream change
1
2
3
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

### Interactive Rebase Steps
To begin an interactive rebase we need to provide a point in time to rebase
with.  Typically, the simplest way to do this is with `HEAD~<number>`.
`HEAD~1` means one commit back from `HEAD`.  Since we did 3 commits we would
use `HEAD~3` to select the base where we were before our 3 commits.

<br>
<br>

```bash
git rebase -i <commitish>
```

<br>
<br>

That means rebase `<commitish>`, interactively, to the current commit (`HEAD`
in this case)

<br>
<br>

You will be presented an editor with all the options.  Read them carefully.

<br>
<br>

#### Commitish?
Yes, an odd word, but it makes sense when you think about it.  There is a whole
language to describe commits, HEAD~1 is a very common version of this.

<br>
<br>

That means with rebase you could provide the exact commit, or a relative path
to the commit hash (HEAD~1)

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
`SQUASH` the three commits you made into one commit.  This will require you to
execute the rebase command, a `HEAD~<number>`, and read the text that appears
to understand how to squash.  It may take one or more tries.  Remember, if you
goof up you can always use reflog to get back to the original commit you
started at.

<br>
<br>

validate that you have created a squashed commit out of the 3 commits

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
execute the following:

<br>
<br>

```bash
git rebase -i HEAD~3
```

<br>
<br>

This means we will interactive rebase the last 3 commits.

<br>
<br>

You should get presented with the following

<br>
<br>

```bash
pick 9ebedbd Added 1 to the end
pick 8456d89 Added 2 to the end
pick f000c2e Added 3 to the end

# Rebase 9f67690..f000c2e onto 9f67690 (3 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup [-C | -c] <commit> = like "squash" but keep only the previous
#                    commit's log message, unless -C is used, in which case
#                    keep only this commit's message; -c is same as -C but
#                    opens the editor
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified); use -c <commit> to reword the commit message
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
```

<br>
<br>

The key line in the text comments is
```
# s, squash <commit> = use commit, but meld into previous commit
```

<br>
<br>


This means that if we replace `pick` with `s` or `squash` we will squash that
commit, or `meld into previous commit`.  Meaning make the previous commit and
squash commit become one commit.

<br>
<br>

```bash
pick 9ebedbd Added 1 to the end
squash 8456d89 Added 2 to the end
squash f000c2e Added 3 to the end
```

<br>
<br>

We could have also done
```bash
pick 9ebedbd Added 1 to the end
s 8456d89 Added 2 to the end
s f000c2e Added 3 to the end
```

<br>
<br>

Save and exit and git will present a new screen

```bash
# This is a combination of 3 commits.
# This is the 1st commit message:

Added 1 to the end

# This is the commit message #2:

Added 2 to the end

# This is the commit message #3:

Added 3 to the end

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Sun Feb 25 08:50:40 2024 -0700
#
# interactive rebase in progress; onto 9f67690
# Last commands done (3 commands done):
#    squash 8456d89 Added 2 to the end
#    squash f000c2e Added 3 to the end
# No commands remaining.
# You are currently rebasing branch 'trunk' on '9f67690'.
#
# Changes to be committed:
#	modified:   README.md
#
```

<br>
<br>

Now you have the chance to create a whole new commit message for the newly
combined commits.

<br>
<br>

Lets edit the message slightly

```bash
# This is a combination of 3 commits.
# This is the 1st commit message:

1, 2, and 3 combined

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Sun Feb 25 08:50:40 2024 -0700
#
# interactive rebase in progress; onto 9f67690
# Last commands done (3 commands done):
#    squash 8456d89 Added 2 to the end
#    squash f000c2e Added 3 to the end
# No commands remaining.
# You are currently rebasing branch 'trunk' on '9f67690'.
#
# Changes to be committed:
#	modified:   README.md
#
```

<br>
<br>

Once you save, you should see something similar:

```bash
➜  remote-git git:(trunk) git rebase -i HEAD~3
[detached HEAD 02d3a0f] 1, 2, and 3 combined
 Date: Sun Feb 25 08:50:40 2024 -0700
 1 file changed, 3 insertions(+)
Successfully rebased and updated refs/heads/trunk.
```
<br>
<br>


Lets look at our logs

```bash
➜  remote-git git:(trunk) git log --oneline -7
02d3a0f (HEAD -> trunk) 1, 2, and 3 combined
9f67690 A + 9
d53a122 (origin/trunk) A + 10
ec6930d merged
f5b13f5 A + 8
6ec352b A + 7
c28b45c A + 5
```

<br>
<br>


Look at that!  Our three commits became one!  Squashing can be quite an
effective technique to keep the history clean and allow you to make many small
commits throughout your dev cycle, preventing loss work, and then one clean
commit for reviewers.  I personally think this is one of the best ways to go
about developing.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### My general workflow
1. many small commits with a message "SQUASHME: <message>"
1. at the end of the dev cycle, i squash and give a proper message
1. PR with a singular commit
1. before i PR i ensure i am at the tip of the branch and that any CI runs against latest master changes

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

