+++
title= "Mark"
date= "2022-05-15T18:33:47+08:00"
# draft: true
+++

## The archtypes in Hugo
When I was pushing blogs use hugo themes, I found a strange thing: I cannot go the the subdirectory of my blog, that's very wired. 

Then after using the command `hugo server` to run my blog locally, I found that my archtypes are wrong.

The archtype is like a template when you use `hugo new` to write new blog, it will generate a new `.md` file. When you open the file, you'll find that this file is not a empty file, you will get some information on this file if you configure your archtypes file.

Traditionally, there're two types of archtype file.
* This is a correct way.
```
---
author: Hugo Authors
title: Math Typesetting
date: 2019-03-08
description: A brief guide to setup KaTeX
math: true
---
```

* This is another way.
```
+++
author = "Hugo Authors"
title = "Markdown Syntax Guide"
date = "2019-03-11"
description = "Sample article showcasing basic Markdown syntax and formatting for HTML elements."
tags = [
    "markdown",
    "css",
    "html",
]
categories = [
    "themes",
    "syntax",
]
series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]
+++
```