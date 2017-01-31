---
title: Mailman with abuse mailing list ... not receiving
author: Karel Malbroukou
tags:
  - Linux
  - list
  - mailman
  - postfix
  - Python
  - Tips
date: 2013-01-28 22:22:25
---

Hi fellows,

Today I bumped onto an issue. On his mail server, my friend got Postfix and Mailman to manage mailing lists.

The problem is that he has a mailing list called *Abuse*, and he can't seem to receive mails to it. When i checked the postfix logs, I saw that a mail sent to the list Abuse was finally redirected to postmaster@localhost... Why that ?!

When looking inside mailman logs, i didn't find any clue... no error messages, but no delivery success to the list.

After a few minutes, i finally looked inside the script _postfix-to-mailman.py_ and I saw that this script was doing something weird ... It redirects all mails that arrives on Mailman for  **postmaster**, **abuse** and **mailer-daemon** to postmaster@localhost by default!

That doesn't do any good if you want to define a list in Mailman with any of those names...

What we did to fix the issue, was just to modify the line (116 approx.):
```
# Redirect required addresses to
if local in ('postmaster', 'abuse', 'mailer-daemon'):
```
with
```
# Redirect required addresses to
if local in ('postmaster', 'mailer-daemon'):
```

And now, everything is working fine! If you get the same issue, try to check that first, and if not ... then good luck :D
