---
title: APC 7920 Lost Admin Password!!!
author: Karel Malbroukou
tags:
  - apc
  - pdu
  - Technologies
date: 2013-01-28 22:22:25
---

Argh!!! We lost the admin password of our APC (PDU)...
First thing that a normal and respectable system or network administrator would do is to read the manual to see if we can find a section: "Lost Password Recovery".

Happily, you will find it ... but quickly you will sink into deep abyss ...

Why do I say that? It's because the manual will say: "Connect to the APC via the Serial connector... bla bla bla ... and then, press the reset button and try to login again  with the default admin user / pass:   apc / apc, within the next 30 seconds."

You could be as lucky as you want, it won't work!

So here are the real instructions:
  1. Connect via Serial to the APC (use either **HyperTerminal** on _Windows_ or **minicom** on _Linux_)
  2. Press the Reset button on the back of the APC during 10 seconds
  3. Release it, and press immediately a second time for just a second
  4. Now, go back on your terminal, and try to log as _apc_ / _apc_

It should just magically work, and guess what? Your APC configuration that you may have done previously has not been changed.
Just the admin password has been reset. Isn't it great? ^^
