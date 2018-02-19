---
title: 'Please, do not install every shit'
media_order: npmjs.png
published: false
publish_date: '19-02-2018 15:56'
header_image_file: npmjs.png
---

I am working on React app with couple of other developers. And you know it, you need
this component, that functionality, etc. so you google it, install the package and the work is done.
Is it ok to work like this? Do you even know what exactly you added to your project and do you really need it?

===

I give you an example. There were to tasks related to more or less the same functionality. I had to create
a side bar that could scroll you to the correct section and then there was a button on top of one page that
should scroll you to another section.

#### What I did

I search the internet and read how to scroll user to an element. Of course, lot of solutions uses jQuery
but you can find a pure ( VanillaJS ) solution. Then you can compare those solutions and see the difference.
Pure JS has 35 rows of code ( with comments ). So you copy-paste this solution -> try it -> works -> done!

*When it is something more complex I add link to the source in to comment.*

#### What the other developer did

He did pretty much the same. Search the internet, found scrolling package, installed it and tried it.
It works, task done!

#### Difference?

Now, I really do not want to write about how he used it.

```javascript
componentWillMount() {
  /**This hack only for server side rendering*/
  scrollToComponent = require('react-scroll-to-component');
  /**-----*/

  ...
}
```

I want you to think about it. Is it worth it to install another package rather than 
write ( or copy-paste ) 35 lines of code? BTW that dependency has 2 other dependencies and they
have 4 other dependencies. So maybe you think you just added 1 dependency, but in fact, you added
lot of foreign code. I do not consider this to be good pattern.

What do you think about it? What is your way to do things like this?