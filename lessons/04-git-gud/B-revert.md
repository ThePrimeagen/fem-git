---
title: "Revert"
description: "Sometimes you got to revert"
---

## The Decision We All Face
Often you have one of two choices to make: push forward or roll back.  Rolling
back can often require you to revert changes made to the main branch.

<br>
<br>

#### Note
In case you are confused about revert and restore

<br>
<br>

This is different than `git restore` since we are not restoring a file to a
previous commit, but instead we are commiting an inverted commit to the
graph to effectively "remove" a commit.

<br>
<br>

If the commit is matter, git revert is the anti-matter

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Git Revert
Reverting is simple.  You just need to provide the commit(ish).
```bash
git revert <commitish>
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
Navigate to `hello-git` project and revert commit with log message `E`.  Before
removing it, check the contents of the commit via `log` with `-p`!

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
One thing we could do

```bash
➜  hello-git git:(trunk) git log -p --grep E
d53a122 (HEAD -> trunk) A + 10
ec6930d merged
f5b13f5 A + 8
6ec352b A + 7
c28b45c A + 5
2107110 A + 4
980fe2d yaya
99df23c small new line
fac2b82 A + 6
958f33f A + 3
b51e34a Merge branch 'trunk' of ../hello-git into trunk
a9cb358 no conflict
d8a2f95 Merge branch 'trunk' of ../hello-git into trunk
9648be0 A + 1
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

Using log -p to look at the contents of E using `--grep` and `-p`
```bash
➜  hello-git git:(trunk) git log -p --grep E
commit a665b08996994c2e6620a6367b0ab524be221cb2
Author: ThePrimeagen <the.primeagen@gmail.com>
Date:   Sun Jan 28 10:56:37 2024 -0700

    E

diff --git a/README.md b/README.md
index b2f0a9a..d3045e2 100644
--- a/README.md
+++ b/README.md
@@ -1,2 +1,3 @@
 A
 D
+E
```

Revert E

```bash
➜  hello-git git:(trunk) git revert a665b08
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
error: could not revert a665b08... E
hint: After resolving the conflicts, mark them with
hint: "git add/rm <pathspec>", then run
hint: "git revert --continue".
hint: You can instead skip this commit with "git revert --skip".
hint: To abort and get back to the state before "git revert",
hint: run "git revert --abort".
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

### HALT
Yes, conflicts  can happen while reverting. Lets fix the conflict and finish
this revert.  Resloving them are a lot like rebase.  figure out the code you
want to keep and `git revert --continue`

```bash
➜  hello-git git:(trunk) git status
On branch trunk
You are currently reverting commit a665b08.
  (fix conflicts and run "git revert --continue")
  (use "git revert --skip" to skip this patch)
  (use "git revert --abort" to cancel the revert operation)

Unmerged paths:
  (use "git restore --staged <file>..." to unstage)
  (use "git add <file>..." to mark resolution)
        both modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

`git status` shows the conflict is within README.md

<br>
<br>

```bash
A + 10
D
<<<<<<< HEAD
E
remote-change
downstream change
=======
>>>>>>> parent of a665b08 (E)
```

You can see right away that one change set contains

<br>
<br>

```bash
E
remote-change
downstream change
```

<br>
<br>

and the other contains nothing (our revert commit).

<br>
<br>

This means that git cannot tell how to revert this commit cleanly.  We can help
by keeping the contents below E but removing E

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Finish the revert by fixing the conflict then use `git revert --continue`

<br>
<br>

use git log to see what the revert looks like

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Merge conflict fixed

```bash
A + 10
D
remote-change
downstream change
```

Now, just like rebase, we have to `git revert --continue`

```bash
➜  hello-git git:(trunk) git add .
➜  hello-git git:(trunk) git revert --continue

Revert "E"

This reverts commit a665b08996994c2e6620a6367b0ab524be221cb2.

# Conflicts:
#	README.md

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch trunk
# You are currently reverting commit a665b08.
#
# Changes to be committed:
#	modified:   README.md
#

[trunk 7488b35] Revert "E"
 1 file changed, 1 deletion(-)
```

The commit message contains a nice message explaining exactly what commit you
revert if any history looker decides to peruse the commits.  A git log -p -1
shows the contents of the change too

```bash
commit 7488b357e64cf426eaa3390a6c406e2d10f63f40 (HEAD -> trunk)
Author: ThePrimeagen <ThePrimeagen@netflix.com>
Date:   Sun Feb 25 20:35:16 2024 -0700

    Revert "E"

    This reverts commit a665b08996994c2e6620a6367b0ab524be221cb2.

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

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

