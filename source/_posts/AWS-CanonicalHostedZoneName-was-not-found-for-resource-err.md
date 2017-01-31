---
title: 'AWS: CanonicalHostedZoneName was not found for resource... err?!'
author: Karel Malbroukou
tags: []
date: 2014-05-13 19:55:28
---

Hi All,

Happy new year!
First post for 2014, how exciting!!! no... not really ...
Let's get to it!

If you've been using CloudFormation for a while, you might have stumble on this error before:

    Attribute: CanonicalHostedZoneName was not found for resource: ELB

Yeah cool ... but what does that mean? Seriously ... (<-- one of my catch phrases according to my partner's brother :D)

Well it means what it means!
Your LoadBalancer resource that you defined in the Cloudformation stack does not have this attribute.

You're probably trying to create an **AliasTarget** DNS record for your ELB DNS name.

I know what some of you would say:
> But but, dude, I looked at AWS docs, and it says to use CanonicalHostedZoneName, so seriously, I'm no idiot, look for yourself: http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-route53-aliastarget.html

And I would say that you read correctly, but you did not check out the right doc.
Here's another link that says:

> If you specify internal for the Elastic Load Balancing scheme, use DNSName instead. For an internal scheme, the load balancer doesn't have a CanonicalHostedZoneName value: http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-elb.html

Mwahaha! Gotcha!

You're creating an **internal** loadbalancer? Use **DNSName** instead!

The beauty of it is that you can also use DNSName for a **public-facing** loadbalancer... so question is, why do we have the choice?

Beat me.
