---
title: 'Git commands that you might need sometime'
media_order: git.png
published: false
taxonomy:
    tag:
        - git
---

Git is something that most of us use daily, though there are functions / commands you do not use but still may help in some situations.

---

## Search for specific string

We are working on a quite big React project at work and we found a bug in the application - something that we implemented couple of months ago is missing.

I was sure that the feature was there. Also in Jira it was marked as *tested* and *closed* so it had to be tested by our testers. But how to find it when you have hundreds of commits?

The task was about adding a new question to one of our existing forms, so I checked the text of the question and tried to search for it. For this purpose, you can use `git log -S<string>` command, where the `-S<string>` argument looks for the string in commit diffs, so it returns just related commits.

```shell
git log -S "question text"
```

Two commits found - first introduced the feature and second removed it ( for some reason ).