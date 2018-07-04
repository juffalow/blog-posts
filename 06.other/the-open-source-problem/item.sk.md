---
title: 'The Open Source problem'
media_order: source-code.png
published: false
header_image_file: source-code.png
---

It was a great idea to use git and create portal like GitHub and share source code among other developers. One of the benefits is, you do not need to reinvent the wheel and just use package you need. And because lot of users will add it to their projects, it gets well tested. But I can see there a big problem: maintenance.

## Maintenance

You find some code, it looks perfect, hundreds / thousands of stars, it has couple of issues, but what doesn't and you use it. Some time later you find out there is a bug...

#### Local fix

I think, this is the most used solution. Just copy everything into the main project, solve the problem with the packge and use this local version. The pros is it is working. But it has more cons - you do not know if there is not a better solution and every other team / developer has to find out his own solution.

#### Pull request

This is the biggest problem what I can see here: Yes, you solved the issue. You created pull request, therefore shared your solution. If there is some better workaround, someone can propose it. You learn something and no one has to solve it again. All this is good. The bad part is there are plenty of pull requests that are still waiting to be reviewed / approved. If the project is older, then the maintainer is not paying that much attention to that project anymore.

Now, even when you have good solution, you are not able to use it without moving the whole package under your project. Or create another package from this new source. Isn't it the good old "reinventing the wheel" thing again?