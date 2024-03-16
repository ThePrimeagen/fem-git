---
title: "Cool Git Commands",
description: "just some fun commands you should know",
---

## quickies

### commit --ammend --no-edit

```bash
➜  hello-git git:(trunk) git log --oneline -3
7488b35 (HEAD -> trunk, tag: git-reset-start, foo-bar) Revert "E"
d53a122 A + 10
ec6930d merged
➜  hello-git git:(trunk) echo "hello" >> README.md
➜  hello-git git:(trunk) ✗ git add .
➜  hello-git git:(trunk) ✗ git commit --amend --no-edit
[trunk f427d4c] Revert "E"
 Date: Sun Feb 25 20:35:16 2024 -0700
 2 files changed, 1 insertion(+), 1 deletion(-)
 create mode 100644 foo.md
```

You will notice that the commit message doesn't change and there isn't a new
commit.  But it does change the commit sha which means you have to force push
to origin if you change history.

```bash
➜  hello-git git:(trunk) git push origin trunk --force # i didn't execute this
```

`--force` says to ignore the history of the branch you are pushing too, you
have better knowledge.  Can be dangerous!  Never force push to a public branch

### reset
Another easy way to edit history without changing the commit is from earlier,
`git reset --soft HEAD~1`

```bash
➜  hello-git git:(trunk) git reset --soft HEAD~1
➜  hello-git git:(trunk) ✗ git add .
➜  hello-git git:(trunk) ✗ git status
On branch trunk
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   README.md
        new file:   foo.md

➜  hello-git git:(trunk) ✗ git commit -m "ci work"
[trunk d0ba67f] ci work
 2 files changed, 1 insertion(+), 1 deletion(-)
 create mode 100644 foo.md
```

Again, i have edited history therefore i will be needing to push to remote.

#### NOTE
The general rule is the following: Never rebase a public branch (i.e.: `main`).

## Too many remotes

Once you have any level of complication to an application and several
developers you will find that you have many remotes.  When this happens and you
want to update _ALL_ of them, use:

```bash
git fetch --all
```

## Tracking

### Problem
create a new branch in `remote-git` with any name you chose.

Try to push it to the remote via `git push`.

### Solution

```bash

➜  remote-git git:(trunk) git checkout -b foo-bar-baz
Switched to a new branch 'foo-bar-baz'
➜  remote-git git:(foo-bar-baz) git push
fatal: The current branch foo-bar-baz has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin foo-bar-baz

➜  remote-git git:(foo-bar-baz)
```

Branch tracking is amazing.  What this means is that you can set which remote a
branch belongs to and then `git push` will push directly to that specific
remote and `git pull` will pull the contents from the remote and put them into
the specific branch.

```bash
## Checks out a remote branch and tracks it to that specific remote.
git checkout --track <remote>/<branch>

## When you push changes you can also tell the branch to track itself to this
## remote.
git push -u <remote> <branch>

## You can also just tell the branch which remote to track
git branch -u <remote>/<branch>
```

Since the branch does not exist on the origin we have to push with `-u`
```bash
➜  remote-git git:(foo-bar-baz) git push -u origin foo-bar-baz

Enumerating objects: 59, done.
Counting objects: 100% (59/59), done.
Delta compression using up to 12 threads
Compressing objects: 100% (41/41), done.
Writing objects: 100% (56/56), 5.46 KiB | 2.73 MiB/s, done.
Total 56 (delta 5), reused 0 (delta 0), pack-reused 0
To ../hello-git
 * [new branch]      foo-bar-baz -> foo-bar-baz
Branch 'foo-bar-baz' set up to track remote branch 'foo-bar-baz' from 'origin'.
➜  remote-git git:(foo-bar-baz)
```

Notice that the push operation with `-u` flag added an extra line of output

```
Branch 'foo-bar-baz' set up to track remote branch 'foo-bar-baz' from 'origin'.
```

## Tags
These are tremendously helpful!  They allow you to name a commit and have it be
immutable.  Think of tags as unchangable branches.

```bash
git tag <name> # to create
git tag -d <name> # delete
git tag # to list
git checkout <tagname> # to checkout a tag
```

### Problem
Create a tag! and then list out your tags!  Do tags show up in `log`

### Solution
They do show up!  Even with a nice `tag: ` prefix

```bash
➜  hello-git git:(trunk) git tag my-first-tag # creates a tag at the current location
➜  hello-git git:(trunk) git tag # list tags
my-first-tag
➜  hello-git git:(trunk) git log --oneline -3
d0ba67f (HEAD -> trunk, tag: my-first-tag) ci work
d53a122 A + 10
ec6930d merged
```

If you check out a tag, you go into `detached HEAD`.  This is because tags are
immutable and you cannot change them, so checking them out is no different than
checking out a commit by its sha

```bash
➜  hello-git git:(trunk) git checkout my-first-tag
Note: switching to 'my-first-tag'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at d0ba67f ci work
➜  hello-git git:(d0ba67f)
```

