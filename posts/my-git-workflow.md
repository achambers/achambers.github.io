---
title: My Git Workflow
date: '2013-06-30'
description:
tags: [Git, Learning]
---

What does your git workflow look like?

Since I started using git about 2 years ago I've constantly been trying to 
improve my git workflow. As I began to form a clearer picture in my mnind of 
how git stores commits, how branches work, how merging works etc, I slowly refined 
how I go about committing to my projects. While I'm far from  a git 
guru, I'm pretty happy with my current workflow so thought I'd share it.  
  
##A clean slate

Firstly, make sure `master` is up to date.

```
df21be6 Aaron Chambers     o [master] [origin/master] Test commit
37cedd7 Aaron Chambers     I first commit
```

##Branch out and commit away

Now, create a local branch and commit until your heart's content.

```
~/code/my-project (master)$ git checkout -b bug-fix
Switched to a new branch 'bug-fix'

~/code/my-project (bug-fix)$ echo "Fixing bugs" >> README.md
~/code/my-project (bug-fix)$ git commit -am "Swatting bugs"
[bug-fix 2e2b000] Swatting bugs
 1 file changed, 1 insertion(+)

~/code/my-project (bug-fix)$ echo "Fixing more bugs" >> README.md
~/code/my-project (bug-fix)$ git commit -am "Getting out the bug spray"
[bug-fix 9917b19] Getting out the bug spray
 1 file changed, 1 insertion(+)

~/code/my-project (bug-fix)$ echo "Fixing yet more bugs" >> README.md
~/code/my-project (bug-fix)$ git commit -am "And now for the bug zapper"
[bug-fix 1fdd4c2] And now for the bug zapper
 1 file changed, 1 insertion(+)
```

If we have a quick look at the commit tree we can see things look pretty good. 
My branch commits come directly after the last commit on `master` (`df21be6`).

```
928041a Aaron Chambers     o [bug-fix] And now for the bug zapper
ed49f8b Aaron Chambers     o Getting out the bug spray
21dffcd Aaron Chambers     o Swatting bugs
df21be6 Aaron Chambers     o [master] [origin/master] Test commit
37cedd7 Aaron Chambers     I first commit
```

##Pull me oh master

Now we need to go back to `master` and see if anyone else has committed to it 
since we last pulled.

```
~/code/my-project (bug-fix)$ git checkout master
Switched to branch 'master'

~/code/my-project (master)$ git pull
remote: Counting objects: 8, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 6 (delta 0), reused 6 (delta 0)
Unpacking objects: 100% (6/6), done.
From github.com:achambers/test
   df21be6..deb4a17  master     -> origin/master
Updating df21be6..deb4a17
Fast-forward
 README.md | 2 ++
 1 file changed, 2 insertions(+)
```

Well, well, well. It seems someone has been pushing some work to `master`. We can 
see that the commit tree has branched at commit `df21be6`.

```
deb4a17 Will Smith         o [master] [origin/master] Made some more awesome
6597417 Will Smith         o Added awesome features
928041a Aaron Chambers     │ o [bug-fix] And now for the bug zapper
ed49f8b Aaron Chambers     │ o Getting out the bug spray
21dffcd Aaron Chambers     │ o Swatting bugs
df21be6 Aaron Chambers     I─┘ Test commit
37cedd7 Aaron Chambers     I first commit
```

I don't really have a good reason other than a touch of OCD but the whole reason 
I  use this workflow is because I am not a fan of merge commits. I like to see 
my commit history looking like a nice straight line. This is partially for 
asthetic reasons, and partially because I find it easier for my brain to digest 
when it is a straight line. Either way, I like to take pride in my commit history 
and I think this workflow does that well.

Therefore we are trying to AVOID the following.

```
fhd65j6 Aaron Chambers     M─┐ [master] [origin/master] Merged branch 'bug-fix'
deb4a17 Will Smith         o │ Made some more awesome
6597417 Will Smith         o │ Added awesome features
928041a Aaron Chambers     │ o And now for the bug zapper
ed49f8b Aaron Chambers     │ o Getting out the bug spray
21dffcd Aaron Chambers     │ o Swatting bugs
df21be6 Aaron Chambers     I─┘ Test commit
37cedd7 Aaron Chambers     I first commit
```

##All your rebase are belong to us

So, in order to get rid of that pesky merge commit, we need to checkout our 
`bug-fix` branch again and rebase it with `master`.

```
~/code/my-project (master)$ git checkout bug-fix
Switched to branch 'bug-fix'

~/code/my-project (bug-fix)$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: Swatting bugs
Applying: Getting out the bug spray
Applying: And now for the bug zapper
```

That's better. That's what I like to see. Our `bug-fix` branch now contains all 
the commits from `master`, and then our commits on top of those. In one sexy 
straight line. Nice! 

```
928041a Aaron Chambers     o [bug-fix] And now for the bug zapper
ed49f8b Aaron Chambers     o Getting out the bug spray
21dffcd Aaron Chambers     o Swatting bugs
deb4a17 Will Smith         o [master] [origin/master] Made some more awesome
6597417 Will Smith         o Added awesome features
df21be6 Aaron Chambers     o Test commit
37cedd7 Aaron Chambers     I first commit
```

##Wrap it all up

Finally, merge the `bug-fix` branch into `master` and we're done.

```
~/code/my-project (bug-fix)$ git checkout master
Switched to branch 'master'

~/code/my-project (master)$ git merge bug-fix
Updating deb4a17..928041a
Fast-forward
 README.md | 3 +++
 1 file changed, 3 insertions(+)

~/code/my-project (master)$ git push
Counting objects: 11, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (9/9), 823 bytes, done.
Total 9 (delta 0), reused 0 (delta 0)
To git@github.com:achambers/test.git
   deb4a17..928041a  master -> master

~/code/my-project (master)$ git branch -d bug-fix
Deleted branch bug-fix (was 928041a).
```

Check out that nice straight line.

```
2013-06-30 22:54 Aaron Chambers     o [master] [origin/master] And now for the bug zapper
2013-06-30 22:53 Aaron Chambers     o Getting out the bug spray
2013-06-30 22:53 Aaron Chambers     o Swatting bugs
2013-06-30 23:02 Will Smith         o Made some more awesome
2013-06-30 23:01 Will Smith         o Added awesome features
2013-06-30 22:49 Aaron Chambers     o Test commit
2013-06-30 22:47 Aaron Chambers     I first commit
```

One of the things I love about this workflow is that the merge to `master` is 
always a fast forward merge as the branch commits are always downstream of the 
master commits.

If you get yourself into the habit of rebasing with `master` often then you 
greatly decrease your chance of
having merge conflicts, and it also means you will always be developing on the 
lastest code in master.

So, I'll ask again. What does your git workflow look like?
