---
title: How to get your SES SMTP password from an Amazon secret key
author: Karel Malbroukou
tags: [ aws, ses, ruby ]
date: 2013-09-26 18:15:20
---

Like a lot of you guys, I've been using Amazon Web Services for a bit now.

And now, planning on using SES, I've having issues.
You would think that, following this [documentation](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/postfix.html), it seems easy to setup.

But by default, you would have to create a user by using the link that the SES console gives you to create a new IAM user, which will have an SMTP password linked to.
Problem is, most of them might not want to create a new IAM user, or might wanna create that IAM user through Cloudformation (see blog post).

Amazon gives you a way to generate that SMTP password from the Secret Key of an IAM user, but in Java!! Linux do not have Java installed by default, and seriously what a pain if you have to install Java just for that!

But, Linux does have Ruby, most of the cases, installed, with openssl and base64 gems!
That's all we need right here!

So how to generate that SMTP password with ruby? Here you go:

``` ruby
#!/usr/bin/ruby
require 'openssl'
require 'base64'

sha256 = OpenSSL::Digest::Digest.new('sha256')
secret_key = "Secret Access Key"
message = "SendRawEmail"
version = "\x02"

signature = OpenSSL::HMAC.digest(sha256, secret_key, message)
verSignature = version + signature

smtp_password = Base64.encode64(verSignature)
puts smtp_password
```

And all done!
