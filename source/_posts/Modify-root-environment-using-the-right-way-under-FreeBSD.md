---
title: Modify root environment using the right way under FreeBSD
author: Karel Malbroukou
tags:
  - FreeBSD
  - root
  - shell
date: 2013-01-28 22:22:25
---

Hello fellows!

Today, after a few months off ... I'm back with a few tips that you would definitely love - or not.
Here, I'll explain how to modify the root environment using the proper way... What an easy task, and would you know how?

For example, **/bin/csh** is the default shell for root, after installing your FreeBSD.
But, maybe, some of you won't like that shell, and would rather use **bash**, so you installed it, and you want to modify the root settings so that when you log in, it will use **bash**.

Most of us will use the following command:
```
bsd # pw user mod -n root -s /usr/local/bin/bash
```

This will certainly work, but what if you boot your system in **single mode**?
If you did install your OS properly, you would have mounted / on a partition, and /usr on another one.

We all know that in **single mode**, the partitions are not mounted, and when the system will boot, it will error out with: "/usr/local/bin/bash" not found, or whatever ... i actually don't remember the error message :D

> I could try again and boot on single mode to find out the real message but I'm wayyy too lazy loool.

So you will have to choose another available shell... soooo boring...

So here comes the magic of BSD!!! Watch carefully **/etc/passwd**:
``` bash
~# cat /etc/passwd | awk -F ':' '$3 == 0 {print}'
root:*:0:0:Admin Tribute:/root:/bin/csh
toor:*:0:0:Bourne-again Superuser:/root:
```

**toor** also has the UID 0 ! And yeah, forget about **root**!
We always say that **root** is evil, then use **toor** to make the shell modification and log in with it!!
`~# pw user mod -n toor -s /usr/local/bin/bash
`

Well ... **toor** is as evil as **root** but shhhh!!! Don't tell my followers :P
