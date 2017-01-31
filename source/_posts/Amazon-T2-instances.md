---
title: Amazon T2 instances
author: Karel Malbroukou
tags:
  - aws
  - cloud
date: 2014-07-02 23:46:05
---

**Amazon Web Services got a new type of instances: T2!**

This new class replaces the old T1 and M1s.
You gain more performance (thanks to the CPU burst to 3.3Ghz) and more memory.
Find more information on the [AWS blog](https://aws.amazon.com/blogs/aws/low-cost-burstable-ec2-instances/).

But there's a catch!
It's not that simple to move from the old generation to T2!

You know how Amazon runs on Xen, and Xen provides two types of virtualization: HVM (Hardware-assisted Virtualized Machine) and PVM (ParaVirtualized Machine).... right.

**T1/M1 are PVM** and **T2 are HVM**. And guess where is that defined?
In the AMI, which can make some of you smile because it's obvious that it would be there (not being sarcastic here).

You were thinking of migrating to T2?
Think twice and evaluate how much work is that gonna be for you.

The good thing is you can do it, but it's a loooot of work.
You could create a snapshot of the root device of your PV instance and copy the files over to the HVM instance or any other solution you might have...

Unless, and hopefully, you made your Instance installation reproducible, using CloudFormation, or a package that installs all the dependencies or using a config management system (Puppet or Chef), so that you could just change the type of the instance and it's underlying AMI, recreate the instance and the job would be done in minutes!

That's where Amazon wins again, if you used the tools it provides you properly!
