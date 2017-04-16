layout: posts
title: Error when using Cloudformation package for Lambda functions?
date: 2017-04-16 18:59:52
tags: [ AWS, Lambda, Cloudformation, S3 ]
---

If you're like me, and you've been using Lambda, you would have been delighted when you found out that Cloudformation now had a [transform for Lambda](https://aws.amazon.com/blogs/compute/introducing-simplified-serverless-application-deplyoment-and-management/).

This post won't get in much detail on how to implement a deployment using this **Transform** feature.. but instead, it will go through one of the issues I had when using it.

If you're here, that's because you've probably hit an issue that I had:

    'NoneType' object has no attribute 'get'

What does that even mean !? Not a lot indeed ..

So I ran the `aws cloudformation package` command with the `--debug` flag, and this line stood out:

    [...]
    2017-04-16 18:28:12,761 - MainThread - botocore.args - DEBUG - The s3 config key is not a dictionary type, ignoring its value of: None
    [...]

But really!? I interpreted this as the s3 bucket I specified to the package command does not exist .. but it does..

I've been using Cloudformation for a while and I've been relying a lot of `aws cloudformation create-stack` to validate my templates as I deploy them.. I just didn't see a great value at using `aws cloudformation validate-template` up to now..

Yes, in a CI, it would be nice to separate the validation step from the deployment step, but really ... I didn't see much value as the `create-stack` or the `update-stack` would run the validation anyway!
I know .. I could have at least lint'ed my YAML but hey! I didn't!

Well ... now I found a good reason for it.. 

The issue was that my YAML was invalid! Of course!!

Guess what ... I'll be sure to at least lint my YAML now!!!
