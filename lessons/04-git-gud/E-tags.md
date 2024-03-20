---
title: "Tags"
description: "You can mark a specific spot in code"
---

## Named locations within Git History
At some point there are changes in which represents a version of your sofware
you want named.

This could be a version you are releasing as an open source library or an
internal note for the public version of a website

Git has you covered with `tags`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### What is a tag?
The best way to think about a tag is a branch that cannot be changed.  It can
only be deleted

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### How to use tags

```bash
git tag <name> # to create
git tag -d <name> # delete
git tag # to list
git checkout <tagname> # to checkout a tag
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
Create a tag
<br>
List out your tags
<br>
Do tags show up in `log`?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  hello-git git:(trunk) git tag my-first-tag # creates a tag at the current location
➜  hello-git git:(trunk) git tag # list tags
my-first-tag
➜  hello-git git:(trunk) git log --oneline -3
d0ba67f (HEAD -> trunk, tag: my-first-tag) ci work
d53a122 A + 10
ec6930d merged
```

They do show up!  Even with a nice `tag: ` prefix

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

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Git push
You can push tags to a remote via `git push --tags` and pull via `git pull
--tags`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Pull your tag to `remote-git`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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

the `--tags` will fetch branches as well as tags

```bash
➜  remote-git git:(foo-bar-baz) git pull --tags
remote: Enumerating objects: 15, done.
remote: Counting objects: 100% (15/15), done.
remote: Compressing objects: 100% (7/7), done.
remote: Total 11 (delta 2), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (11/11), 951 bytes | 951.00 KiB/s, done.
From ../hello-git
 * [new branch]      baz             -> origin/baz
 * [new branch]      foo-bar         -> origin/foo-bar
   d53a122..d0ba67f  trunk           -> origin/trunk
 + 4f452da...e6d9d4b trunk2          -> origin/trunk2  (forced update)
 * [new tag]         my-first-tag    -> my-first-tag
Already up to date.
➜  remote-git git:(foo-bar-baz)
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

