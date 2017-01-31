---
title: '/usr/bin/ruby: invalid option -D ... nuuuuhhhh!'
tags: [ ruby, gem ]
date: 2014-01-23 19:47:10
---

Having issues install your ruby gem?

Well, don't worry, it's not your fault!!
But you are doing something wrong ... confused?

Let me explain.

You probably trying to install a gem via bundle or whatever and getting that responsed back:
```
Gem::Ext::BuildError: ERROR: Failed to build gem native extension.
    /usr/bin/ruby extconf.rb
/usr/bin/ruby: invalid option -D  (-h will show valid options) (RuntimeError)
extconf failed, exit code 1
```
Well relax!! I got that as well, and well ... you might have a "space" in the folder or parent folder name where you're trying to install the gem.

Just remove that space and replace it with an underscore '\_' and magic!

Problem solved!
