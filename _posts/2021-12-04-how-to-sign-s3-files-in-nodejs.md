---
layout: post
title: "How to sign AWS S3 files in Node.js"
date: 2021-12-04 13:50
categories: [English, Coding]
keywords: aws, s3, signing, pre-signed, files, permission
author: Özgür Adem Işıklı
post_img: 36.jpg
post_img_link: https://pixabay.com/photos/binding-contract-contract-secure-948442/
lang: en
description: In this article you can learn how to sign an AWS S3 file in Node.js.
---

I noticed that every time I need to sign an S3 file, I already forgot how I could do that. Because I can be bored so easily, I am creating this documentation to remember it easily.

## What you need?

Add the `aws-sdk` to your project;

```bash
$ npm install --save aws-sdk
```

## Configuration

You need an S3 configuration like the following example;

```js
import AWS from "aws-sdk";

const s3 = new AWS.S3({
  accessKeyId: "AWS_ACCESS_KEY_ID",
  secretAccessKey: "AWS_SECRET_ACCESS_KEY",
  region: "eu-central-1",
});
```

## Signing a file

And finally, you can sign the file by setting an expiry date.

```js
const signIt = (path, expires = 60 * 60) => {
  return s3.getSignedUrl("getObject", {
    Bucket: "your-bucket-name",
    Key: path,
    Expires: expires,
  });
};
```

## Warnings

You have to have permission to use `getSignedUrl` method with your AWS access keys.
