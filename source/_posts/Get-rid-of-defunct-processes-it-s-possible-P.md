---
title: 'Get rid of defunct processes ... it''s possible :P'
author: Karel Malbroukou
tags:
  - defunct
  - FreeBSD
  - kill
  - Linux
  - processes
  - Tips
  - zombies
date: 2013-01-28 22:22:25
---

Hi guys,

Have you ever been in the situation of getting on your nerves because you can't kill a process ??
I did not :P But I was just wondering how to get rid of them.

I'll teach you how to do it in this tip!

First of all. Do you know what is a defunct process?
Under Unix, we call those kind of processes: **Zombies**.

We call them **zombies** because they are already dead processes, so we can't kill them.
Note that **zombies** are not really processes. They are just entries inside the processes' table, that has not been cleaned up.

This happens when a parent process did not wait for his child to exit.

To get rid of them, you can just find the process parent of that process, kill it so that the Zombie gets a new parent, which happens to be **init**, and the latter will clean up its entry in the processes' table.

To find out those defunct process with their parent PID, use:
``` bash
~# ps -f ax  | sed '1p;/d[e]funct/!d'
UID        PID  PPID  C STIME TTY      STAT   TIME CMD
root       339   333  0 Apr05 ?        Z      0:00 [sh] <defunct>
```

Then you can use the PPID of the process to check all its children by using:
``` bash
~# ps --ppid 333
  PID TTY          TIME CMD
  339 ?        00:00:00 sh <defunct>
```

If you don't see any processes which you might want to tinker with, you can just kill the parent:
``` bash
~# kill 333
~# ps -f | sed '1p;/d[e]funct/!d'
UID        PID  PPID  C STIME TTY          TIME CMD
```

Finished! Comments are welcome ;)
