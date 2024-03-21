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
Create and add 3 values with the `section` `fem` (frontend masters) but
different keynames and we will use this for our platform for exploration.

<br>
<br>

I'll use:

```bash
fem.dev is great
fem.marc is ok
fem.git would
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
➜  my-first-git git:(master) git config --add fem.dev "is great"
➜  my-first-git git:(master) git config --add fem.marc "is ok"
➜  my-first-git git:(master) git config --add fem.git "would"
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
* `--get-regexp <regex>`: this takes a pattern and looks for all names matching

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Can you list out all of `fem` value's with a single command?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  my-first-git git:(master) git config --get-regexp fem
fem.dev is great
fem.marc is ok
fem.git would
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
that you have added.  Take `fem.dev` as an example.  Can you change that value
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
➜  my-first-git git:(master) git config --add fem.dev "is amazing"
```

But when we list out the values we see

```bash
➜  my-first-git git:(master) git config --get-regexp fem
fem.dev is great
fem.marc is ok
fem.git would
fem.dev is amazing
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
➜  my-first-git git:(master) git config --get fem.dev
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
try to remove our duplicate key with `--unset`.  If you don't know how to do
this, check out the man page, `man git-config` and search for `unset`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
➜  my-first-git git:(master) git config --unset fem.dev
warning: fem.dev has multiple values
```

Notice that we get a warning.  Does this mean we have removed one of the key
value pairs?  Or all of them?  Well lets list out what we have

```bash
➜  my-first-git git:(master) git config --get-all fem.dev
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
➜  my-first-git git:(master) git config --unset-all fem.dev
➜  my-first-git git:(master) git config --get fem.dev
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

### Fun fact
configuration is just a file.  that means your local configurations are
probably in the `.git` folder

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
find the git config in the `.git` folder and cat it out

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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

```toml
➜  my-first-git-repo git:(master) cat .git/config
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[fem]
        marc = is ok
        git = would
```

Sometimes when the magic is gone its almost disappointing...

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
Yeah!  We were able to unset all of fem.dev's values!  We can unset all fem.*
values by using `--remove-section`.  Remember a `key` is actually two parts.
Use `--get-regexp` to validate you have removed everything

<br>
<br>

`<section>.<key>`.  So remove the entire `fem` namespace

<br>
<br>

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
➜  my-first-git git:(master) git config --remove-section fem
➜  my-first-git git:(master) git config --get-regexp fem

(END)
```

We have removed all the `fem` keys!  Lets go!

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
through --global, whereas our `fem` additions were --local since we didn't
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
Try adding the same key `fem.dev` to `--local` and `--global` locations (one
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
➜  my-first-git git:(master) git config --global --add fem.dev "is amazing"
➜  my-first-git git:(master) git config --local --add fem.dev "is amazing2"
➜  my-first-git git:(master) git config --get-regexp fem
fem.dev is amazing
fem.dev is amazing2
(END)

➜  my-first-git git:(master) git config --list --local
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
fem.dev=is amazing2
(END)
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
➜  my-first-git git:(master) git config --get fem.dev
is amazing2
```

That grabs the local version, but I also added it second.  Lets add another
Global property and see if we still grab local first

```bash
➜  my-first-git git:(master) git config --global --add fem.dev "is amazing3"
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

```bash
➜  my-first-git git:(master) git config --get fem.dev
is amazing2
```

So that means the following:
1. grabs from more specific to less specific
1. gets the latest value from the most specific category

<br>
<br>

### To prove #2
try executing `git config --get --global fem.dev`.  We should get `is greatest`

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

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
Lets make our default branch from `master` to `trunk`.  The config setting is
`init.defaultBranch`

<br>
<br>

### Question
should this be a local or global change?

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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
have to be rename.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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

