layout: posts
title: Cloudformation Transform::Include Limitations
date: 2017-05-12 23:16:05
tags: [ aws, cloudformation, include, tranforms ]
---

It's been now a few days since I've played with [Transform::Include](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/create-reusable-transform-function-snippets-and-add-to-your-template-with-aws-include-transform.html).

There's two limitations I've found so far:

  - One of them is using [short-notation intrinsic cloudformation functions](http://blog.open-tribute.org/2017/05/09/cloudformation-include-yaml-issue/)
  - The other one is that you can't use two **includes** at the same level in your Cloudformation template

If you follow the link for the first one, you will understand what the issue.

For the second one, please follow my lead!

Let's say example, you would like to separate the resources in your template logically into snippets.

For example, you might want to create an S3 bucket and a CloudFront distribution on top of the S3 bucket to be able to use custom SSL, or maybe just for actual caching capabilities.


Doing this in one template is probably the way to go, as they would be tightly attached, but your template might grow very quickly, as you might have an S3 bucket resource, the SSL certificate resource, the CloudFront resource and possibly the S3 bucket policy as well..

Your first thought might be to separate the **S3 bucket + policy** into one snippet, and the **CloudFront + SSL certificate** into another snippet!
Great idea! That's what I would do.. except, you can't do it!

Let's assume you have something similar to:
```
AWSTemplateFormatVersion: '2010-09-09'
Description: Transform Include example

Parameters:
  ArtifactBucket:
    Type: String

Resources:
  'Fn::Transform':
    Name: 'AWS::Include'
    Parameters:
      Location: !Sub "s3://${ArtifactBucket}/s3.yaml"

  'Fn::Transform':
    Name: 'AWS::Include'
    Parameters:
      Location: !Sub "s3://${ArtifactBucket}/cloudfront.yaml"
```

What will actually happen, is that Cloudformation would only deploy the second **Include**!

It would be really awesome if you could use multiple Includes on the same level.. but who knows, that might be coming in the future..
Or even, right now, AWS already has a fix and is pushing it to **us-east-1** as you read!

Remember they keep pushing improvements every hour!
