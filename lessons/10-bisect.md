---
title: "Git Bisect"
description: "When you done messed up."
---

## The repo needing to be downloaded
git clone git@github.com:ThePrimeagen/git-bisect.git

## A Classic Problem
* somewhere in the last 500 commits something has gone wrong.
* To test if something has gone wrong takes several minutes or longer.

This is not a common problem, but it is a problem you will run into in the real
world.  And it can be a complete pain to resolve if you do not know the tools
you have at your disposal.

## Logs
One very straight forward strategy to determine where a bug began is manually
reviewing logs and identifying when a file changed.

### Pros
* If you know the file/module in which the bug exists and the file changes
infrequently then logs can be a very fast method to identify the problem
commit.

* If people take seriously their commit messages and add key words then
searching can provide a fast and powerful mechanism to find the correct change.

### Cons
* The file/module in which contains the bug changes frequently
* Poor/No commit messages
* You cannot boil down the bug to a specific key word (e.g.: Widget)
* If there are too many commits that match searching it can much more
  cumbersome than using other methods
* You simply don't know any keywords to narrow down your search

## Searching with token
### Becoming familiar
Before you begin with the exercise it will be good to be familiar with the
repo.

1. install any deps with `npm i`
1. run tests with `npm run test`

You should see something similar if everything went well
```
⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯ Failed Tests 1 ⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯

 FAIL  src/index.spec.js > foo
AssertionError: expected 140 to deeply equal 138

- Expected
+ Received

- 138
+ 140

 ❯ src/index.spec.js:6:20
      4| test("foo", async () => {
      5|     await (new Promise(res => setTimeout(res, 30000)));
      6|     expect(foo(2)).toEqual(2 * 69);
       |                    ^
      7| }, 35000);
      8|

⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯[1/1]⎯

 Test Files  1 failed (1)
      Tests  1 failed (1)
   Start at  15:46:36
   Duration  30.46s (transform 44ms, setup 0ms, collect 18ms, tests 30.11s, environment 0ms, prepare 107ms)
```

#### Observation
we know our function foo and test foo are failing

## grep and git log
`git log` comes with a `--grep` option

```bash
git log --grep "<term>"
```

This will search through the commits and look in the commit message for
`<term>`.

#### A bonus info
`-p` will show you the commits of the change as well.  This repo's changes are
very small so you can use this along with `--grep` and easily read the changes.

### Problem
Can you find the wrong commit using `--grep`?

### Solution
```bash
➜  git-bisect git:(master) git log -p --grep "foo"
```

Shockingly, we have spotted our code.
```bash
commit 3798398f377cd1722284ff30b211e3b66e218738
Author: mpaulson <mpaulson@netflix.com>
Date:   Fri Feb 16 13:20:16 2024 -0700

    feat: altered foo to meet new specifications

diff --git a/src/index.js b/src/index.js
index 4f35883..e8052cc 100644
--- a/src/index.js
+++ b/src/index.js
@@ -4,5 +4,5 @@
  * @return {number}
  */
 export function foo(x) {
-    return x * 69;
+    return x * 70;
 }
```

Now this could be a "lucky" search here.  We happened to find it but in the
real world this may not be nearly so easy.  But its good to know the tool
exists.

You could image that each testing run takes 30 minutes, so spending 5 minutes
to potentially find the bug instead of 3 hours is a trade off i would make any
day of the week.  Of course this also assumes you know exactly which file has
fallen apart, and more specifically for us, there isn't a simple test to fix :)

## Searching with filename
One flaw in our previous search is that we were looking for occurrences of a
word in commit messages which may not be the most efficient way to look.  We
know the file that is probably the problem, we could always check its history.

To search files via `git log`
```bash
git log -p -- file1 file2...
```

### Problem
Search through potential files using

### Solution

```bash
git log -p -- src/index.js
```

You may not notice, but this method only shows changes to that specific file
instead of the full patch change.  This allows for a bit faster perusing of the
file and its change.  Again, in this trivial example, it is easy to spot the
offending commit!

## Bisect
But log searching just simply may fail or the code is complex enough that it
isn't possible to simply look and understand.  So what do we do?  Well we need
to search through the repository for the offending commit that changed the
code.

To perform `biscet` you need two things to be true

#### Property 1
All commits are ordered.  They are ordered by time.

#### Property 2
You know a commit that the issue is not present

The reason property 1 is so important is the following:

* If there is a problem that is currently plaguing the project and you go back
10 commits and observe the problem is still there. You don't have to check 9
commits back, or 8 back, you know that the problem currently exists and 10
commits ago its present.  Therefore its present betwixt HEAD and 10 commits ago

This implies a very important concept

```
  ----------------------------------
  |                                |
  a ---------- unknown ----------- b
```

If `a` is working commit, and `b` doesn't work, select the middle commit
between `a` and `b`, call it `c`

```
  ---------------------------------
  |                               |
  a -- unknown -- c -- unknown -- b
```

### Problem
if `c` turns out to be a commit that passes the test (it works) what observation can we make about the above graphic?

### Solution
All commits between `a` and `c` are good.  Which results in the following graph

```
  ----------------------------------
  |                                |
  a --- good --- c --- unknown --- b
```

### Problem
If `c` fails, what observations can we made about the original graph?

### Solution
All commits between `c` and `b` are bad.  Which results in the following graph
```
  ---------------------------------
  |                               |
  a --- unknown --- c --- bad --- b
```

#### Observation
we can repeat this process to find our offending commit that broke the test!

To repeat we need to select new bounds. So instead of `a` and `b`, we repeat
with the bounds of

* if `c` is good, `c` and `b`
* if `c` is bad, `a` and `c`

We have cut our search space in half!

If you don't recognize this algorithm it is the same principal that guides
binary search.

### Pros
* You need not to know anything about the bug and you don't have to rely on
commit messages.  Simply a failing test case.

* It is the fastest way to search a sorted space

* You don't have to be searching only for a bug but for any change in your
project.  You can also use this to _find_ where a bug got fixed, where a
performance regression happened, or anything else you can think of.

### Cons
* not really any... Unless its a trivial bug that works via log searching this
is pretty much the best possible option

### Performing bisect
Git bisect requires you to have a last known good commit and a last known bad
commit.  From there it is able to perform the binary search.

The basic steps of a bisect are the following

1. set the known good commit `git bisect start`
2. set the known bad commit `git bisect bad`, uses the current one
3. set the known bad commit `git bisect good <commit>`
4. test
5. `git bisect <good | bad>`
6. goto 4 until git tells you the commit

### Problem
Use the first commit of the repo and the current tip of `master` and find the
problematic commit via git bisect.

### Solution
```bash
# Starts the process
➜  git-bisect git:(master) git bisect start

# Sets current commit as the bad commit.  You can manually select any commit
➜  git-bisect git:(master) git bisect bad

# Find the initial commit
➜  git-bisect git:(master) git log --oneline

# Set the last known good commit to the first commit
➜  git-bisect git:(master) git bisect good b56ed57

# Test to see if we are working or not
➜  git-bisect git:(master) npm run test

...

 Test Files  1 failed (1)
      Tests  1 failed (1)
   Start at  10:49:35
   Duration  30.34s (transform 24ms, setup 1ms, collect 9ms, tests 30.11s, environment 0ms, prepare 74ms)
```

Looks like we have a bad commit so lets execute the following:

```bash
# This tells git that we have a bad commit
➜  git-bisect git:(8bf2c77) git bisect bad
Bisecting: 5 revisions left to test after this (roughly 3 steps)
[0562cd710d40d23b5928747a45d71d2a76443752] F
# notice that my shell shows i have changed commits
➜  git-bisect git:(0562cd7)
```

Lets test again!  Keep on testing until we have found the commit.

```bash
 ✓ src/index.spec.js (1) 30041ms
   ✓ foo 30040ms

 Test Files  1 passed (1)
      Tests  1 passed (1)
   Start at  10:53:21
   Duration  30.26s (transform 26ms, setup 0ms, collect 12ms, tests 30.04s, environment 0ms, prepare 60ms)
```

Ok we have made great success, therefore we need to tell git that we are successful.

```bash
➜  git-bisect git:(0562cd7) git bisect good
Bisecting: 2 revisions left to test after this (roughly 2 steps)
[eb867402c2e4678887c8aa322c0da3ef54851f7c] I
➜  git-bisect git:(eb86740)
```

Again, git checks out the next correct branch.  Lets keep running until we find
the offending commit.

Once you have successfully ran to the end, this is what you should see.
```bash
➜  git-bisect git:(3798398) git bisect bad
3798398f377cd1722284ff30b211e3b66e218738 is the first bad commit
commit 3798398f377cd1722284ff30b211e3b66e218738
Author: mpaulson <mpaulson@netflix.com>
Date:   Fri Feb 16 13:20:16 2024 -0700

    feat: altered foo to meet new specifications

 src/index.js | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

And that is the offending commit!

To finish up with your session, execute the following command.

```bash
➜  git-bisect git:(3798398) git bisect reset
Previous HEAD position was 3798398 feat: altered foo to meet new specifications
Switched to branch 'master'
```

## Git Bisect - Automated
Git bisect is great, but its a bit manual... right?  If only there was a way to
automate it

In steps `git bisect run`.  Effectively the exit code
```bash
# same setup as before with start, good, bad
git bisect run <cmd>
```

This will continue to run until we find the proper commit.  The run command
uses the command provided and determines a good and bad commit by exit code

### Problem
Use `git bisect run ./node_modules/.bin/vitest --run` to automate the testing
and find the same problem commit

### Solution
```bash
➜  git-bisect git:(master) git bisect start
➜  git-bisect git:(master) git log --oneline
➜  git-bisect git:(master) git bisect good b56ed57
➜  git-bisect git:(master) git bisect bad
➜  git-bisect git:(8bf2c77) git bisect run ./node_modules/.bin/vitest --run
running  './node_modules/.bin/vitest' '--run'

 RUN  v1.2.1 /home/mpaulson/personal/git-bisect

 ❯ src/index.spec.js (1) 30107ms
   × foo 30106ms

⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯ Failed Tests 1 ⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯

 FAIL  src/index.spec.js > foo
AssertionError: expected 140 to deeply equal 138

- Expected
+ Received

- 138
+ 140

 ❯ src/index.spec.js:6:20
      4| test("foo", async () => {
      5|     await (new Promise(res => setTimeout(res, 30000)));
      6|     expect(foo(2)).toEqual(2 * 69);
       |                    ^
      7| }, 35000);
      8|

⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯[1/1]⎯

 Test Files  1 failed (1)
      Tests  1 failed (1)
   Start at  20:15:58
   Duration  30.54s (transform 45ms, setup 0ms, collect 13ms, tests 30.11s, environment 0ms, prepare 135ms)

Bisecting: 5 revisions left to test after this (roughly 3 steps)
[0562cd710d40d23b5928747a45d71d2a76443752] F
running  './node_modules/.bin/vitest' '--run'

 RUN  v1.2.1 /home/mpaulson/personal/git-bisect

 ✓ src/index.spec.js (1) 30101ms
   ✓ foo 30099ms

 Test Files  1 passed (1)
      Tests  1 passed (1)
   Start at  20:16:29
   Duration  30.59s (transform 48ms, setup 0ms, collect 11ms, tests 30.10s, environment 0ms, prepare 162ms)

Bisecting: 2 revisions left to test after this (roughly 2 steps)
[eb867402c2e4678887c8aa322c0da3ef54851f7c] I
running  './node_modules/.bin/vitest' '--run'

 RUN  v1.2.1 /home/mpaulson/personal/git-bisect

 ✓ src/index.spec.js (1) 30070ms
   ✓ foo 30068ms

 Test Files  1 passed (1)
      Tests  1 passed (1)
   Start at  20:17:00
   Duration  30.59s (transform 76ms, setup 0ms, collect 13ms, tests 30.07s, environment 0ms, prepare 182ms)

Bisecting: 0 revisions left to test after this (roughly 1 step)
[972fa2ab45e5041a1fe8c95f31b520bc62d7af85] this commit is certainly not about foo
running  './node_modules/.bin/vitest' '--run'

 RUN  v1.2.1 /home/mpaulson/personal/git-bisect

 ❯ src/index.spec.js (1) 30080ms
   × foo 30079ms

⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯ Failed Tests 1 ⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯

 FAIL  src/index.spec.js > foo
AssertionError: expected 140 to deeply equal 138

- Expected
+ Received

- 138
+ 140

 ❯ src/index.spec.js:6:20
      4| test("foo", async () => {
      5|     await (new Promise(res => setTimeout(res, 30000)));
      6|     expect(foo(2)).toEqual(2 * 69);
       |                    ^
      7| }, 35000);
      8|

⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯[1/1]⎯

 Test Files  1 failed (1)
      Tests  1 failed (1)
   Start at  20:17:31
   Duration  30.60s (transform 106ms, setup 0ms, collect 18ms, tests 30.08s, environment 0ms, prepare 239ms)

Bisecting: 0 revisions left to test after this (roughly 0 steps)
[3798398f377cd1722284ff30b211e3b66e218738] feat: altered foo to meet new specifications
running  './node_modules/.bin/vitest' '--run'

 RUN  v1.2.1 /home/mpaulson/personal/git-bisect

 ❯ src/index.spec.js (1) 30111ms
   × foo 30111ms

⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯ Failed Tests 1 ⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯

 FAIL  src/index.spec.js > foo
AssertionError: expected 140 to deeply equal 138

- Expected
+ Received

- 138
+ 140

 ❯ src/index.spec.js:6:20
      4| test("foo", async () => {
      5|     await (new Promise(res => setTimeout(res, 30000)));
      6|     expect(foo(2)).toEqual(2 * 69);
       |                    ^
      7| }, 35000);
      8|

⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯[1/1]⎯

 Test Files  1 failed (1)
      Tests  1 failed (1)
   Start at  20:18:02
   Duration  30.57s (transform 27ms, setup 0ms, collect 9ms, tests 30.11s, environment 0ms, prepare 84ms)

3798398f377cd1722284ff30b211e3b66e218738 is the first bad commit
commit 3798398f377cd1722284ff30b211e3b66e218738
Author: mpaulson <mpaulson@netflix.com>
Date:   Fri Feb 16 13:20:16 2024 -0700

    feat: altered foo to meet new specifications

 src/index.js | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
bisect found first bad commit%
➜  git-bisect git:(3798398)
```

What.  That was easy!
