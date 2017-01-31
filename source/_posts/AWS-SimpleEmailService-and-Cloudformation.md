---
title: AWS SimpleEmailService and Cloudformation
author: Karel Malbroukou
tags: [ AWS ]
date: 2013-12-04 13:39:53
---

_Note: This document is based on Amazon Linux AMI (CentOS)._

Welcome back to my blog!
I'm guessing you're here because you're having problem creating a user automagically from your CloudfFormation stack which would be used by your SMTP server for authentication into SES.

Well, you're at the right place!

You create a user in Cloudformation with something like this:

```
[...]
    "SESUser": {
      "Type": "AWS::IAM::User",
      "Description": "User used to send email through SES",
      "Properties": {
        "Path": "/application/",
        "Policies": [ {
          "PolicyName": "DeveloperSiteIAM",
          "PolicyDocument": { "Statement": [
            { "Effect": "Allow", "Action": "*", "Resource": "*" }
          ] }
        } ]
      }
    },

    "SESKeys": {
      "Type": "AWS::IAM::AccessKey",
      "Properties": {
        "UserName": { "Ref": "SESUser" }
      }
    },
[...]
```

You refer those in your **sasl_passwd** as documented by AWS: [Postfix Configuration for SES](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/postfix.html)

But when you try to send an email out, you get this following error:

    SASL authentication failed; server email-smtp.us-east-1.amazonaws.com[174.129.24.189] said: 535 Authentication Credentials Invalid</div>

Frustrating!!! How could you ... not let me pass!?!

Well, relax friends and read again! In that very same page, AWS specified:

    "Your SMTP credentials and your AWS credentials are not the same"

Get it already?

But you can for sure go around that with some scripting!
You can generate a Username / Password to use in your SASL configuration to connect to SES using the keys of that IAM user.

The username is actually the access key id of that user, and the password can be generated like this: [SES SMTP Credentials](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/smtp-credentials.html)

Err ... Java ... algorithms ... go back to school and learn it!
Ok ok, you can find it [here](http://pastebin.com/juNdKwMW) in ruby (is that any better??).
Well, Ruby is usually installed on Linux by default, and sure is on the Amazon Linux Image.
So you can use that script there!

And just because it's Christmas today -- yes it IS --
I'm going to give you an example of a **cloudformation stack** template that uses that script: [here](http://pastebin.com/Zrh681Lw).

The files specified in the template are being downloaded from S3, so make sure you create a bucket and put the files in your bucket with the following paths:
  - [<bucket>/ses_example/postfix_main.cf](http://pastebin.com/VMqwGW1u)
  - [<bucket>/ses_example/sasl_passwd.rb](http://pastebin.com/juNdKwMW)

That cloudformation template is also a good example for you to understand **CfnHup** configuration and **CloudformationInit Mustache** (file templating).

I hope this helped solve your issue!

_Note: This document is based on Amazon Linux AMI (CentOS)._
