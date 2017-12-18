---
title: 'Two screwed up days'
media_order: yarn.png
header_image_file: yarn.png
header_image_width: 128
header_image_height: 128
---

Little story about my two hard and nervous days. Problems with NPM, fighting with Node and Linux, reading about Yarn. Wrote it down while sitting in front of the computer and thinking about nothing and everything at the same time, i.e. relaxing.

===

Maybe 2 months ago I inherited one “pure HTML” project. To be clear, it is many HTML parts and one big Grunt task, that builds everythings together. Cool, but…

## NPM package manager

 edited a few things, add something and then it came. I called <kbd>grunt build</kbd>. ( Of course I installed everything with <kbd>npm install</kbd> before. ) Local Npm module *** not found. So somehow, the previous developer has it there whithout having it in *package.json*. I installed couple modules, no problem.

#### Jenkins job

Build on my machine was successful. I moved everything on the server. Done. I decided to create a Jenkins job for this ( need to copy several directories to various directories ). This should go fast, I created a few jobs before, therefore this is only “copy & paste”.

Job is running…and few moments later, *Error*. NPM cannot istall on sub-module. I checked Node and NPM version on my local machine with server where Jenkins job is running. Everything is ok and versions are the same.

#### Not working solution

Let’s try to remove everything, <kbd>rm -Rf ./node_modules</kbd> and <kbd>rm -Rf $HOME/.npm</kbd>. Run <kbd>npm install</kbd> directly from terminal. And voila, it’s working. At least, I thought so. I run the job again and it failed. Wtf? I was getting angry.

The day continued and I was trying nearly everything. Changed the module version, remove everything, installed it back, and so on. Nothnig helped. I went back and read the output of the working attemmpt.

#### Postinstall script

The only difference was, that in that case, NPM run postinstall script. It did the same on my computer. I gave it another chance. During one experiment it run it again. Unfortunately I realy don’t know why and when it runs that command / script and how to force it to do that.

Realy angry I wanted some break. Bedminton should be ok.

## Yarn

Next day, full of will, I came to work. I remembered, Facebook published its own package manager – [Yarn](https://yarnpkg.com/). Back to the beginning.

Executing <kbd>yarn</kbd> and waiting for the response. The output is different to NPM, but it was doing something in the end, probably the “post script”. So I run it again to see if it does it always or it was coincidence. Coincidence.

#### Solution

I had to gave up. That module is corrupted and I don’t know how to fix it. Next step : open `Gruntfile.js` and see what is going on. Guess what? The main option for that module was missing. So practically it was doing nothing. Yes!

## Conclusion

You may think this is pointless and the whole post is just rubbish. But read on…

When it was the lucky time and the build was successful with NPM, it tooks 28 seconds. With Yarn, it fell down to 18 seconds. It is obvious, because Yarn has golbal cache directory. It doesn’t have to download anything more than once. For this reason, you can use it offline either. And I like the idea of lockfile it generates after install, to ensure that every install results in the exact same file structure in *node_modules*.

This is the end of the sadly happy story. Thanks for the reading.

#### References

[Yarn: A new package manager for JavaScript](https://code.facebook.com/posts/1840075619545360) post, which was on Hacker News, where I originally read it.
