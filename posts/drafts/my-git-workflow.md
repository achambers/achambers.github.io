---
title: My Git Workflow
date: '2013-06-30'
description:
tags: []
---

```
2013-06-30 22:49 Aaron Chambers     o [master] [origin/master] Test commit
2013-06-30 22:47 Aaron Chambers     I first commit
```

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

```
2013-06-30 22:54 Aaron Chambers     o [bug-fix] And now for the bug zapper
2013-06-30 22:53 Aaron Chambers     o Getting out the bug spray
2013-06-30 22:53 Aaron Chambers     o Swatting bugs
2013-06-30 22:49 Aaron Chambers     o [master] [origin/master] Test commit
2013-06-30 22:47 Aaron Chambers     I first commit
```

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

```
2013-06-30 23:02 Will Smith         o [master] [origin/master] Made some more awesome
2013-06-30 23:01 Will Smith         o Added awesome features
2013-06-30 22:54 Aaron Chambers     │ o [bug-fix] And now for the bug zapper
2013-06-30 22:53 Aaron Chambers     │ o Getting out the bug spray
2013-06-30 22:53 Aaron Chambers     │ o Swatting bugs
2013-06-30 22:49 Aaron Chambers     I─┘ Test commit
2013-06-30 22:47 Aaron Chambers     I first commit
```


What we do not want (merge commit)

```
2013-06-30 23:11 Aaron Chambers     M─┐ [master] [origin/master] Merged branch 'bug-fix'
2013-06-30 23:02 Will Smith         o │ Made some more awesome
2013-06-30 23:01 Will Smith         o │ Added awesome features
2013-06-30 22:54 Aaron Chambers     │ o And now for the bug zapper
2013-06-30 22:53 Aaron Chambers     │ o Getting out the bug spray
2013-06-30 22:53 Aaron Chambers     │ o Swatting bugs
2013-06-30 22:49 Aaron Chambers     I─┘ Test commit
2013-06-30 22:47 Aaron Chambers     I first commit
```

Now with rebase

```
~/code/my-project (master)$ git checkout bug-fix
Switched to branch 'bug-fix'

~/code/my-project (bug-fix)$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: Swatting bugs
Applying: Getting out the bug spray
Applying: And now for the bug zapper
```

```
2013-06-30 22:54 Aaron Chambers     o [bug-fix] And now for the bug zapper
2013-06-30 22:53 Aaron Chambers     o Getting out the bug spray
2013-06-30 22:53 Aaron Chambers     o Swatting bugs
2013-06-30 23:02 Will Smith         o [master] [origin/master] Made some more awesome
2013-06-30 23:01 Will Smith         o Added awesome features
2013-06-30 22:49 Aaron Chambers     o Test commit
2013-06-30 22:47 Aaron Chambers     I first commit
```

Finally, merge the branch back in to master and push

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

```
2013-06-30 22:54 Aaron Chambers     o [master] [origin/master] And now for the bug zapper
2013-06-30 22:53 Aaron Chambers     o Getting out the bug spray
2013-06-30 22:53 Aaron Chambers     o Swatting bugs
2013-06-30 23:02 Will Smith         o Made some more awesome
2013-06-30 23:01 Will Smith         o Added awesome features
2013-06-30 22:49 Aaron Chambers     o Test commit
2013-06-30 22:47 Aaron Chambers     I first commit
```


What does your day to day git workflow look like?

These days I'm pretty happy with how I use git in my development flow so I wanted to share it
for those that haven't yet settled into a git groove.

For me, the key to my workflow is rebasing. Mainly because I no longer need to ugly merge commits in my history, and then as an added bonus,
so I can squish my local commits together into one nice commit.








As I have become more skilled at using git I have often found myself looking for the best way to work git into
my development flow.  Once I actually understood some core git concepts such as how git branches, merges, stores commits etc
I found myself

I'm always looking for ways to improve the way I work.  At the moment though, I am pretty happy with , however, I am now at a point where I am pretty with how my workflow looks. 
