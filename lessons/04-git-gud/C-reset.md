---
title: "Reset"
description: "An aggressive approach to becoming clean"
---

## Git reset
This has such a wide range of responsibilites from get rid of what is in the
worktree or the index to walking back a commit

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Soft
Git reset soft can be very useful if you need to make a commit that is
partially finished and you want to edit that commit and change the contents

```bash
➜  hello-git git:(trunk) git log -p --oneline -1
7488b35 (HEAD -> trunk) Revert "E"
diff --git a/README.md b/README.md
index eb42c13..38dc2c1 100644
--- a/README.md
+++ b/README.md
@@ -1,5 +1,4 @@
 A + 10
 D
-E
 remote-change
 downstream change
```

Now lets say we need to continue to edit our previous commit.  We have two options.

1. we could make changes and use `commit --amend`.  If you are not familiar
   with `git commit --amend` it allows you to meld the current staged changes
   into the previous commit and edit the message.
1. we could use `git reset --soft HEAD~1` to move `trunk` back one commit and
   alter the index and worktree to match the contents of the commit.

<br>
<br>

#### Note
Both operations will change the sha since we are changing the graph
fundamentally

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Use `git reset --soft HEAD~1` to move `trunk` back one commit.  This should
make the working state of your branch contain the changes of the revert instead
of the revert commit being in the graph

<br>
<br>

inspect state of your git branch via `git log` and `git status`

<br>
<br>

`git commit` back the reverted change with a new message

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  hello-git git:(trunk) git reset --soft HEAD~1
➜  hello-git git:(trunk) git log -p --oneline -1
d53a122 (HEAD -> trunk) A + 10
diff --git a/README.md b/README.md
index 68dae75..eb42c13 100644
--- a/README.md
+++ b/README.md
@@ -1,4 +1,4 @@
-A + 7
+A + 10
 D
 E
 remote-change
```

Notice `log -p` shows us that the current commit (-1) is not the revert.  We
have successfully moved back `trunk`!

now check out the git status!

```bash
➜  hello-git git:(trunk) git status
On branch trunk
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   README.md
```

Its the revert we just got done doing!

```bash
➜  hello-git git:(trunk) git diff --staged
diff --git a/README.md b/README.md
index eb42c13..38dc2c1 100644
--- a/README.md
+++ b/README.md
@@ -1,5 +1,4 @@
 A + 10
 D
-E
 remote-change
 downstream change
```

commit the soft reset contents

```bash
➜  hello-git git:(trunk) git commit -m 'its not just a revert anymore'
[trunk ce41293] its not just a revert anymore
 1 file changed, 1 deletion(-)
➜  hello-git git:(trunk) git log --oneline -2
ce41293 (HEAD -> trunk) its not just a revert anymore
d53a122 A + 10
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

### NOTE
Obviously this is dangerous as we have just altered history of a branch

The basics of soft is to reset the history to the point you want (HEAD~1) and
the index and worktree will contain the changes whence you came (Our revert in
this example).  We could have used soft reset and went back several commits and
we would have all their changes.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Hard
Hard will do the same thing as soft except it will drop changes to the `index`
and worktree.  That means any work that is being tracked by git will be
destroyed

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Before we try walking back a commit, lets first create a local change and see
what happens when we execute `git reset --hard`

Add a small change to `README.md` and create a new file, `foo.md`, with any
change you see fit, then execute `git reset --hard` which will destroy index
and worktree changes

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  hello-git git:(trunk) echo "HELLO" >> README.md
➜  hello-git git:(trunk) echo "Foo" > foo.md
➜  hello-git git:(trunk) git status
On branch trunk
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        foo.md

no changes added to commit (use "git add" and/or "git commit -a")
```

#### Note
* foo.md is not tracked by git
* README.md is tracked and the changes are present in the worktree

```bash
➜  hello-git git:(trunk) git reset --hard
HEAD is now at ce41293 its not just a revert anymore
➜  hello-git git:(trunk) git status
On branch trunk
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        foo.md

nothing added to commit but untracked files present (use "git add" to track)
```

Notice that it destroyed all work to README.md.  That is because --hard will
reset _ALL_ work in the worktree and index (staging area).  So even if we had
 `git add README.md` it would still have been reset back to the HEAD state.

foo.md did not get destroyed because git is not tracking the file in any way.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Destroy foo by using `git add` and `git reset --hard`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  hello-git git:(trunk) git add .
➜  hello-git git:(trunk) git reset --hard
HEAD is now at ce41293 its not just a revert anymore
➜  hello-git git:(trunk) git status
On branch trunk
nothing to commit, working tree clean
```

Notice that now we destroyed our work with foo.md.  That is because we started
to track foo.md by adding to the index (staging area).  Now when we called git
reset --hard it destroyed that work.

#### NOTE
CAUTION WITH RESET HARD

It can destroy your work and it can be virtually impossible to get back that
work.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Git reset --hard with HEAD~1
We can walk back our tree just like soft, except we discard the changes

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Try it now.  Try walking back our commit with `git reset --hard HEAD~1`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  hello-git git:(trunk) git reset --hard HEAD~1
HEAD is now at d53a122 A + 10
➜  hello-git git:(trunk) git status
On branch trunk
nothing to commit, working tree clean
```

Not only did we reset trunk one step back, we also discarded all changes.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Challenge Question!
Can you restore `hello-git` to the position it was BEFORE we started this section?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  hello-git git:(trunk) git reflog
... 14 other entries ... # i did extra work here
7488b35 (tag: git-reset-start) HEAD@{15}: commit: Revert "E"
```

So we can see our previous commit with `Revert "E"`.  Lets move `trunk` back to
there.

```bash
➜  hello-git git:(trunk) git checkout 7488b35
Note: switching to '7488b35'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 7488b35 Revert "E"
➜  hello-git git:(7488b35) git branch -D trunk
Deleted branch trunk (was d53a122).
➜  hello-git git:(7488b35) git checkout -b trunk
Switched to a new branch 'trunk'
➜  hello-git git:(trunk)
```

Look at that.  We have checked out that point in time, deleted our current
version of `trunk`, and then checked out a new version of trunk.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
There are more things you can do with `git reset` but i find i use restore and
git reset --hard only.  Git reset --hard to restore me back to HEAD cleanly and
restore to remove a staged file.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

