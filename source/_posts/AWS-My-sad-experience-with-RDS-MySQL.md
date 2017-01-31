---
title: 'AWS: My sad experience with RDS MySQL'
author: Karel Malbroukou
tags: [ AWS, RDS, Database ]
date: 2014-05-13 19:53:28
---

Hello folks,

Welcome to my blog ... for those who never came before.. Probably you!
If you're here, it's probably because you're as sad as I am.

I'm going to share my quick RDS experience with you today.

At work, we're working on getting a new RDS instance ready for a datacenter migration (from physical datacenter to AWS woot!).

It's not happening as easily as I first thought (it never does!).

Anyway, today I crossed path with the sad man inside me for 3 things and you may want to know:
  - RDS Snapshot restore
  - Cloudformation Fail
  - RDS-as-a-Slave snapshots

## RDS Snapshot restore

**Do not** think of RDS snapshot restore as a normal **mysqldump** restore!
It's a snapshot of the RDS volume, just like you would do an EBS volume snapshot.

And you can't restore an EBS snapshot into the volume can you? ... No.
You can only create a new volume from an EBS snapshot, that makes sense right?

Well it's the same principle for RDS snapshot! You cannot restore an RDS snapshot into the same instance, you would have to create a new one and specify the snapshot that you want to restore.

There's a few things that you need to know there. That means, if you have a Cloudformation stack to create your RDS instance, don't think RDS snapshot will be enough to be able to easily restore the data of yesterday (if you ended up deleting the main table for example).
Quite unfortunate, isn't it.. You might want to keep a mysqldump running from somewhere.
Well, you could still delete your current instance, and restore into an instance with exactly the same identifier you would say...

Also, if you created a snapshot from an RDS instance that has Iops enabled, make sure you restore into an RDS instance that has Iops enabled otherwise, your cloudformation stack would fail.
Some of you probably think: "Well I did it through the AWS Console though".
Mwahaha lies I tell ya! Go back in the AWS Console and check your RDS instance, I bet it has exactly the same Iops setup as the RDS instance from which the snapshot was taken.

## CloudFormation fail

Sometimes, for whatever possible reasons, your CloudFormation stack might fail when updating a huge database... and it will end in the state: **UPDATE_ROLLBACK_FAILED**.

Well ... go put your gloves on, get a sandbag and hit it until you bleed, because you would need it if that happens on your production database... there's no way around it! You would have to recreate your stack!

Update: Actually there is a way around it, be nice and call AWS support.. and ask for the Cloudformation tech peeps!

## RDS-as-a-slave snapshot**

Let's say now that you created an RDS instance and is a replica of another MySQL instance somewhere because you might be migrating your application from one datacenter to AWS.

Well, note that if you want to take a snapshot of the RDS, you might want to stop the replication (```call mysql.rds_stop_replication;```) and do a ```SHOW SLAVE STATUS \G``` to get all the master information before taking the snapshot.

Why? I'm glad you ask! Even though those information will be saved in the snapshot, as it's information that is written to the disk, when you restore a snapshot into a new RDS instance, it will reset the slave on the instance!

Yeah ... I've fired a feature request. Let's see what happens!

I hope it was informative, and good luck guys!

On the bright side, I still think  AWS techs did a great a job on it, they're still on a big journey, don't shoot them! We need Cloud providers like them!
