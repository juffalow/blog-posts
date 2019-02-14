---
title: 'Git commands that you might need sometime'
media_order: 'git.png,git-graph.png'
taxonomy:
    tag:
        - git
summary:
    delimiter: '==='
---

Git is something that most of us use daily, though there are functions / commands you do not use but still may help in some situations.

===

## Search for specific string

We are working on a quite big React project at work and we found a bug in the application - something that we implemented couple of months ago is missing.

I was sure that the feature was there. Also in Jira it was marked as *tested* and *closed* so it had to be tested by our testers. But how to find it when you have hundreds of commits?

The task was about adding a new question to one of our existing forms, so I checked the text of the question and tried to search for it. For this purpose, you can use `git log -S<string>` command, where the `-S<string>` argument looks for the string in commit diffs, so it returns just related commits.

```shell
git log -S "question text"
```

Two commits found - first introduced the feature and second removed it ( for some reason ). So I didn't need to go through every commit, just used this command and immediately knew what happend.

## Use remote changes

I am working on two (sometimes tree) computers. I encountered a problem when I did some changes on one computer, created a commit, but did not push. Then I switched computers and I needed those changes, so I did them again, plus couple more. When I came back to the first computer I didn't want to merge those changes, I just wanted to continue on top of those new changes.

One solution is to delete last commit (*n* commits) and pull *master*. But there is another way to fetch the master and use it without merging.

```shell
# fetch the branch (in this case master)
git fetch origin master

# reset your current branch (master) to origin's master
git reset --hard origin/master
```

Now, I have the origin master on my computer and I lost changes I made here before.

## Git branch graphs

Maybe you've already seen some GUI tools for git where is a nice graph with all commits and branches. If you want to achieve this in command line, you can again use the `git log` command.

```shell
git log --all --decorate --oneline --graph
```
![Git graph](git-graph.png)

There are many options to format the graph. So you may want to play with it a little bit.

```shell
git log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all

git log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n''          %C(white)%s%C(reset) %C(dim white)- %an%C(reset)' --all
```
