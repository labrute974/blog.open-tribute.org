---
title: Live stats for dd ... everything is possible!
author: Karel Malbroukou
tags:
  - FreeBSD
  - Linux
  - Tips
date: 2013-01-28 22:22:25
---

Good Afternoon!! ... or morning ... depends where you are :P

Well, in computing world, it's always shiny, happy, warm!! So, we don't care of the time ^^
But sometimes, we do care ... especially when you ran a **dd** of Gigabytes of data that is running forever!!

I'll show you how to get stats even when the command is still running ..

You, user of **dd** should certainly know that when this command ends, it gives you some stats output like:
```
15634841+0 records in
15634841+0 records out
8005038592 bytes (8.0 GB) copied, 583.169 s, 13.7 MB/s
```

But the manpage of **dd** states it very clearly. You can get live stats of **dd** sending him a signal: **SIGUSR1**.

What you have to do is to open another terminal, get the PID of the running command, then use:
```
~# kill -USR1 <PID of dd>
```

On the terminal on which your **dd** is running, you should get the stats output without stopping the process.

Hype!
