---
title: 'How to use git bisect'
date: '12-05-2016 00:48'
taxonomy:
    tag:
        - git
---

You know something worked properly a few days ago and now it is broken? Or maybe there is a new feature and you don’t know when it was added and what was changed? You can use this command and search in history of git. It is easy and fast.

===

This will be very short post. I haven’t use this command before and nowadays I was reading something about git where this command was used. I do not consider myself as an expert in git. There are still many features I am not using or I don’t know about. So I googled it…

## Commit that introduced a bug

The first result on google is the git docs page, where you can read :

> git-bisect – Use binary search to find the commit that introduced a bug
> - [git](https://git-scm.com/docs/git-bisect)

What does this mean? I use simple example but it can be extended and used to search any change in the source code ( not only bugs ). So to demonstrate it, I create a bash script and commit after each line.

#### Initialize git repo

```
cd ~/Documents
mkdir bisecttest
cd bisecttest

touch script.sh
chmod +x script.sh

git init
git add .
git commit -m "initial commit"
```

#### Good and working part

Open the file *script.sh* in your favorite editor and add these lines :

```
#!/bin/bash
echo "1 : ok"
```

And commit. ( <kbd>git add . && git commit -m "number 1 commit"</kbd> ). Append new line `echo "2 : ok"` and commit again. Repeat this 3 times. You end up with this :

```
#!/bin/bash
echo "1 : ok" # number 1 commit
echo "2 : ok" # number 2 commit
echo "3 : ok" # number 3 commit
echo "4 : ok" # number 4 commit
echo "5 : ok" # number 5 commit
```

Run the script and it should write numbers from 1 to 5 on the screen.

#### Bad and not working part

Now, create the error ( buggy ) part somewhere in around the middle of the script, lets say between `echo "3 : ok"` and `echo "4 : ok"`. In the script it means, exit with an error code number. E.g.:

```
..
echo "3 : ok"
exit 1
echo "4 : ok"
...
```

In real world you know this would screw up the whole thing, but now you are happy to commit new line ( <kbd>git add . && git commit -m "error commit"</kbd> ).

#### Again some good part

You added bug and haven’t test the whole app. You continue like everything is working properly.

```
echo "6 : ok" # number 6 commit
echo "7 : ok" # number 7 commit
echo "8 : ok" # number 8 commit
```

#### Now you realize something is wrong

You run the script, it should write numbers from 1 to 8 on the output, but stops on the number 3. Why? Where did I ( or whoever ) make a mistake? You can try and search in code or use <kbd>git bisect</kbd>!

You know it was working 5 – 6 commits ago and now it is not ( this is very important ). You run command :

```
git bisect start
```

Git switch itself into a “debug mode”. Mark current version as bad :

```
git bisect bad
```

Checkout working commit / tag and mark it as good ( 6 commits ago is `number 3 commit` ) :

```
git checkout HEAD~6

git bisect good
```

Now, git knows which version is bad, where in history it was good and begin to search. How? It automatically ( after you enter one bad and one good commit ) checkouts a commit in the middle of that range of history. You run the script and see if it is good or bad.

<div class="alert alert-info" role="alert">This is easy, but remember, it can be whatever. If it is a webpage, reload it and check if the bug is still there. If it is an Android app, compile it, install and try it.</div>

After you typed that command, git jumped in the middle of this commit and the first good commit. Therefore the position is on `error commit`. In that commit we introduced the bug which means it is a bad one.

```
git bisect bad
```

There remains only one commit to check – `number 5 commit` – that is good.

```
git bisect good
```

Now, it is obvious that the error occurred in `error commit`. Git shows message like this :

```
bf9422a7dcc52bf84773ee4c3e22775750009294 is the first bad commit
commit bf9422a7dcc52bf84773ee4c3e22775750009294
Author: Matej Jellus <juffalow@juffalow.com>
Date:   Thu May 12 23:20:45 2016 +0200

    error commit

:100755 100755 6477afa22ed745e211df5a9d575f674ed890472f 5588800457f0a7778fc3c4892fec7fba62ef1b46 M	script.sh
```

...and the search is over. If you are curious what exactly it was type git show <kbd>&lt;commit hash&gt;</kbd>

```
commit bf9422a7dcc52bf84773ee4c3e22775750009294
Author: Matej Jellus <juffalow@juffalow.com>
Date:   Fri May 13 09:16:20 2016 +0200

    error commit

diff --git a/script.sh b/script.sh
index 6477afa..5588800 100755
--- a/script.sh
+++ b/script.sh
@@ -2,5 +2,7 @@
 echo "1 : ok"
 echo "2 : ok"
 echo "3 : ok"
+exit 1
 echo "4 : ok"
 echo "5 : ok"
```

To return back to original HEAD and end the debug mode type <kbd>git bisect reset</kbd>.

#### Automatically test and find bad commit

If you are able to write a test for your search you can speed this operations up. This example servers as a test at the same time.

```
git bisect start
# mark current commit as bad
git bisect bad
# go to history and checkout commit that is good
git checkout HEAD~6
# mark it as good
git bisect good
# and now let the git test it for you...
git bisect run ./script.sh
```
