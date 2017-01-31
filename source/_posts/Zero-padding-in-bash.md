---
title: Zero padding in bash
author: Karel Malbroukou
tags:
  - FreeBSD
  - Linux
  - Tips
date: 2013-01-28 22:22:25
---

Hi folks,

I have been asked how to do zero padding in **bash** ... as C language does it with _printf_, **Bash** also offers a _printf_ command that has almost the same syntax as the one in the C language...

_printf_ is used to format the display of some text.

If you need to use a zero padding in Bash, you can use as followed:
```
~# printf "%03d\n" 15
015
```

This will watch the number (5 in this example) and calculate how many zero need to be added at the beginning so that we have 3 numeric characters displayed.

Another example would be:
```
~# printf "%03d\n" 1232
1232
```

The %d specifies that we want to display a number, then between the percent and the letter d, we can specify the format in which the number will be displayed. Here we force the display to print out 3 numeric characters minimum.

*   If the number we specify has less than 3 numeric characters, zeros will be added at the beginning of it so that is displays 3 numeric characters.
*   If the number we specify has 3 numeric characters or more, _printf_ will just print the number.

Hope this could help ;)
