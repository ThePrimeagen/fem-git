---
title: "A Bit Of Plumbing"
description: "time to get the hands dirty"
---

## Git internal representations
To understand git you must go past the porceline and check out the plumbing

<br>
<br>

### SHAs
git commits come with a sha (a hash with 0-9a-f characters).

<br>
<br>

You can specify the first 7 characters of a sha for git to identify what you
are referring to.

<br>
<br>

To get the sha of a commit, we can use the `log` command and copy the long hex string

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Find the commit sha of your first commit (copy it to your system clipboard)

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
git log
```

You should see something like this
```bash
commit 5ba786fcc93e8092831c01e71444b9baa2228a4f (HEAD -> master)
Author: ThePrimeagen <the.primeagen@gmail.com>
Date:   Sun Jan 21 19:40:56 2024 -0700

    batman
```

```bash
commit 5ba786fcc93e8092831c01e71444b9baa2228a4f
---------^ this is the sha
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


### Question
Why is your sha different than mine?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
sha: SHA stands for Secure Hashing Algorithm. SHA is a modified version of MD5

Second, your sha, if you remember me saying, is the combination of changes you
have made, author, time, etc etc

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
This next part will be a bit hard, but its doable.

<br>
<br>

Can you find your commit's file(s) of data within `.git` folder.  See if you
can `cat` out any data

<br>
<br>

**hint**:
Your commit's sha is a key piece of information

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

**hint**:
The first 2 characters of the commit sha...

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Upon initial look via ls, you will see a nothing that looks familiar.

```bash
➜  my-first-git git:(master) ls -la .git
total 52
drwxrwxr-x  8 ThePrimeagen ThePrimeagen 4096 Jan 21 12:10 .
drwxrwxr-x  3 ThePrimeagen ThePrimeagen 4096 Jan 21 12:10 ..
drwxrwxr-x  2 ThePrimeagen ThePrimeagen 4096 Jan 21 10:29 branches
-rw-rw-r--  1 ThePrimeagen ThePrimeagen    7 Jan 21 12:10 COMMIT_EDITMSG
-rw-rw-r--  1 ThePrimeagen ThePrimeagen   92 Jan 21 10:29 config
-rw-rw-r--  1 ThePrimeagen ThePrimeagen   73 Jan 21 10:29 description
-rw-rw-r--  1 ThePrimeagen ThePrimeagen   23 Jan 21 10:29 HEAD
drwxrwxr-x  2 ThePrimeagen ThePrimeagen 4096 Jan 21 10:29 hooks
-rw-rw-r--  1 ThePrimeagen ThePrimeagen  209 Jan 21 12:10 index
drwxrwxr-x  2 ThePrimeagen ThePrimeagen 4096 Jan 21 10:29 info
drwxrwxr-x  3 ThePrimeagen ThePrimeagen 4096 Jan 21 10:52 logs
drwxrwxr-x 10 ThePrimeagen ThePrimeagen 4096 Jan 21 12:10 objects
drwxrwxr-x  4 ThePrimeagen ThePrimeagen 4096 Jan 21 10:29 refs
```

But within the `objects` directory you should see at least one interesting entry

```bash
➜  my-first-git git:(master) ls -la .git/objects
total 28
drwxrwxr-x 7 ThePrimeagen ThePrimeagen 4096 Jan 21 19:40 .
drwxrwxr-x 8 ThePrimeagen ThePrimeagen 4096 Jan 21 19:40 ..
drwxrwxr-x 2 ThePrimeagen ThePrimeagen 4096 Jan 21 19:40 4e
drwxrwxr-x 2 ThePrimeagen ThePrimeagen 4096 Jan 21 19:40 5b
drwxrwxr-x 2 ThePrimeagen ThePrimeagen 4096 Jan 21 19:40 9a
drwxrwxr-x 2 ThePrimeagen ThePrimeagen 4096 Jan 21 19:40 info
drwxrwxr-x 2 ThePrimeagen ThePrimeagen 4096 Jan 21 19:40 pack
```

Do you see anything that is familiar in here?

<br>
<br>

I do.  My commit, `5ba786fcc93e8092831c01e71444b9baa2228a4f` starts with `5b`
and so does a directory here.  Lets ls that directory

```bash
➜  my-first-git git:(master) ls -la .git/objects/5b
total 12
drwxrwxr-x 2 ThePrimeagen ThePrimeagen 4096 Jan 21 19:40 .
drwxrwxr-x 7 ThePrimeagen ThePrimeagen 4096 Jan 21 19:40 ..
-r--r--r-- 1 ThePrimeagen ThePrimeagen  125 Jan 21 19:40 a786fcc93e8092831c01e71444b9baa2228a4f
```

You may now notice again that `a786...` is the remaining part of my commit sha,
and yours exists in the same format!

<br>
<br>

#### Observation
Commits exist in the `.git/objects` directory with the first 2 letters as a
directory, and the remaining 38 as a file.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## What's in its Pocketses, Precious?

If we try to cat out the commit file we see nothing useful
```bash
➜  cat .git/objects/5b/a786fcc93e8092831c01e71444b9baa2228a4f

x[ )@͢<41M]V%qP9C'*"iܣUfmA"DqFx3-C(U˅-YIw]0y6y1  @uڟ`V?9r%
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

### Remember
ALL of git state is stored in files.  everything.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### The Tools of the Plumber
There are ways to inspect files within the git's data store.

```bash
git cat-file -p <some-sha>
```

This will echo out the contents of the sha.  This can be a commit, a tree, or a
blob (more on those in a bit)

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Can you get `git cat-file -p <sha>` to echo out the contents of `first.md` by
inspecting the commit sha?  You may have to have a few rounds of catting

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
First start by `git cat-file -p <your commit sha>`.  You should see something
similar.

```bash
➜  git cat-file -p 5ba786fcc93e8092831c01e71444b9baa2228a4f

tree 4e507fdc6d9044ccd8a4a3061324c9f711c4667d
author ThePrimeagen <the.primeagen@gmail.com> 1705891256 -0700
committer ThePrimeagen <the.primeagen@gmail.com> 1705891256 -0700

batman
```

You will notice there is no `first.md` or its contents therefore we must be
able to find something... Oh look at that, `tree` has a sha, lets try that

```bash
➜  git cat-file -p 4e507fdc6d9044ccd8a4a3061324c9f711c4667d

100644 blob 9a71f81a4b4754b686fd37cbb3c72d0250d344aa    first.md
```

Wait... is that first.md...

```bash
➜  git cat-file -p 9a71f81a4b4754b686fd37cbb3c72d0250d344aa

hello world
```

Well, well, well, look at what the VSC has drug in... if it isn't our file!
Blob `9a71f81a4b4754b686fd37cbb3c72d0250d344aa` is literally first.md at the
point of our first commit.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Key Concepts
* tree: tree is analagous to directory
* blob: blob is analagous to file

<br>
<br>

You probably noticed that the tree (directory), when cat'd, contains a single
entry, a blob, which was named first.md.

<br>
<br>

### BIG TAKEAWAY
Git does not store diffs, git stores complete version of the entire source at
the point of each commit. In other words, each commit contains all the
information to completely reconstruct the source code that was tracked.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## A Second Change
### Problem
With your amazing git skillz, create a second file, `second.md`, insert some
text, stage, and commit the file.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
vim second.md # Great editor to add such a wonderful change
git add second.md
git commit -m "second"

[master 48d06ff] second
 1 file changed, 2 insertions(+)
 create mode 100644 second.md
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
Explore your new git commit.  What can you say about `first.md`?  What about
`second.md`?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Use `git log` to get commit sha of your previous commit or use the line from
your commit message `[master 48d06ff] second`.

Now lets list out the contents of that commit
```bash
➜  my-first-git git:(master) git cat-file -p 48d06ff
tree 6282551fedc655bc5ee9180ad67021c22245fdae
parent 5ba786fcc93e8092831c01e71444b9baa2228a4f
author ThePrimeagen <the.primeagen@gmail.com> 1706387467 -0700
committer ThePrimeagen <the.primeagen@gmail.com> 1706387467 -0700

second
```

### Parents
All but the first commit will have a parent.  Notice that our new commit has a
parent!

<br>

Inspect the tree with `git cat-file -p 6282551fedc655bc5ee9180ad67021c22245fdae`

```bash
➜  my-first-git git:(master) git cat-file -p 6282551fedc655bc5ee9180ad67021c22245fdae
100644 blob 9a71f81a4b4754b686fd37cbb3c72d0250d344aa    first.md
100644 blob 7f112b196b963ff72675febdbb97da5204f9497e    second.md
```

Now compare the original tree from our first commit

```bash
➜  my-first-git git:(master) git cat-file -p 4e507fdc6d9044ccd8a4a3061324c9f711c4667d

100644 blob 9a71f81a4b4754b686fd37cbb3c72d0250d344aa    first.md
```

<br>
<br>

Notice that our commit, `48d06ff`, has the _same_ `first.md` file, but with a
newly added `second.md`! So to manually reconstruct the entire file tree thus
far, we just need to cat-file both first.md and second.md from the current
commhut!

<br>
<br>

That also means that git stores _pointers_ to the ENTIRE worktree, not the
entire worktree itself which means its significantly more efficient space wise.

<br>
<br>

hopefully this makes git feel less magical.  Just remember at some point every
program is just a bunch of if statements, for loops, and variables.  This is
true even about git

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### For Them
Create an inner directory and do it again and show them the inner trees!

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
