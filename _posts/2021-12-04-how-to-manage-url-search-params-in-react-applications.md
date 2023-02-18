---
layout: post
title: "How to manage URL search params in React apps with hooks"
date: 2021-12-04 18:20
categories: [English, Coding]
keywords: react, hooks, url, search, parameters, URLSearchParams
author: Özgür Adem Işıklı
post_img: 37.jpg
post_img_link: https://pixabay.com/photos/red-fox-animal-road-fox-wildlife-6796430/
lang: en
---

## Terminology

This is another simple explanation to myself, to remember a very simple thing; URL search parameters. But first, let's ask a question; what the hell is **URLSearchParams**?

By the definition from <a href="https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams" target="_blank">MDN Web Docs</a>, it is an interface that defines utility methods to work with the query string of a URL. Let's assume that you have an URL like the following one;

`https://example.com?foo=1&bar=2`

In this URL, the `?foo=1&bar=2` part is the _URLSearchParams_. That's all!

## Why I need to manage it?

For example, if the user changes some filters for the pagination in your page, keeping the user's selection in the URL can be very useful. Because when they share the link, you can implement the filters immediately on your API query, or you can do something like that.

I am going to explain it by using the tab selection component. Whenever the user changes the active tab, we are going to keep the selection in URLSearchParams to use laters, and we'll use that data when the page's first visit.

## TabComponent.js

```jsx
import React, { useState, useEffect } from "react";
import { useLocation, useHistory } from "react-router";

const getSearchParam = (name) => {
  const urlParams = new URLSearchParams(window.location.search);
  return urlParams.get(name);
};

export default function TabExample({}) {
  const [current, setCurrent] = useState(getSearchParam("tab") || "tab-1");
  const query = new URLSearchParams(useLocation().search);
  const history = useHistory();

  useEffect(() => {
    query.set("tab", current);
    history.push({ search: query.toString() });
  }, [current]);

  return (
    <div>
      <button type="button" onClick={() => setCurrent("tab-1")}>
        Tab 1
      </button>
      <button type="button" onClick={() => setCurrent("tab-2")}>
        Tab 2
      </button>
      <button type="button" onClick={() => setCurrent("tab-3")}>
        Tab 3
      </button>
    </div>
  );
}
```
