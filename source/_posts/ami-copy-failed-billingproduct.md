layout: posts
title: Cannot copy AMI to another account due to BillingProduct code .. say what?
date: 2017-04-18 17:49:07
tags: [ AMI, AWS, Windows ]
---

*Note that I'm using Windows AMIs out of ... obligation?*

*tl;dr Because it's a Windows AMI, you would need to first spin up a new instance from that shared AMI, then create a new AMI in the destination account from that instance.*

So like me ... you've been using Windows Amazon Machine Images.

And you're looking at copying your freshly built image across to another AWS account, but the copy is failing with:

    Images with EC2 BillingProduct codes cannot be copied to another AWS account.

Well .. you're using Windows, you're on your own :P

Nah alright, I'll help you out here.

Basically that message means that you're trying to copy a Paid AMI from the marketplace or a Windows AMI!


That's good to know .. but how do you create an AMI for those bad boys then?

Well, you can still launch an instance from that shared AMI then create an AMI from that EC2 Instance (remember, if you're using Windows, make sure you [sysprep](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ami-create-standard.html) the machine).


Now you might be asking... 

> Can't I just share a snapshot of the EBS volume of the origin instance to the other account, copy that snapshot and create an AMI out of it?

Well... yes you can. For Windows AMI though, you will have a tough time!

You sure can do this, but when you will create an image from the snapshot, the AMI platform will show **Other Linux** rather than **Windows**!!!

According to Amazon documentation (as of 2017-04-17), you can only create a Windows AMI out of an actual EC2 Instance!



Because I love pushing boundaries, I went further than this, and actually tried to spin up an EC2 Instance from that **Other Linux** AMI.

The instance booted!!! And although I don't get **System Log** and **Get Windows password** feature due to AWS thinking it's a Linux machine, I've been able to RDP into the instance using the origin instance windows password!

Cherry on the cake, as AWS treats that instance as a Linux instance, billing is done according to that!


Now.. of course, I do not recommend you doing this, nor do I know what this means for the Windows licensing etc.

So don't do it at home!

The End.
