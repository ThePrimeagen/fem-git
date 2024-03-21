---
title: "Branching in Git"
description: "Sometimes you need to branch out"
---

## Git Branching
You don't always want to be developing on the main branch.  Sometimes you need
a feature that is developed off the main line, such that you can return to the
main line, update the code, branch off, and perform some immediate needed fix.

<br>
<br>

This is where git branches come in.  They are _cheap_, virtually free, to
create.  With our of understanding git internals this will become much more
clear throughout this section

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Since we are in a new git repo, lets create our initial commit.  We will be
using the `hello-git` you just created previously to test the `trunk` default
branch setting

<br>
<br>

I would like a `README.md` with one line, `A`, and a commit message of `A`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  hello-git git:(trunk) echo "A" > README.md
➜  hello-git git:(trunk) git add .
➜  hello-git git:(trunk) git commit -m 'A'

[trunk (root-commit) cb75afe] A
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
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

### Creating branches
Creating branches is easy

```bash
git branch foo
```

Will create a new branch named `foo`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Try it out, create a branch named `foo`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  hello-git git:(trunk) git branch foo
➜  hello-git git:(trunk)
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

### Well... what happened?

* A branch was created pointing to the same commit as `trunk`
* We remain on `trunk`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### List out branches
We can observe this by executing `git branch`.  `git branch` will display all
of our local branches

```bash
git branch
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
List out your branches.  How do you know what branch you are on?  Use `git log`
with `--decorate` to see where the branches point at and prove that `foo`
points to the same commit as `trunk`

<br>
<br>

**Side Note**
If you use `git log` and you see the same output as when you use `--decorate`
this happens.  There are some internal settings that automagically make this
happen

```
git log > out
cat out
```

you may notice that you lose the branching information that you had.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
git branch

  foo
* trunk
(END)
```

That shows we have 2 branches, one `foo` and one `trunk`.  `trunk` is selected,
and you can tell because of the `*`

lets execute `git log` and write down our sha for this specific commit

```bash
commit cb75afebfac407bfc860dd854b626322a6dc8345 (HEAD -> trunk, foo)
Author: ThePrimeagen <the.primeagen@gmail.com>
Date:   Sun Jan 28 10:04:01 2024 -0700
```

You will notice that the commit has `HEAD -> trunk, foo`.  foo is pointing to
this commit.  A branch _POINTS_ to a commit, it is not a set of changes.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### A Touch of Git Foo
Can you find your branch details in `.git`?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Ahh yes... its right there...

```bash
➜  hello-git git:(trunk) cat .git/refs/heads/foo
cb75afebfac407bfc860dd854b626322a6dc8345
➜  hello-git git:(trunk) cat .git/refs/heads/trunk
cb75afebfac407bfc860dd854b626322a6dc8345
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



### Switching branches
You can switch branches easily in two ways

```bash
git switch <branch name>
git checkout <branch name>
```

`checkout` is a more versatile operation and i am personally just in the habbit
of using it.  You can use whatever you want / are comfortable with

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
switch to branch foo

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  hello-git git:(trunk) git checkout foo
Switched to branch 'foo'
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
Now that you are `foo` lets create another commit with message `B` and a new
line added to `second.md` of "B"

<br>
<br>

Then do it again, except replace `B` with `C`

<br>
<br>

When finished use `git log` with what options you want, to see if you can see
some differences now!

<br>
<br>

This should be the setup you should have at the end of this

```bash
    B - C foo
   /
  A       trunk
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
➜  hello-git git:(foo) echo "B" > second.md
➜  hello-git git:(foo) git add .
➜  hello-git git:(foo) git commit -m 'B'

[foo 4ad6ccf] B
 1 file changed, 1 insertion(+)
 create mode 100644 second.md
```

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
Author: ThePrimeagen <the.primeagen@gmail.com>
Date:   Sun Jan 28 10:17:11 2024 -0700

    C

commit 4ad6ccf530547ce89cfd90cd7a6e747f9531d917
Author: ThePrimeagen <the.primeagen@gmail.com>
Date:   Sun Jan 28 10:15:38 2024 -0700

    B

commit cb75afebfac407bfc860dd854b626322a6dc8345 (trunk)
Author: ThePrimeagen <the.primeagen@gmail.com>
Date:   Sun Jan 28 10:04:01 2024 -0700

    A
```

2 things:
1. Notice that `HEAD -> foo`, or current location of git points to foo, which
   is `16984cb`
1. `trunk` is still at `A`.  Trunk has not been updated with this code

#### Fun side-note
* try out `--graph` if you didn't!
* try out `--oneline` if you haven't
* try all three options together!

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### You can delete branches
If you create a branch and you wish to delete it you can use `-d` and `-D`.
For more information read up in the git manual, `man git-branch`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Commits locked in...
Ok, now that we have a branch full of changes... what do we do with it?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

