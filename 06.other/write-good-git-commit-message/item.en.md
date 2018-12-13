---
title: 'Write good git commit message'
media_order: git.png
date: '21-01-2018 16:33'
taxonomy:
    tag:
        - git
header_image_file: git.png
---

For a long time I didn't know that even writing commit message can have its own "best practices". When I got in touch with git for the first time, this part was described with words like "...and here you can write something short about what's going on in the commit".

===

#### Bad commit messages

Look at the commit messages below. If you want to merge them, you realy don't know, what you are adding / changing, what they do or why you need them. The same applies if you want to search something in the history. You scroll down the log, but it is a mess and a waste of time.

```text
cd3e27a contact page
aee9d0d comments
eac95e5 list of online users, some other changes because of server
fae5636 little edit
```

#### Good commit messages

And now take a look at these messages. Better? I think so.
 
```text
43ec6aa Fix error when the URL is not reachable
4fe84ab Add error message if something went wrong
753aa05 Add server fingerprint check
df3a662 Fix shadow box closing problem
```

## How to write a good commit message

The whole message should have its format - subject, body and optionally conclusion consisting of resolved / closed issues.

#### Subject

The git commit help page described it very good as *a single short (less than 50 character) line summarizing the change, followed by a blank line*. The subject should start with a capital letter and should not end with dot. And the important thing here is, it has to be in imperative form. [Chris Beams](https://twitter.com/cbeams) wrote a simple rule to get it right every time :

A properly formed Git commit subject line should always be able to complete the following sentence: if applied, this commit will *your subject line here*. For example :

* if applied, this commit will *Delete unnecessary files*
* if applied, this commit will *Add grep option*
* if applied, this commit will *Fix error when protocol is missing*

It will not work for bad commit messages :

* if applied, this commit will *contact page*
* if applied, this commit will *list of online users, some other changes because of server*

The git itself is using this approach. When you merge something it generates a commit message like "Merge branch...", or when reverting "Revert...".

#### Body

Here you write what and why is changed. The body should not exceed 72 characters for a line. Of course not every commit has to have body.

#### Bottom line

On the end, you can add which issue does the commit fix or is related to. This can be a link, number or if you use GitHub you can write it as *Resolves #N* / *Closes #N*, where *N* is the issue ID.

#### Example

This is an example commit from one of my repositories :

```text
Fix error when protocol is missing

First, it checks if the protocol is set. If not, it changes the url and
add the basic http protocol on the beginning.
Second, it does a "preflight" request and follows all redirects and
returns the last URL. The process then continues with this URL.

Resolves #17
```

## Conclusion

Thank you for reading, I hope you learned something new. If you have another tip(s) how to write better commit messages, or how to better use this tool, please leave a comment.

#### Generate a changelog

Another advantage of such commits is it is really easy to generate changelog.

```
# show whole commit history
$ git log --oneline --decorate --color

# show history from one tag to another
$ git log 0.0.9..0.0.10 --oneline --decorate --color

# show history from tag to head
git log 0.0.9..HEAD --oneline --decorate --color
```

The result of such command is the list of commits.

```text
21629ee Fix getResources bug
aa13384 Update docs
44de44c Add HSTS check
```

#### Commitizen

There is a command line tool available on GitHub named [Commitizen](https://github.com/commitizen/cz-cli) which makes it a little bit easier. When you want to commit, you just type <kbd>git cz</kbd> and it asks you couple of questions and then creates the commit with proper message for you.

#### References

[Git commit](https://git-scm.com/docs/git-commit) manpage

[Closing issues using keywords](https://help.github.com/articles/closing-issues-using-keywords/) on GitHub

[How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/) post from Chris Beams

[Angular Commit Message Format](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines)
