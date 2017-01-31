---
title: Getting the ID of an AMI from its name in each region
author: Karel Malbroukou
tags: [ aws, bash ]
date: 2016-06-06 11:21:43
---
Good morning folks,

It's been years that I've been thinking of doing that script, but never been bothered to be honest :D
But today I got annoyed at it, for the thousands' time, so here I go.

Let's say, you want to create a mappings in **Cloudformation** for the **Amazon Linux AMI**, to be used by your instances.
You will need to specify the **AMI id** in the **Mappings** section, but it's not that easy to get the AMI id in _**every single region**_ ...
You would need to change region and check the ID, plus you would need to know the name of each region ...

So you might go in the AWS Console, and search for the AMI, use the name as filter, and change the region to copy / paste the AMI Id of the Amazon Linux.
Yes... that's what I did... sounds awful? Because it is!!!

So here's a script I'm now going to use everything I need to do this:

``` bash
AMI_NAME="amzn-ami-hvm-2016.03.1.x86_64-gp2"

for i in $(aws ec2 describe-regions --query "Regions[].RegionName" --output text)
do
  echo -n "$i "; AWS_DEFAULT_REGION=$i aws ec2 describe-images --filters "Name=name,Values=${AMI_NAME}" --query "join(',', Images[].ImageId)" --output text
done
```

Boom! Enjoy fellas!
