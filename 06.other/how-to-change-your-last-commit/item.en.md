---
title: 'How to change your last commit'
media_order: git.png
publish_date: '06-03-2018 15:20'
taxonomy:
    tag:
        - git
header_image_file: git.png
---

You commited something and right after that you saw you forgot to remove log or add another file?
Or you just want to extend your message? It is not necessary to revert the commit everytime.

===

#### How to add file to the last commit

You did some changes, added files and commited. Then you saw you forgot to add another file. What to do?

```shell
git add <the file>
git commit --amend --no-edit
```

The `--no-edit` option just uses the original commit message without changing it or without launching an editor.

#### How to change commit message for the last commit

You commited something and you forgot to add an important note into the commit message? Or you want to add issue number? No problem...

```shell
git commit --amend
```

It will open your editor and let you change the message. Of course you can use `-m` option:

```shell
git commit --amend -m "commit message"
```

#### Warning

Do not use ammend if you have already pushed the commit to the repo and someone pulled it. This could, or better - probably will, cause problems.