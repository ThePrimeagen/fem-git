---
title: "Stashing"
description: "Sometimes you just need to stash some changes"
---

## Small note
I will often say the phrase _upstream_ to refer to `hello-git`.  This is
because i am treating `hello-git` similar to `github`.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## A Problem We All Face
You have made a change... but you need to pull in changes from a remote repo,
what do you do?

<br>
<br>

commit changes then rebase or merge? (i would pick rebase)

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### The problems of merge and #1
If you create a commit with changes that are half baked just to pull in
origin changes you will have your (potentially) broken commit, a merge
commit, then your fixing commit.  If you need to revert these changes, it will
get more difficult (more on reverting later).

<br>
<br>

### This is a good case for rebase
Your changes with rebase, will get put on top of all the incoming changes which
allows your partial commit easier to work with.  This style also makes
squashing easier

<br>
<br>

### But i would rather have my partial commit not committed yet
This is where stash comes in.

<br>
<br>

### Worktrees
Often the superior approach and we will discuss them later

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Stash
`git stash` will take every change _tracked_ by git (change to index + change
to work tree) and store that result, much like a commit, into the "stash."

To quote from the man page

> Use git stash when you want to record the current state of the working directory and the index, but want to go back to a clean working directory. The command saves your local modifications away and reverts the working directory to match the HEAD commit.

<br>
<br>

Stash is a STACK of temporary changes

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Operations
You can push your changes into the stack by using

```bash
git stash
```

<br>
<br>

Stashes, much like commits, can come with a message (`-m "<your message>"`)

<br>
<br>

```bash
git stash -m "my lovely message here"
```

<br>
<br>

Stashes can be listed out:

<br>
<br>

```bash
git stash list
git stash show [--index <index>]
```

<br>
<br>

To pop the latest stash:

<br>
<br>

```bash
git stash pop
```

<br>
<br>

To pop a stash at an index:

<br>
<br>

```bash
git stash pop --index <index> # works well with git stash list
```

<br>
<br>

#### Remember
`man git-stash` is your friend.  if you forget how a command works, please
review the manual first!  Its the authority of how to use the tool

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
use branch `trunk`

<br>
<br>

1. create an upstream change.  Commit a small change to hello-git

```bash
echo "upstream change" >> upstream.md
```

2. add a small change to `remote-git`  don't commit

```bash
echo "downstream change" >> README.md
```

3. now that we have an active tracked change pull in the upstream change

**What error do you get?**

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
1. create the origin changes

<br>
<br>

```bash
# make sure you are on branch trunk
cd /path/to/hello-git
echo "upstream change" >> upstream.md
git add upstream.md
git commit -m "upstream changes"

[trunk 6849c67] upstream changes
 1 file changed, 1 insertion(+)
 create mode 100644 upstream.md
```

<br>
<br>

2. create the downstream changes but do not commit

<br>
<br>

```bash
# make sure you are on branch trunk
cd /path/to/remote-git
echo "downstream change" >> README.md
```

<br>
<br>

3. validate that the changes are tracked by the worktree

```bash
➜  remote-git git:(trunk) git status
On branch trunk
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

<br>
<br>

4. Try to pull in origin changes

```bash
➜  remote-git git:(trunk) git pull

error: cannot pull with rebase: You have unstaged changes.
error: please commit or stash them.
```

<br>
<br>

#### Error

```
error: cannot pull with rebase: You have unstaged changes.
error: please commit or stash them.
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
Now that we have produced the error and the answer is clear: use stash.

<br>
<br>

Lets play around with stash a bit more.  To become more familiar perform the
following:

<br>
<br>

1. stash your changes
1. view your stash list
1. pop your stashed chages
1. stash your changes but with a custom message
1. create more changes and stash those so we have 2 in the list
1. pull in the upstream's changes

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
This will be a long set of changes, but they are all pretty simple.

<br>
<br>

1. stash your changes

```bash
➜  remote-git git:(trunk) git stash

Saved working directory and index state WIP on trunk: 42afc8d A remote change
➜  remote-git git:(trunk) git status
On branch trunk
nothing to commit, working tree clean
```

<br>
<br>

#### NOTE
If you have changes to non indexed files then they will not be added to the
stash command.  Careful not to lose them.

<br>
<br>

2. view your stash list

```bash
➜  remote-git git:(trunk) git stash list
stash@{0}: WIP on trunk: 42afc8d A remote change
(END)
```

<br>
<br>

To view the current stashed change (the one in position 0) use `show`

```bash
diff --git a/README.md b/README.md
index 9f276a6..2ca5a19 100644
--- a/README.md
+++ b/README.md
@@ -2,3 +2,4 @@ A
 D
 E
 remote-change
+downstream change
```

<br>
<br>

3. pop your stashed chages

```bash
➜  remote-git git:(trunk) git stash pop
On branch trunk
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (e318e20fb946c2a78700611129d9ae040b4cc80c)
```

<br>
<br>

4. stash your changes but with a custom message

```bash
➜  remote-git git:(trunk) git stash -m "my very nice change about..."
Saved working directory and index state On trunk: my very nice change about...
➜  remote-git git:(trunk) git stash list
stash@{0}: On trunk: my very nice change about...
```

<br>
<br>

5. create more changes and stash those so we have 2 in the list

<br>
<br>

#### Note
Having named stashes can be useful if you come back a week later to a project
and forgot what you have stashed / where you have put your changes

<br>
<br>

```bash
➜  remote-git git:(trunk) echo "some other change" >> README.md
➜  remote-git git:(trunk) git stash -m "other changes"
Saved working directory and index state On trunk: other changes
➜  remote-git git:(trunk) git stash list
stash@{0}: On trunk: other changes
stash@{1}: On trunk: my very nice change about...
```

<br>
<br>

6. pull in the upstream's changes

```bash
➜  remote-git git:(trunk) git pull
Updating 42afc8d..6849c67
Fast-forward
 upstream.md | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 upstream.md
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
Now we need to get back our original changes.  Please pop the changes of our
_first_ stash.  Remember the stash is a stack like data structure.  Therefore
your first change isn't the first item in the stash.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
We need to use `pop` and `--index` to accomplish this.

```bash
➜  remote-git git:(trunk) git stash pop --index 1
On branch trunk
Your branch is up to date with 'origin/trunk'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{1} (c58f8ce5300e4207031438236c5732901666a43a)
➜  remote-git git:(trunk) git commit -m 'greatest changes'
[trunk 7282922] greatest changes
 1 file changed, 1 insertion(+)
```

Stashing is quite powerful and allows you to be able to bring in upstream
changes without losing your work or creating commits which can be annoying to
deal with!

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Worktrees
We will talk more about these, but generally this is my favorite way to work in
a fast evolving codebase

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

