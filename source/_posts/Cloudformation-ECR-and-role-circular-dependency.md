---
title: 'Cloudformation, ECR and role circular dependency ...'
author: Karel Malbroukou
tags: [ AWS, IAM, ECR ]
date: 2016-10-02 08:50:12
---

So ... after a few months of abstinence, I'm back into the AWS space.

And working on [uSpot](http://uspot.io/), because I use AWS more than Google Cloud, I decided to move my services from GCE to AWS ECS.
Of course, being an automation engineer, or at least I tend to believe so :D, even my own infrastructure needs to be built properly ... with CI / CD ... infrastructure-as-code etc.

Anyway, yesterday I went on to create a Cloudformation template to create my ECR repository and the role that can push to it, and bumped into a **circular dependency**...
If you look at how to create an ECR repository, you would want to specify an IAM user / role that can access that repository.

Now, because I try to be secure-minded, I want to limit the role I'm created to just that single repo, through its policy.
Here's how it would look like: [Invalid gist](https://gist.github.com/labrute974/034729ad1db74ed16dfdb01d507bbb2e).

Looking at that gist, it makes sense that we get a circular dependency, as both the role and the ECR resources are referencing each other.
Luckily, Cloudformation allows you to create the role policy document separately from the actual role.

That's it!! That's the way around it! ECR depends on the role, not the policy!

Here's the [working gist](https://gist.github.com/labrute974/52da3f0273e62c6ced5bbafccee044d6).

Enjoy!
