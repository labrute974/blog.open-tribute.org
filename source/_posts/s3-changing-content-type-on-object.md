layout: posts
title: Changing S3 object content type through the AWS CLI
date: 2017-06-204 19:36:06
tags: [ aws, cli, s3, content-type ]
---

So you've uploaded a file to S3 and want to change its content-type manually?

A good example would be that you have a static website where you're storing a json file containing informations about your app like the version etc. called `info` that you upload together with the sync-ing of the frontend code, using `aws s3 sync` command.

When you do that, S3 doesn't automatically know the Content-Type of the file `info` so it won't set it to `application/json`.. so if you were to check that file by accessing its URL, instead of just displaying the content in the browser, it will get downloaded.

Sure, you could manually go into the S3 console, and set the Content-Type to `application/json`, but in a CI / CD environments, you want this to happen automatically, maybe by using the AWS CLI.

The only way to do this via the command line, is to copy the file.
So you might try this:

    ~ $ aws s3 cp --content-type 'application/json' --acl public-read s3://<bucket>/info s3://<bucket>/info
    copy failed: s3://<bucket>/info to s3://<bucket>/info An error occurred (InvalidRequest) when calling the CopyObject operation: This copy request is illegal because it is trying to copy an object to itself without changing the object's metadata, storage class, website redirect location or encryption attributes.

Um... what?? Error? But I am changing the object metadata!!

Well ... the AWS CLI for S3 has the flag `--content-type` but also have a flag called `--metadata` ... but `--metadata` doesn't allow you to change the content-type.

Instead you can force S3 to think you're changing the metadata with the flag `--metadata-directive REPLACE`.

So try this:

    ~ $ aws s3 cp --content-type 'application/json' --acl public-read s3://<bucket>/info s3://<bucket>/info --metadata-directive REPLACE
    copy: s3://<bucket>/info to s3://<bucket>/info

Yes!!! It worked!

Go enjoy your S3 deployment automation now!
