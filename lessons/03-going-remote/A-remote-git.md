---
title: "Remote"
description: "Sometimes my problems are just one pull away"
---

## Remote Git
Often we need code changes that have been created by our fellow frienemies.
But how do we get their changes into our repo?  Or how do we push our changes
to someone else repo?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### It doesn't have to be remote...
Often we think of remote repos as github or gitlab, but it doesn't have to be
that way.

<br>
<br>

If you have never used git, maybe remote is an odd term.

<br>
<br>

A remote is simply a copy of the repo _somewhere else_.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Create a new repo, `remote-git`.  Lets initialize it as an empty git repo using
`git init`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  hello-git git:(trunk) cd /path/to/project
git init remote-git

Initialized empty Git repository in /path/to/project/remote-git/.git/
➜  project cd remote-git
➜  remote-git git:(trunk)
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

### Distributed Version Control
* A remote is just another git repo that is of the same project and has changes
  we may need.

To add a remote the syntax is:

```
git remote add <name> <uri>
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
Add `hello-git` as a remote.  You should name it `origin`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  remote-git git:(trunk) git remote add origin ../hello-git
```

To check you can execute `git remote -v` to list out your remotes and their
locations

<br>
<br>

```bash
➜  remote-git git:(trunk) git remote -v
origin  ../hello-git (fetch)
origin  ../hello-git (push)
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

## Gitism
There are two naming conventions i have seen:

<br>
<br>

### Remote is project repo
typically when you have a remote git repo its called `origin`.  This is the
_source of truth_ repo.

<br>
<br>

### Remote is your fork of some other repo
sometimes you have _your_ remote repo (fork), which you will name `origin` and
you have the project repo which is typically named `upstream`.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### NOTICE:
Earlier we set our git default branch name to `trunk` notice that it remained
even on this new project.

Now we need to `merge` in the changes from `hello-git` into _our_ new repo
`remote-git`.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Fetch
we can fetch all the git state from our remote repository by executing `git
fetch`.  This wont update the current branches checked out, just where the
`origin/*` has them set to.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Try fetching all the branches from `hello-git` by using `git fetch`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  remote-git git:(trunk) git fetch
remote: Enumerating objects: 28, done.
remote: Counting objects: 100% (28/28), done.
remote: Compressing objects: 100% (18/18), done.
remote: Total 28 (delta 5), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (28/28), 2.15 KiB | 2.15 MiB/s, done.
From ../hello-git
 * [new branch]      bar              -> origin/bar
 * [new branch]      foo              -> origin/foo
 * [new branch]      foo-rebase-trunk -> origin/foo-rebase-trunk
 * [new branch]      trunk            -> origin/trunk
 * [new branch]      trunk-merge-foo  -> origin/trunk-merge-foo
```

Now those branches should look familiar considering we just got done making
them!

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Using `git log` can you verify that the remote's trunk has been correctly
merged into our git state but the current trunk we have checked out is not up
to date?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
First we can see our trunk has no commits
```bash
➜  remote-git git:(trunk) git log
fatal: your current branch 'trunk' does not have any commits yet
```

`trunk` remains in its current state
`origin/trunk` has been updated

We verify we have the remote commits in our git state

```bash
➜  remote-git git:(trunk) git log origin/trunk
b23e632 (origin/trunk, origin/bar) Y
2f43452 X
a665b08 E
79c5076 D
cb75afe A
```

Notice the branch names pointing to commit `b23e632` are `origin/trunk` and
`origin/bar`.

<br>
<br>

### Note
IMPORTANT: Branch names can contain the remote they come from.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### NOTICE
if we wish to see what `branch`es came down with `git fetch` we can execute
`git branch -a` (git branch all) to see all branches that currently exist.

<br>
<br>

Anytime you see a branch that is `<remote>/<name>` it means that it is the last
known state of the `<remote>` repo's `<name>` (branch).  Practically what this
means is that at any moment you are likely out of sync with your remote.  That
is ok.  That is the wonders of git.

<br>
<br>

You don't have to update, you can use yours until you are ready to fetch in changes.

<br>
<br>

Now convenience is a real thing.  I try to update regularly.  The large the out
of sync is, the likelihood of conflict goes north

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Lets update our `trunk` with `origin/trunk`.  Then lets use `git log` to
validate those commits have been merged

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  remote-git git:(trunk) git merge origin/trunk
➜  remote-git git:(trunk)
```

Well... that wasn't very climatic was it?  Lets see what we got!

```bash
➜  remote-git git:(trunk) git log --oneline
b23e632 (HEAD -> trunk, origin/trunk, origin/bar) Y
2f43452 X
a665b08 E
79c5076 D
cb75afe A
```

You will now see that our local `trunk` matches our `origin/trunk`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Git pull
Fetching is always a good idea.  It keeps your local repo's remotes up to date,
but it doesn't alter your state.

<br>
<br>

The thing is that a lot of the time you just want the changes merged for you
into your branch.  This can be done with one convenient command, `git pull`

<br>
<br>

```bash
git pull <remote> <branch>
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
1. Add a line at the end of `README.md` in `hello-git` and commit it with message
   `A remote change`.
1. Execute `git pull` in `remote-git`
1. Think about the error.  Why does it exist?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  remote-git git:(trunk) cd ../hello-git
➜  hello-git git:(trunk) echo "remote-change" >> README.md
➜  hello-git git:(trunk) git add README.md
➜  hello-git git:(trunk) git commit -m "A remote change"
[trunk 42afc8d] A remote change
 1 file changed, 1 insertion(+)
```

<br>
<br>

Now lets swap back to `remote-git` and pull in the changes instead of `fetch` /
`merge`

<br>
<br>

```bash
➜  remote-git git:(trunk) git pull
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 271 bytes | 271.00 KiB/s, done.
From ../hello-git
   b23e632..42afc8d  trunk      -> origin/trunk
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> trunk
```

<br>
<br>

You will notice we failed.  Why?  The reason is that we did not setup our
`trunk` branch to track the `origin/trunk` branch.  Git will not automatically
track state in a "remote" because that may not be what you want to do.
Therefore if you `git pull` it wont know where to pull from since nothing has
been specified.  This becomes even more obvious once you have more than one
remote.

<br>
<br>

Its often convenient to setup tracking because we can use `push` and `pull`
without specifying the target branch.  Git assumes that just because two
branches have the same name doesn't mean they are the same.  So you need to
tell git to track branches manually _on preexisting branches_.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Read the error again and figure out how to "track" the remote branch.  Once you
execute the command then `git pull` to the origin.  Use the tracking command
that does not involve `git pull`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  remote-git git:(trunk) git branch --set-upstream-to=origin/trunk trunk

Branch 'trunk' set up to track remote branch 'trunk' from 'origin'.
➜  remote-git git:(trunk) git pull # Now git pull is successful!
Updating b23e632..42afc8d
Fast-forward
 README.md | 1 +
 1 file changed, 1 insertion(+)
```

Since the changes were `fetch`ed with our previous `pull` we didn't have to
re-fetch, but instead we simply merged with a "fast forward" merge strategy.

<br>
<br>

Remember all of our merge and rebase talk?  Well now it really applies here.
We were able to fast forward merge because we made no changes to trunk.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Think about Github
Hopefully this gives you some insight into what is actually happening with
github/gitlab/stash.  Its just another repo that everyone has agreed to commit
to

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### What about rebase?
Typically whenever I pull in changes from the remote authority repo I will
`rebase` the changes.  I do not like adding in a bunch of merge commits.  This
is a personal preference, but I think its a superiour one :)

<br>
<br>

- A long lived branch with a bunch of merge commits is much more difficult to revert.
- If every change is a single commit, then the ability to revert is very trivial.
- I prefer to test my changes against the current state of master not against the current state i have fetched

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### There is a config for it
There are two strategies for `rebase`ing changes.

1. add --rebase flag to a pull.  `git pull --rebase`
1. edit your config to make this behavior the default behavior. (good thing we
   know about how the config works)

```bash
➜  remote-git git:(trunk) git config --add --global pull.rebase true
```

<br>
<br>

**Please don't update that setting now though**

<br>
<br>

Now we rebase remote code every time.  You don't have to set this if you don't
like the idea of rebasing from authority

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Git !pull
The opposite of pull?

<br>
<br>

Yes, `git push`

<br>
<br>

If you wish take your changes and move the remote repo, you can do this by
using `git push`.  Much like pull, if you are not "tracking" then you cannot
simply `git push` but instead you will have to specify the remote and branch
name.

<br>
<br>

Lets make some changes to `bar` and push them to `hello-git`

<br>
<br>

#### Fun Facts
* `git push <remote> <local_name>:<remote_name>` allows you to push and have it received with a different name
* `git push <remote> :<remote_name>` will delete a branch on the remote

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Create a single commit, "CHANGE FROM REMOTE", as a one line change to the end
of README.md to branch `bar`.  Then push the changes back to `hello-git`

<br>
<br>

validate the change made it to `hello-git`'s `bar` branch

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  remote-git git:(trunk) git checkout bar
Branch 'bar' set up to track remote branch 'bar' from 'origin'.
Switched to a new branch 'bar'
➜  remote-git git:(bar) echo "Change from remote" >> README.md
➜  remote-git git:(bar) git add README.md
➜  remote-git git:(bar) git commit -m 'CHANGE FROM REMOTE'
[bar aab17e0] CHANGE FROM REMOTE
 1 file changed, 1 insertion(+)
```

### NOTICE
When we checked out `bar` we automatically started tracking.  We didn't have to
_create_ the branch.  This is because locally we do not have a `bar` branch,
but `origin` does.  Also `origin` is our _ONLY_ remote. Therefore it makes
sense to create a local branch and track the remote it came from.

<br>
<br>

Now lets push our change

<br>
<br>

```bash
➜  remote-git git:(bar) git push
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 12 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 306 bytes | 306.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To ../hello-git
   b23e632..aab17e0  bar -> bar
```

<br>
<br>

And lets see if we see this on `hello-git`

<br>
<br>

```bash
➜  remote-git git:(bar) cd ../hello-git
➜  hello-git git:(trunk) git log bar
aab17e0 (bar) CHANGE FROM REMOTE
b23e632 Y
2f43452 X
a665b08 E
79c5076 D
cb75afe A
```

<br>
<br>

Well look at that!  We are sharing changes now!

<br>
<br>

The astute observer will notice that `bar`'s branch on hello-git was moved
forward... why?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

