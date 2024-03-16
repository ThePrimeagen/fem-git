---
title: "Branching in Git"
description: "Sometimes you need to branch out"
---

## Git Branching
You don't always want to be developing on the main branch.  Sometimes you need
a feature that is developed off the main line, such that you can return to the
main line, update the code, branch off, and perform some immediate needed fix.

This is where git branches come in.  They are _cheap_, virtually free, to
create.  With our of understanding git internals this will become much more
clear throughout this section

### Before we create any branches
Since we are in a new git repo, lets create our initial commit.  We will be
using the `hello-git` you just created previously to test the `trunk` default
branch setting

```bash
➜  hello-git git:(trunk) echo "A" > README.md
➜  hello-git git:(trunk) git add .
➜  hello-git git:(trunk) git commit -m 'A'

[trunk (root-commit) cb75afe] A
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
```

### Creating branches
Creating branches is easy

```bash
➜  hello-git git:(trunk) git branch foo
➜  hello-git git:(trunk)
```

Well... what happened?

* A branch was created at the same point `trunk` is
* We remain on `trunk`

We can observe this by executing `git branch`.  `git branch` will display all
of our local branches

```bash
➜  hello-git git:(trunk) git branch
```

I see when executing this the following: (its its own screen)

```bash
  foo
* trunk
(END)
```

That shows we have 2 branches, one `foo` and one `trunk`.  `trunk` is selected,
and you can tell because of the `*`

lets execute `git log` and write down our sha for this specific commit

```bash
commit cb75afebfac407bfc860dd854b626322a6dc8345 (HEAD -> trunk, foo)
Author: mpaulson <the.primeagen@gmail.com>
Date:   Sun Jan 28 10:04:01 2024 -0700
```

You will notice that the commit has `HEAD -> trunk, foo`.  foo is pointing to
this commit.  A branch _POINTS_ to a commit, it is not a set of changes.

#### Important note
When you create a branch, the branch points to whatever commit you are
currently on.  That means if you are on branch `brand-new-feature` and you
create a new branch, `even-better-feature`, both `brand-new-feature` and
`even-better-feature` will be pointing to the same commit

now lets checkout our newly created branch, `foo`, with `git checkout`

```bash
➜  hello-git git:(trunk) git checkout foo
Switched to branch 'foo'
```

#### Fun Fact
you can use `git switch <branch-name>` to switch to an existing branch

And look at that, we are now on foo.  Lets make a change in foo!

```bash
➜  hello-git git:(foo) echo "B" > second.md
➜  hello-git git:(foo) git add .
➜  hello-git git:(foo) git commit -m 'B'

[foo 4ad6ccf] B
 1 file changed, 1 insertion(+)
 create mode 100644 second.md
```

Lets make one more change to foo!


```bash
➜  hello-git git:(foo) echo "C" > second.md
➜  hello-git git:(foo) git add .
➜  hello-git git:(foo) git commit -m 'C'

[foo 16984cb] C
 1 file changed, 1 insertion(+), 1 deletion(-)
```

Now lets execute `git log` and see if we can see a difference

```bash
commit 16984cb1725b0bfeff43f60f92b3467ee6d525e3 (HEAD -> foo)
Author: mpaulson <the.primeagen@gmail.com>
Date:   Sun Jan 28 10:17:11 2024 -0700

    C

commit 4ad6ccf530547ce89cfd90cd7a6e747f9531d917
Author: mpaulson <the.primeagen@gmail.com>
Date:   Sun Jan 28 10:15:38 2024 -0700

    B

commit cb75afebfac407bfc860dd854b626322a6dc8345 (trunk)
Author: mpaulson <the.primeagen@gmail.com>
Date:   Sun Jan 28 10:04:01 2024 -0700

    A
```

2 things:
1. Notice that `HEAD -> foo`, or current location of git points to foo, which
   is `16984cb`
1. `trunk` is still at `A`.  Trunk has not been updated with this code

#### Fun side-note
Remember we used `--graph` when executing git log.  Try it now and see the
difference!

### Commits locked in...
Ok, now that we have a branch full of changes... what do we do with it?

