---
title: vSphere vMotion and NTP ... DONG!
author: Karel Malbroukou
tags: [ vmware, ntp, vmotion, virtualization, time ]
date: 2013-08-23 10:15:53
---

Have you ever had a VMware infrastructure, and almost everytime there's a vMotion happening, the NTP daemon of the guest just goes BOING!?

Here I am, on my white Horse, coming to help!
That's because, the source ESXi host and the destination do not have the same time for some reason.

Ha! I can hear: "Yeah of course!!!" or "we already knew that ... noob.."
Or... you would probably think: "but wait a minute, we disabled **Time Synchronization** between the host and the guest in **vmware-tools**!!!"

I'm sure you did, but you just disabled one of the Synchronization processes.
VMware does sync the guest time with the host time at other time for various reasons.

Basically, an example is your VM being suspended for an hour. When you will restore the machine, VMware will come and say: "This guest has been resumed, his time must be wrong! I'm going to update it!"
Well, you can actually bypass this and let you NTP daemon do that job!

First here are the common reasons why VMWare would sync your guest to the ESXi Host (common because I don't know if there's any other lol):

*   Periodically
*   After taking a snapshot
*   After reverting a snapshot
*   After migrating a guest to another host
*   After resuming a guest from suspend
*   After defrag of a vdisk
*   When the vmware-tools daemon starts up
*   After the host resumes from sleep

To make sure VMware won't touch your guest time anymore, you need to disable all those triggers.

To do so, you need to edit the .vmx of your VM (make sure to shutdown your VM first) and add the following lines:

```
time.synchronize.continue = FALSE
time.synchronize.restore = FALSE
time.synchronize.resume.disk = FALSE
time.synchronize.shrink = FALSE
time.synchronize.tools.startup = FALSE
time.synchronize.resume.host = FALSE
```

Shaazzaaaam!!!
