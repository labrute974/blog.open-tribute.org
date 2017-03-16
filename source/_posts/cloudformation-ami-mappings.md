layout: posts
title: Finding Amazon Linux AMI IDs for Cloudformation Mappings
date: 2017-03-16 16:24:27
tags:
---

So ... this post came about because I use [Cloudformation](https://aws.amazon.com/documentation/cloudformation/) for all my infrastructure in AWS.

When you create an EC2 instance, you need a base AMI, and for all my tests I usually just use the Amazon Linux AMI.

Now, the issue here is that an AMI is region-bound, so the Amazon Linux AMI that Amazon provides has different IDs in each AWS region.. so usually I would use a [Mappings](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/mappings-section-structure.html) configuration in Cloudformation to specify the right **Amazon Linux AMI ID** according to the region I'm playing in.

Now, the name of the AMI is the same in all region! Thanks AWS for consistency!
Plus, the AMI name is unique *per region*!

So let me tell you my evolution on finding the AMIs for each region.

## 1. Find a cloudformation template that uses Cloudformation Mappings for Amazon Linux AMI

That's years ago! Don't judge me :D

So here, I would first go on Google, and try to find a Cloudformation sample that uses Cloudformation Mappings, then copy / paste the Mappings section.

**Pros**
uhh... None?

**Cons**
Well, first of all, very manual.. right.

Also, Amazon delivers new AMI with new updates all the time, but the Cloudformation stack might not have the latest AMI referenced.

I have never thought I would need again so I didn't bother bookmarking the link ... guess what..
I've always needed it..

## 2. Find the AMI IDs from the console

See how I finally decided to use an AWS tool rather than Google to find the information? Winner!!

I would go in the AWS console, find the AMI I want, and filter in the AMI console on the name then change each region and filtering on the same AMI name.. and copy / paste was my best friend. Ever.

**Pros**
 * I could see if there was a new Amazon Linux AMI.

**Cons**
 * Long, painful ... my life sucks kind of way.

## 3. Using AND the console AND the awscli

Then I decided.. let's automate this to some level, so I wrote a script that would loop through each region and get the image id based on the AMI name.
Here I would first go in the Console, same as before, and get the AMI name.

*Disclaimer: For arguments sake, I didn't include all the regions here, but there's an easier way later anyway!*

After that, I would run the following script:

    for region in ap-southeast-2 us-east-1 us-west-2
    do
      echo -n "${region}: "
      aws ec2 describe-images --filters Name=name,Values=<ami_name> --query "Images[0].ImageId" --region $region
    done 

**--query** might sound obscure to you..
Well, this is a jmespath query, which you can [read more about here](http://jmespath.org/).

## 4. Step into the future. AWSCLI only solution!

Now now... I'm better than this hey!
Well, awscli has a way to list all the regions.. and I finally decided that I had enough with my lazyness, and automate the whole shizzel:

    for region in $(aws ec2 describe-regions --query "Regions[].RegionName" --output text)
    do
      echo "${region}: $(aws ec2 describe-images --owners amazon --filters Name=name,Values=amzn-ami-hvm-*s3 --query "reverse(sort_by(Images, &CreationDate))[0].ImageId" --output text --region $region)"
    done

This snippet loops through all the regions, and return all the Instance-store based Amazon Linux AMI, and sorting in a descending fashion on the **CreationDate**.

JmesPath only sorts lexicographically.. it's not aware of dates.
But this works because the field CreationDate that AWS returns is in [ISO 8601 format](https://www.w3.org/TR/NOTE-datetime), which is lexicographical.

Of course, you can format the output the way you want, and feel free to modify the filter to match the AMI you want.




_There you go! Hopefully, that helps, **Use it, Love it, Live it!**_
