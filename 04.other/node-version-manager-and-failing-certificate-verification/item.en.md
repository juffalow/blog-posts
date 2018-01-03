---
title: 'Node Version Manager and failing certificate verification'
media_order: node-js.png
date: '07-11-2017 19:18'
taxonomy:
    tag:
        - Node.js
header_image_file: node-js.png
header_image_width: 128
header_image_height: 78
---

In my current job, we have some kind of network settings ( or better proxy settings ) that from time to time cause problems with certificate verification.

===

#### nvm ls-remote

I used nvm to install node and npm on my computer. Few days ago I needed to change my node version to newer one. It is very easy with nvm, you just list all versions and choose which one you want. So I typed…

```
$ nvm ls-remote
```

This command should list all available versions of node. But all I got was :

```
N/A
```

The problem in this case is with certificate verification. Nvm doesn’t have any option to disable this, but when you read the docs carefully, there is option to set `$NVM_NODEJS_ORG_MIRROR` variable. So you can do the following :

```
$ export NVM_NODEJS_ORG_MIRROR=http://nodejs.org/dist
```

And now everything works, because there is no need to check certificate when it is only http.
The <kbd>nvm ls-remote</kbd> command outputs :

```
v0.1.14
v0.1.15
v0.1.16
...
v8.8.1
v8.9.0   (Latest LTS: Carbon)
v9.0.0
```

And then you can install any version you want :

```
$ nvm install <version>
```

## References

[GitHub – nvm](https://github.com/creationix/nvm#listing-versions)