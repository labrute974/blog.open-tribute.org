---
title: 'First glance at Yarn: a new javascript package / dependency manager'
author: Karel Malbroukou
tags: [ javascript, yarn, node ]
date: 2016-10-17 12:01:08
---

Being subscribed to the [Javascript Weekly newsletter](http://javascriptweekly.com/), I received an email last week about a new node package manager: Yarn.

Built by Facebook, and knowing how good the tools they build are, I decided to give it a go.
To be honest, NPM slowness was really starting to frustrate me..

I won't cover how to migrate from NPM to Yarn as it's been covered by the [Yarn docs](https://yarnpkg.com/en/docs/migrating-from-npm) already.

So, I did a quick test this morning and timed it.

Running yarn install vs. npm install for the first time today got me these results:

``` bash
$ time yarn install
[...]
real    0m32.301s
user    0m15.931s
sys     0m8.744s

~/application/ $ time npm install
[...]
real    0m44.660s
user    0m19.924s
sys     0m8.410s
```
Quite a significant difference already hey!? More than 12 seconds faster!

Then I went on and run those commands a second time:

``` bash
~/application/ $ time yarn install
[...]
real    0m1.468s
user    0m0.589s
sys     0m0.144s
~/application/ $ time npm install
[...]
real    0m6.274s
user    0m4.210s
sys     0m0.663s
```
Much much faster!

Now, I often use the help, as being old (yeah ... right ...), I do sometimes forget the other subcommands that I don't often use, here are the timings:

``` bash
~/application/ $ time yarn install
[...]
real    0m0.991s
user    0m0.424s
sys     0m0.095s
~/application/ $ time npm help
[...]
real    0m2.199s
user    0m0.808s
sys     0m0.178s
```

_**Convinced yet?**_

One caveat is that Yarn (v0.15.1) doesn't seem to include a search subcommand like NPM does.
