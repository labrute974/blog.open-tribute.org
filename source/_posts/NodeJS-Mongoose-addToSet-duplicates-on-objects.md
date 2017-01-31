---
title: NodeJS & Mongoose $addToSet duplicates on objects
author: Karel Malbroukou
tags: [ mongo, mongoose, js, javascript ]
date: 2015-05-09 21:04:25
---

Hi Fellow Surfers,

Today I'll talk a bit about **Mongoose** which is a NodeJS module to allow use manage your MongoDB models / schemas.

I've been working with **NodeJS** for the last few months when I started my journey on creating my first iPhone app: [uSpot](http://www.facebook.com/uspotapp).

**NodeJS** and **MongoDB** are both very new to me, so of course, I've been hitting a lot of blockers due to my beginner experience!

One of them is the use of **_$addToSet_** operator that MongoDB provides when updating a document.

This operator allows you to add unique value to an array field on your document.

Let's say you had a shopping list of a list of items: _**[ "bananas", "apples" ]**_ and you were to update the shopping list by adding an item called _**"bananas"**_, by using _**$addToSet**_, Mongo would check if the value exists already or not, if not, add it to the array.

So far, how great! Now, let's say you want to use a Javascript Object instead.

For example, let's say you have an array like this:
``` json
[
  { "name": "bananas", "qty": 2 },
  { "name": "apples", "qty": 4 }
]
```
If you were to do:
``` Javascript
shoplist.update( { "$addToSet": {
  "items": { "name": "apples", "qty": 4 }
} }, function(err, list) { ... });
```

The resulting array is:
``` json
[
  { "name": "bananas", "qty": 2 },
  { "name": "apples", "qty": 4 },
  { "name": "apples", "qty": 4 }
]
```
Why ... wait ... what?! Now, that's unexpected!

Guess why that happens?
That's because **Mongoose** by default creates a new _**MongoDB ObjectId**_ ( this hidden **__id_** field) every time you pass it a **Javascript Object** to update the field of a document.

Now how to go around it?
You can tell **Mongoose** to not create a new **_ObjectId_**, by making sure your **mongoose schema** is as followed:
``` Javascript
var mongoose = require('mongoose');

var shopListSchema = {
  "name": { type: "String" },
  "items": [
     {
        "name": { type: "String" },
        "qty": { type: "Number" },
       ** "_id": false**
     }
  ]
}
```
Setting false to the **__id_** property gives you the expected result!

Time to get back to code!
