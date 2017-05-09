layout: posts
title: Cloudformation Transform::Include - YAML/JSON malformed
date: 2017-05-09 22:29:49
tags: [ aws, cloudformation, transforms ]
---

Good day y'all,

You're here because you probably had issues using the [Transform::Include](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/create-reusable-transform-function-snippets-and-add-to-your-template-with-aws-include-transform.html) feature of Cloudformation.

_Note: by the time you get to this blog, AWS might have fixed this bug._

Let me tell you .. it's awesome!

Most people I've worked with have been looking for this for a while! To achieve it, they were using Nested stacks.
But we all know that it's very different.

I was personally very excited to see that announcement. But really ... I only had an opportunity to start using it yesterday.
And it's awesome!

Now there's a few gotchas ... that might get fixed in the future, but for now there's a few issue.

Let's consider the following cloudformation template:
```
AWSTemplateFormatVersion: '2010-09-09'
Description: Transform Include example

Parameters:
  ArtifactBucket:
    Type: String

  BucketName:
    Type: String

Resources:
  'Fn::Transform':
    Name: 'AWS::Include'
    Parameters:
      Location: !Sub "s3://${ArtifactBucket}/s3.yaml"
```
This cloudformation teamplte basically is just gonna to create an s3 bucket.
The definition of the S3 resource would be done in the `s3.yaml` file, hosted in our artifact bucket.

The `s3.yaml` would have the following content:
```
AppBucket:
  Type: "AWS::S3::Bucket"
  Properties:
    BucketName: !Ref BucketName
```

So far, easy hey!

_Note: Before you try to deploy the template, make sure you've uploaded the `s3.yaml` snippet in the right location first_

Now, let's try to deploy that cloudformation template:

```
~ $ aws cloudformation deploy --stack-name transform-include-example --template-file file://cloudformation.yaml
Waiting for changeset to be created..

Failed to create the changeset: Waiter ChangeSetCreateComplete failed: Waiter encountered a terminal failure state Status: FAILED. Reason: Transform AWS::Include failed with: The specified S3 object's content should be valid Yaml/JSON
```

Wait ... what ?! Is that telling me that the `s3.yaml` file is not valid?? But .. when I lint my yaml file, it says it's valid!!

Well well... actually, not sure if it's a bug, or not (although it smells like one), but basically you cannot use the short notation of the Cloudformation intrinsic functions.

You can't use `!Ref BucketName` in this case.

Instead the correct snippet would be:
```
AppBucket:
  Type: "AWS::S3::Bucket"
  Properties:
    BucketName:
      Ref: BucketName
```

Try that, and let me know on [Twitter](https://twitter.com/labrute974)!
