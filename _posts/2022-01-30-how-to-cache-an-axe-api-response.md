---
layout: post
title: "How to cache an Axe API response?"
date: 2022-01-30 10:00
categories: [English, Coding]
keywords: axe-api, redis, caching, rest, api
author: Özgür Adem Işıklı
post_img: 39.jpg
post_img_link: https://pixabay.com/photos/cheetah-cat-wildcat-predator-5537416/
lang: en
---

## What is Axe API?

Axe API is the fastest way to create Rest API by defining only database models and relationships between them. It is built on Knex.js, and its awesome active records pattern. On the other hand, you have another familiar thing, Express.

Axe API provides you the ability to separate your common tasks to build an API from your business logic. Axe API expects model definitions to analyze your routing structure. After you created your models and their relations between them, Axe API can handle all well-known API requests. Creating an API with 5 tables takes almost 15 minutes.

Shortly, Axe API performs three basic functions;

- Analyzes your models and their relationships to create routes.
- Handles all HTTP requests.
- Separate your business logic from API best practices.

## Caching in Axe API

In this article, we are going to talk about a little bit of caching.

Caching is something that can be seen as simple but it can be so complicated over time. I am not intending to show all details about caching. But here, we can talk about a simple caching implementation.

**Axe API** doesn't provide a caching mechanism internally like [Authentication](https://axe-api.github.io/security/authentication/) and [Rate Limiting](https://axe-api.github.io/security/rate-limiting/). But you can add a simple caching middleware to your endpoints.

## Middleware

First of all, let's create a simple caching middleware;

`app/Middlewares/cache.js`

```js
import Redis from "ioredis";
const redis = new Redis();

export default async (req, res, next) => {
  next();
};
```

> You have to install `ioredis` library by using `npm install ioredis --save` command. In this example, we are going to use [Redis](https://redis.io/).

After that, we need to bind this middleware to a model. Let's assume that you have a model called `City`. In that model, you want to cache pagination responses by query parameters.

`app/Models/City.js`

```js
import { Model, HANDLERS } from "axe-api";
import cache from "../Middlewares/cache.js";

class City extends Model {
  get middlewares() {
    return [
      {
        handler: HANDLERS.PAGINATE,
        middleware: cache,
      },
    ];
  }
}

export default City;
```

## Defining Cache Key

Let's assume that your city API response will be the same for all users. Authentication is not important for us. But as you can guess, the query parameters are so important. Users should be able to query all cities, and we should keep queried data in the Redis server.

To detect query parameters, we can use Express' `originalUrl` value.

```js
import Redis from "ioredis";
const redis = new Redis();

export default async (req, res, next) => {
  const key = req.originalUrl;
  next();
};
```

But I don't think that using a value like `api/cities?q={"title.$like":"*ber*"}` is not a good thing. That's why I can suggest hashing the key, like the following examples;

```js
import Redis from "ioredis";
const redis = new Redis();
const { createHmac } = await import("crypto");

export default async (req, res, next) => {
  const key = createHmac("sha256", "APPLICATION-SECRET")
    .update(req.originalUrl)
    .digest("hex");
  next();
};
```

In this way, we may keep the data with different keys for different requests.

## Getting Data From Cache

First, we should check the key is exists on the Redis Server. If there is a key, we should return that value and we should add a simple HTTP header to the response to explain the request hits the cache or not.

```js
import Redis from "ioredis";
const redis = new Redis();
const { createHmac } = await import("crypto");

export default async (req, res, next) => {
  const key = createHmac("sha256", "APPLICATION-SECRET")
    .update(req.originalUrl)
    .digest("hex");

  const cachedValue = await redis.get(key);
  if (cachedValue) {
    res.set("X-AxeAPI-Cache", "Hit");
    return res.json(JSON.parse(cachedValue));
  }

  next();
};
```

## Setting Data To Cache

If there is not any value in Redis Server, we should execute the HTTP request. But we need the response body to keep it in the Redis. We can handle it with the following code;

```js
import Redis from "ioredis";
const redis = new Redis();
const { createHmac } = await import("crypto");

export default async (req, res, next) => {
  const key = createHmac("sha256", "APPLICATION-SECRET")
    .update(req.originalUrl)
    .digest("hex");

  const cachedValue = await redis.get(key);
  if (cachedValue) {
    res.set("X-AxeAPI-Cache", "Hit");
    return res.json(JSON.parse(cachedValue));
  }

  const oldJson = res.json;
  res.set("X-AxeAPI-Cache", "Missed");
  res.json = async (body) => {
    await redis.set(key, JSON.stringify(body), "EX", 60);
    return oldJson.call(res, body);
  };

  next();
};
```

With this code, we can keep the HTTP response body in the Redis Server for sixty seconds. Also, if we got the same request with the same query parameters, we can use the cached value.

## Summary

This code example is a simple demonstration of how to cache API responses in Axe API. You can find more detail about the Axe API [here](https://axe-api.github.io/getting-started/introduction/). Of course, if your API data is changeable, you should think about a solution for cache invalidation. Maybe it is time to use the power of [Hooks](https://axe-api.github.io/advanced/hooks/) or [Events](https://axe-api.github.io/advanced/hooks/#events).
