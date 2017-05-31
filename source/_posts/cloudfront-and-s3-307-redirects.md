layout: posts
title: 307 Redirect when accessing S3 through Cloudfront?
date: 2017-05-30 08:20:13
tags: [ redirect, error, s3, aws, cloudfront ]
---

So you're trying to configure Cloudfront on top of an S3 bucket where you host your static website?

And you're getting [307](https://httpstatuses.com/307) redirecting to the S3 bucket DNS when accessing the DNS you configured in Cloudfront?

laBrute is coming to your rescue ... well... you might not like the answer.

But let's wind back a bit and look at the scenario:

  1. You created an S3 bucket called: `origin-mysite` in the Sydney region, which would have the URL *origin-mysite.s3-ap-southeast-2.amazonaws.com*
  2. You then create a Cloudfront distribution with that bucket set as the origin
  3. You create a DNS name `www.mysite.com` that points to CloudFront and have it set in Cloudfront as [Alternate Domain Names](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/CNAMEs.html)
  4. You try to now access your site via `http://www.mysite.com` but get redirected to `http://origin-mysite.s3-ap-southeast-2.amazonaws.com` ... ?!

That's because of the distributed nature of S3.

```
If a request arrives at the wrong Amazon S3 location, Amazon S3 responds with a temporary redirect that tells the requester to resend the request to a new endpoint.
```

You can read more about it [here](http://docs.aws.amazon.com/AmazonS3/latest/dev/Redirects.html).

Basically the fix is ... to wait for it!

Note that if you were using the `us-east-1` region, you should not hit this specific problem.
