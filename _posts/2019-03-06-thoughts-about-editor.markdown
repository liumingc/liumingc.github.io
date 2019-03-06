---
layout: post
title:  "Thoughts about editor"
date:   2019-03-06 21:31:50 +0800
categories: jekyll update
---

A modern editor should have the following features:
- multi tab & multi file, fast select & switch between buffers
- popup window to show hint/preview
- macro, and code snippets
- folding & syntax highlight
- source sensitive fast navigation
- locate some definition & refs

The last 3 features, requires some kinds of parsing, which can be hard. For
example, folding sometimes is done by defining some sort of keywords. But in
sml, `fun foo bar = ...`, there is no end keyword, so you need to parse an expr
to do the fodling. To fold a function, to delete a function, to skip to
next/prev function, get a decl outline, all these things need parsing.

I'm using VSCode, vim, PyCharm, jEdit now. I'm familiar with vim, but not with
vimL. jEdit seems nice, but not quite familiar, it use beanshell to extend it's
flexibility. VSCode's plugin can be heavy, so is PyCharm. And PyCharm is IDE, so
it may be not good at editing not python files.

And when you have multi modes, and many keyboard shortcuts, then them can come
to conflict, there should be a nice way to manager this.

Maybe vim + vimL/python is really a good choice.

What kind of plugins do you wanna write then?
