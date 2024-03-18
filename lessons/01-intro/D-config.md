---
title: "Configuring Git"
description: "There are some reasons to configure Git"
---

## git config
Earlier we went over the basics of `git config` by setting your user name and
email.

```bash
git config --add --global user.name "ThePrimeagen"
git config --add --global user.email "the.primeagen@aol.com"
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

### --add
In the previous section we discussed adding and added user.name and user.email.
Remember every `key` is made up of two distinct parts, `section` and `keyname`

```bash
git config --add <section>.<keyname> <value>
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
Create and add 3 values with the `section` `boot` but different keynames and we
will use this for our platform for exploration.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  my-first-git git:(master) git config --add boot.dev "is great"
➜  my-first-git git:(master) git config --add boot.lane "is ok"
➜  my-first-git git:(master) git config --add boot.git "would"
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

### Listing out values
There are a couple ways you can list out config values.
* `--list`: where it will list out the entirety of the config.
* `--git-regexp <regex>`: this takes a pattern and looks for all names matching

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Can you list out all of `boot` value's with a single command?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  my-first-git git:(master) git config --get-regexp boot
boot.dev is great
boot.lane is ok
boot.git would
(END)
```

To tell you the truth, I personally end up using `git config --list | grep
<thing>` more often though it is not equivalent, it

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Using what you know currently, can you change one of your values of your config
that you have added.  Take `boot.dev` as an example.  Can you change that value
to `is amazing!` instead?  Verify that you have changed the value with
`--get-regexp`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
We have only talked about `--add` and `--list`.  by using `--add` we would
execute the following:

```bash
➜  my-first-git git:(master) git config --add boot.dev "is amazing"
```

But when we list out the values we see

```bash
➜  my-first-git git:(master) git config --get-regexp boot
boot.dev is great
boot.lane is ok
boot.git would
boot.dev is amazing
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
What has happened here?

This may come as a bit of a surprise but config keys are not unique.  You can
have the same key more than once.

instead of using `--list` lets use `--get <key>` and see what comes out

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  my-first-git git:(master) git config --get boot.dev
is amazing
```

Notice that it got the latest value.  This seems pretty reasonable default.
You can get all of the values of a key by using `--get-all`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Unsetting
You can "unset" a value and you can remove an entire section.  Lets do both.

`--unset`: Unsets one key
`--unset-all`: Unsets all matching keys

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Use `--unset` and remove a duplicate key.  Then use any command you can think
of to see what key was removed by git's `--unset`

### Solution
```bash
➜  my-first-git git:(master) git config --unset boot.dev
warning: boot.dev has multiple values
```

Notice that we get a warning.  Does this mean we have removed one of the key
value pairs?  Or all of them?  Well lets list out what we have

```bash
➜  my-first-git git:(master) git config --get-all boot.dev
is great
is amazing
(END)
```

That means we did not delete a single key!  You cannot unset when there are
multiple keys.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Repeat above but use `--unset-all`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  my-first-git git:(master) git config --unset-all boot.dev
➜  my-first-git git:(master) git config --get boot.dev
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
Yeah!  We were able to unset all of boot.dev's values!  We can unset all boot.*
values by using `--remove-section`.  Remember a `key` is actually two parts.

`<section>.<key>`.  So remove the entire `boot` namespace

If you don't know how to use `--remove-section`, jump into the main page and
use search to find the section

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  my-first-git git:(master) git config --remove-section boot
➜  my-first-git git:(master) git config --get-regexp boot

(END)
```

We have removed all the `boot` keys!  Lets go!

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Locations
We have briefly touched on location.  There are several locations for git
configs and they merge together to produce your final config.

The locations are:
--system, --global, --local, --worktree and you can provide a file path for the
config via --file <filename>

When we added our user.name and email, we did it for all of our projects
through --global, whereas our `boot` additions were --local since we didn't
specify where to add them to.

You can use `--local` and `--global` when using `--get`, `--list`, `--add`, and `--unset`.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Try adding the same key `boot.dev` to `--local` and `--global` locations (one
at a time) and see what happens when you execute `--list` and `--get-regexp`

add to `--global` first

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  my-first-git git:(master) git config --global --add boot.dev "is amazing"
➜  my-first-git git:(master) git config --local --add boot.dev "is amazing2"
➜  my-first-git git:(master) git config --get-regexp boot
boot.dev is amazing
boot.dev is amazing2
(END)

➜  my-first-git git:(master) git config --list --local
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
boot.dev=is amazing2
(END)
```

Notice that using `--local` with `--list` allows us to see just the local
values, but there is nothing preventing a global and a local value showing up.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
But what about `--get`?  Does that choose the "latest" or does it choose
differently?  Can you suss out the ordering mechanism for what value is
retrieved via `--get`?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  my-first-git git:(master) git config --get boot.dev
is amazing2
```

That grabs the local version, but I also added it second.  Lets add another
Global property and see if we still grab local first

```bash
➜  my-first-git git:(master) git config --global --add boot.dev "is amazing3"
➜  my-first-git git:(master) git config --get boot.dev
is amazing2
```

So that means the following:
1. grabs from more specific to less specific
1. gets the latest value from the most specific category

### Like all of programming
Git config probably at one point seemed a bit weird and you executed one
command that long time ago and you haven't done anything since.

<br>
<br>

As you can see, config is really simple now that we have taken the time to
learn it.  As always I encourage you to review the git manual

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Lets rename our `master` branch to `trunk`.  Now we want to do this globally
and not just for our specific project so we need to make sure we set correct
flags:  set the key `init.defaultBranch` to the value of `trunk`, globally.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
git config --global init.defaultBranch trunk
```
This will change the default setting for all projects going forward.  This does
not mean that our current git projects have changed.  Current projects will
have to rename and push to their remotes.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### Lets try it out!
Lets try creating a new git repository.  You will want to do this part as we
will use this project for the rest of the course.

```bash
cd to/where/i/create/projects/at
mkdir hello-git
cd hello-git
git init
```

Whatever name you have chosen should show up now as the default branch!

```bash
➜  hello-git git init
Initialized empty Git repository in /home/ThePrimeagen/personal/hello-git/.git/
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

