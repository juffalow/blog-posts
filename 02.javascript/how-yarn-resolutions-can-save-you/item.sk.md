---
title: 'How Yarn resolutions can save you'
media_order: yarn.png
published: false
header_image_file: yarn.png
---

Have you ever heard about yarn resolutions? It is not something you would use everyday, but it is definitely useful.

===

## I had a problem

Yesterday I commited a piece of code, pushed it and after couple of minutes, the test env did not change. Problem was with jenkins build and a new package I added. That package is using a dependency with specific version which had known issue that caused problems from time to time. The issue was solved in a new version but package I used is using the old (buggy) version.

So I looked on GitHub and the package wasn't updated for a couple of months. This is a [problem](https://juffalow.com/other/the-open-source-problem) I wrote about in the past. Luckily this wasn't an error with the code, but with a dependency version defined in *package.json*.

#### Selective dependency resolutions

Yarn allowes you to override package versions inside your dependencies. So now it is easy to solve this kind of problem.

```
  "resolutions": {
    "<package>/**/<dependency>": "<new version>"
  }
```

This tells yarn, that the *package* is using its *dependency* with *new version* and not with the version defined in its *package.json*.

You can read more about it in the [docs](https://yarnpkg.com/lang/en/docs/selective-version-resolutions/).
