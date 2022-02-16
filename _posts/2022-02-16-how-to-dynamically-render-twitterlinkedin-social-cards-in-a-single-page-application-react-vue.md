---
layout: post
title: "How to dynamically render Twitter/LinkedIn social cards in a Single Page Application (React/Vue)?"
date: 2022-02-16 19:00
categories: [English, Coding]
tags: twitter, linkedin, cards, open graph, protocol, react, vue, spa, nginx, facebook, social media, proxy
meta: twitter, linkedin, cards, open graph, protocol, react, vue, spa, nginx, facebook, social media, proxy
author: ozziest
post_img: 40.jpg
post_img_link: https://pixabay.com/photos/proxy-proxy-server-free-proxy-4605834/
---

## What is Open Graph?

_Open Graph is a protocol that enables any web page to become a rich object in a social graph. [1]_ Basically, we can use **Open Graph** standards to provide basic information about the page to social media platforms such as **Twitter**, **LinkedIn**, and **Facebook**.

Also, the implementation of social media card creation is so simple. Just check the following codes out.

```html
<html prefix="og: https://ogp.me/ns#">
  <head>
    <title>The Rock (1996)</title>
    <meta property="og:title" content="The Rock" />
    <meta property="og:type" content="video.movie" />
    <meta property="og:url" content="https://www.imdb.com/title/tt0117500/" />
    <meta
      property="og:image"
      content="https://ia.media-imdb.com/images/rock.jpg"
    />
  </head>
  <body></body>
</html>
```

Easy peasy, right?

## The Problem

If your web application uses the [Server-side rendering (SSR)](https://www.educative.io/edpresso/what-is-server-side-rendering) method, implementing this feature is so simple. But in my [secondhand.dev](https://secondhand.dev) project, we don't use **SSR**. We built the [secondhand.dev](https://secondhand.dev) application with [React](https://reactjs.org/) and the application is basically a [Single Page Application](https://en.wikipedia.org/wiki/Single-page_application). That means that we render the page in the _browser_. That's why we can't provide dynamic meta tags when the user adds an URL from our web application.

Because, the response of the server is something like this;

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="description" content="second-hand products from developers" />
    <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
    <title>secondhand.dev - second-hand products from developers</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>
```

In this article, we are going to talk about a specific method to solve this problem although there are many solutions.

## How Bots Work?

**Twitter**, **LinkedIn**, and **Facebook** use a _bot_ to _crawl_ the URL that you share when you paste an URL to share in those social media platforms. In the background, they send a simple **HTTP** request to your server to get your HTML meta tags. In that HTTP request, they use specific [User-Agent](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent) information to present they are not a real user.

<img src="/images/posts/01.gif" class="center" />

This is the way how we can understand whether the request owner is a bot or not.

## Bot Redirection

I use [Nginx](https://www.nginx.com/) as the webserver to serve [secondhand.dev](https://secondhand.dev). If you are using another way to serve your application, like [CloudFront](https://aws.amazon.com/cloudfront/), your steps would be different than mine. But still, you should do the same thing; redirecting the bot request to another application.

The following codes show the front-end application configuration. I've reduced the complexity, of course. But you will get the point.

```nginx
server {
  server_name secondhand.dev;
  root /app;
  index index.html index.htm;

  location / {
    try_files $uri $uri/ /index.html;
  }
}
```

We are going to add a simple redirect to this configuration to handle bot requests by checking the User-Agent data, and we will redirect the request to another [Node.js](https://nodejs.org/en/) application.

```nginx
server {
  server_name secondhand.dev;
  root /app;
  index index.html index.htm;

  if ($http_user_agent ~* (twitterbot|LinkedInBot|facebookexternalhit)/) {
    rewrite ^(.*)$ /bot/$1;
  }

  location /bot/ {
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header Host $http_host;

    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";

    proxy_pass http://bot_handler/;
    proxy_redirect off;
    proxy_read_timeout 240s;
  }

  location / {
    try_files $uri $uri/ /index.html;
  }
}
```

In this configuration, we add three things;

- Handling bot requests by checking User-Agent data;
  - `if ($http_user_agent ~* (twitterbot|LinkedInBot|facebookexternalhit)/)`
- Adding rewrite rule;
  - `rewrite ^(.*)$ /bot/$1;`
- Handling bot rule and proxy requests to another point;
  - `proxy_pass http://bot_handler/;`

At last, we should define the proxy handler.

```nginx
upstream bot_handler {
  server 127.0.0.1:4000;
  keepalive 64;
}
```

With this configuration, **Nginx** handles all requests but it will redirect bot requests via proxy*pass to a Node.js application that runs on `4000` ports. Now, it is time to build the \_Node.js application*.

## Node Application

Please keep in mind, we just aim to build a simple Node.js application to answer social media bots. The real implementation can be more complex by your requirements. In here, I'll show just the basic stuff.

Let's create a simple application;

```bash
$ npm init -y
$ npm install --save express
```

> Probably you'll need a database connection. So you have to add your database connection libraries by your choice.

Let's create the simple handler (`index.js`);

```js
const express = require("express");
const app = express();

app.get("/", async (req, res) => {
  return res.send("Twitter bot!");
});

app.get("/post/:slug/:id", async (req, res) => {
  const { id } = req.params;
  if (!id) {
    return res.status(404).send("Not found");
  }

  // TODO: fetch your post from database here
  const post = {
    title: "Your post title",
    description: "Your post description",
    url: "https://your-app.com/post/your-post-title/1",
    image: "https://your-app.com/images/01.jpg",
  };

  return res.send(`
    <html>
      <head>
        <meta name="twitter:card" content="summary" />
        <meta name="twitter:site" content="@your_account" />
        <meta name="twitter:creator" content="@your_account" />
        <meta name="twitter:title" content="${post.title}" />
        <meta name="twitter:description" content="${post.description}" />
        <meta name="twitter:url" content="${post.url}">
        <meta name="twitter:image" content="${post.image}">

        <meta property="og:url" content="${post.url}" />
        <meta property="og:title" content="${post.title}" />
        <meta property="og:description" content="${post.description}" />
        <meta property="og:image" content="${post.image}" />
        <meta property="og:image:width" content="512" />
        <meta property="og:image:height" content="512" />
        <meta property="og:image:alt" content="${post.title}" />
      </head>
      <body>
      </body>
    </html>
  `);
});

app.get("*", async (req, res) => {
  return res.send("Twitter bot!");
});

console.log("listening on 4000");
app.listen(4000);
```

In this example, we implement the following steps;

- Create an `express` application and listen `4000` port for HTTP requests.
- Listen to `/post/:slug/:id` route to create dynamic meta tags for blog posts. It could be different in your application, of course.
- Fetching the data from database by the unique value (in my example, it is `id` param).
- Return all meta tags by the post data.

With this method, when a social media bot (_Twitter_, _LinkedIn_, _Facebook_) sends a request to get meta tags, _Nginx_ will redirect the request to our _Node.js application_ first. Then, _the Node.js application_ will fetch the data from the database and it will return the meta tags.

## Summary

I think that social media cards are very important to show users elegant views. Look at the differences between the two tweets, and say I am wrong.

<img src="/images/posts/41.png" class="center" />

On the other hand, please keep in mind that there are several different ways to trick bots. But this is very simple, clean, and performant.

## References

[[1] The Open Graph protocol](https://ogp.me/)
