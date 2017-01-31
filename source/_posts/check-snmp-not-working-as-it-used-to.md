---
title: check_snmp not working as it used to...
author: Karel Malbroukou
tags:
  - Linux
date: 2013-01-28 22:22:25
---

Hi All,

**Another post to announce something big!!!**

So my example is about **check_snmp** on a [Citrix Netscaler](http://www.citrix.com/products/netscaler-application-delivery-controller/overview.html) ... but the issue is valid for any OID that returns a string.

Basically, sometimes between version 1.4.13 and .1.4.15 of the nagios-plugins package (that contains **check_snmp**) **check_snmp** string parsing stopped working properly.

I was having troubles with our snmp checks when checking for **"Time of last state transition"** on the netscaler.
Verified other snmp checks against the netscaler and they were just fine.
So it wasn't anything to do with the netscaler.

Ran the command on version 1.4.16 of check_snmp and got this output:
```
/usr/lib/nagios/plugins/check_snmp -H -o NS-ROOT-MIB::haTimeofLastStateTransition.0 -C
SNMP OK - Timeticks: (1304917500) 151 days, 0:46:15.00 |
```

Where if ran the exact same command on version 1.4.12 I had this output:
```
SNMP OK - Timeticks: (1304922200) 151 days, 0:47:02.00 | NS-ROOT-MIB::haTimeofLastStateTransition.0=Timeticks: (1304922200) 151 days, 0:47:02.00
```
**That's a first difference between the version!**

But that was the least of my concerns.

What I was concerned about was that when trying to use the critical threshold flag it wouldn't work at all!!
```
/usr/lib64/nagios/plugins/check_snmp -H -o NS-ROOT-MIB::haTimeofLastStateTransition.0 -C -c 86400:0
Range format incorrect
```
WTF!?! HAAAAAA

Well, the fix was actually easy (after banging my head against the laptop a few times)!
To trigger critical for anything under a certain value, the syntax has changed is now: :(nothing here, no zero!)
Example:
```
/usr/lib64/nagios/plugins/check_snmp -H -o NS-ROOT-MIB::haTimeofLastStateTransition.0 -C -c 86400:
SNMP OK - 1304963700 | NS-ROOT-MIB::haTimeofLastStateTransition.0=1304963700
```

And look!!! Even the output is fixed!!

Weird isn't it?
Didn't look too much into it, but feel free to and comment this post with what you found!
