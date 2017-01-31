---
title: Dell Single bit warnings error...
author: Karel Malbroukou
tags: [ dell, omreport, linux ]
date: 2013-03-01 11:01:28
---

Hi  **penguins-working-on-dell-servers**,

It's been maybe 4 years I promised myself to write a post about that ...
Time flies I guess :D

Working on Dells, I would assume that you all have **OpenManage** installed on your servers.
Have you ever received an alert about DIMMs on Dell servers?

Looking at the logs you get:

``` bash
# omreport chassis
Health
Main System Chassis
SEVERITY     : COMPONENT
Non-Critical : Memory

Ok           : Power Management
Ok           : Processors
[...]
# omreport system alertlog

Alert Log
Alert Log contains...
Severity      : Non-Critical
ID            : 1403
Date and Time : Thu Feb 28 20:36:14 2013
Category      : Instrumentation Service
Description   : Memory device status is non-critical
Memory device location: DIMM_A4
Possible memory module event cause:Single bit warning error rate exceeded
[...]
```

You called straight away the _Dell Call centre_, waited and received an answer like: _"Try this command ... or this command ... have you tried to power it off and on again?_"

Well, I did a while ago! Just don't fire at the Dell techs! It's their job to ask you all those questions, as much as it is your job to get it resolved quickly.

Guess what, unless this becomes a real issue, you don't need to call <u>Dell Call centre</u> anymore.

After all, this is a Non-Critical issue. It basically means that the memory found an error and corrected for it.

So what I would advise, is, calm down, drink some tea, and just clear the memory failures!

On linux, you can clear them with:
``` bash
# /opt/dell/srvadmin/sbin/dcicfg command=clearmemfailures
```
When printing again the health of your server, everything will be OK!!

But, if this happens again, on the same DIMM, in a few weeks time, your DIMM or slot might have real problem.
In this case, open your server and swap memory in this slot with another slot.
For example, sap DIMMs between slot 4 (in this case) and slot 8.

If it happens on slot 8, then you have a bad memory. If it happens again on slot 4, then you slot is bad, so you'll need to change your motherboard ... now you can call Dell!

Hope it helps in your day to day stress :P
