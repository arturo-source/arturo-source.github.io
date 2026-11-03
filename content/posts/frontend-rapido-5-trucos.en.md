---
title: "5 tricks to make your frontend load 12 times faster"
description: "Five simple optimizations you can apply today in your project to go from 13 MB and 15 seconds of loading to 175 KB and 1 second."
summary: "Five simple optimizations you can apply today in your project to go from 13 MB and 15 seconds of loading to 175 KB and 1 second."
date: 2026-11-03T18:12:43+01:00
draft: false
tags: ["frontend", "web"]
---

{{< youtube eREOIdKyqmA >}}

I'm going to show you five tricks that frontend engineers use to make a website load very fast, and that are very easy to apply to your current project. To test them I used a very simple example application (a shop with about twelve products, a chart, reviews and a search box) that went from **13 MB and 15 seconds** of loading on a regular 4G network, to **175 KB and 1 second**. With the same features.

## How to measure: Chrome DevTools and Lighthouse

Before optimizing anything you have to measure. Open Chrome DevTools (with `F12`, or like me, with `Ctrl + Shift + I`) and go to the **Network** tab. There you need to do two things:

1. Check **Disable cache**, because we want to simulate a user visiting for the first time.
2. In the *throttling* dropdown, choose **Fast 4G**, to simulate a phone on a 4G network.

Now reload the page and you'll see everything that gets downloaded: how many megabytes are transferred, how long it takes to be visible (the blue number, `DOMContentLoaded`), how long it takes to load everything, and you can filter by type (images, fonts, JavaScript, CSS, and `Fetch/XHR`, which are the calls from JavaScript to the backend).

Google also gives us a tool called **Lighthouse** (also inside DevTools), which scores your website using the metrics Google uses to decide whether your site is good or not. The initial version of my application scored **41 points on mobile and 74 on desktop**. Very bad, we would expect 100.

## 1. Lazy loading and code splitting

When a user visits for the first time, they don't need to load everything, only what they see. In my application, for example, the reviews aren't visible until you click the button to see them. So, why download their JavaScript and their data at the beginning?

With **code splitting** you split your JavaScript into chunks, and with **lazy loading** you only download each chunk when it's needed. When you click the button, two JavaScript files are downloaded and the call to the backend that returns the reviews is made.

What we've done is defer what isn't visible initially, so the page loads faster. With this we go to **59 on mobile and 92 on desktop**. It improves, but the application still weighs practically the same (12.8 MB versus 13.5 MB), so there's something else we can do.

## 2. Modern image formats

In the Network tab, if you filter by images you'll see they took up **11 MB**. It's the clearest place to optimize. The images were JPG, an old compression format. Now there are formats like **WebP** and **AVIF**, which compress far more without losing practically any quality.

The simplest thing is to convert the image and change the `src` of your `img` tag. But if you want to keep compatibility, because some client of yours can only render JPG, use the `picture` tag:

```html
<picture>
  <source srcset="product.avif" type="image/avif" />
  <source srcset="product.webp" type="image/webp" />
  <img src="product.jpg" alt="Product" />
</picture>
```

The browser picks the first one it supports: if it doesn't know AVIF, it tries WebP, and if not, it uses the same old JPG. With this the application went from 13 MB to **1.3 MB**.

The funny thing is that in Lighthouse it barely changed: 60 on mobile and 95 on desktop (before 59 and 92). But you can see it yourself, images load much faster, and for a user it's far more satisfying to see the image instantly than to wait 10 seconds. **Metrics aren't everything**, and you have to look at the details.

## 3. Don't block rendering

Google doesn't like it when you block the page while it's loading. There are two things you can do:

- **Defer the scripts that aren't needed right now.** In my case, `lodash` (an external library) and the *analytics* (which is my own code). Without deferring them, nothing else starts until they finish downloading. With them deferred, they load later, once the page is already visible.
- **Give top priority to the main image.** The *hero* image is the first one the application needs, so we add `fetchpriority="high"` so the browser doesn't load it after rendering everything, but at the same time, because it's important.

```html
<script src="analytics.js" defer></script>
<img src="hero.avif" fetchpriority="high" alt="Hero" />
```

This goes up to **72 on mobile and 97 on desktop**. It's not as decisive as the previous trick, you don't feel the application has improved a lot, but Google does appreciate it, and you lose nothing by doing it.

## 4. Reduce network requests

The general rule is that **fewer requests is better**. If you could return the application already rendered without making any more requests, for Google that would be perfect. It's not an absolute rule, but it works as a guide. Before we had 52 requests and now we have almost half. What did I change?

- **A single call for the products.** Before, it requested the products one by one; now the backend returns all twelve at once. If your backend allows it, better (although normally they'd be paginated).
- **SVGs inside the HTML.** Before there were eight SVG requests, and now there are none because they're embedded directly in the HTML. There can be reasons to request them separately, but if you embed them you'll get a better score.
- **Remove unnecessary libraries.** I was using `lodash` (and something like `moment` to handle dates). Many of their functions have been in native JavaScript for 5 or 6 years, so you probably don't even need them. And if you use a function like `debounce` (which doesn't make the request until some milliseconds pass without typing), it isn't native in JavaScript, but it's very easy to implement. `lodash` takes half a megabyte; my implementation, **0.6 KB**.

With this we reach **100 points on mobile and 100 on desktop**.

## 5. Compression and aggressive caching

We already have 100 points, what else can we improve? Well, Lighthouse measures the **first load**, but the people using your application visit once a day, or five times every hour, and for that we need other optimizations. The previous ones serve the first load; this one, the day-to-day.

### Compression

If you hover over a resource in the Network tab, you'll see two sizes: what was transferred and the real size of the resource. My HTML was 7.5 KB but was transferred as 2.6 KB. The chart library went from 200 KB to 60 KB. That's compression.

The browser sends in its headers something like `Accept-Encoding: gzip, br, zstd`, that is, "I accept you sending it compressed with gzip, or with Brotli or Zstandard", which are two modern compression types, better than gzip. In my case I send it in **Brotli**, which for HTML and JavaScript (in general, anything that's text) is usually the best option.

### Cache

Until now I had always checked *Disable cache*. If I uncheck it and reload, you'll see the resources appear as `memory cache`: the request doesn't even reach the server. If you looked at your server logs (Nginx, Caddy, Apache...), there would be no request for the image or the JSON. Everything is in the client's cache.

We achieve this with the `Cache-Control` header. In the previous version I forced the browser to never cache, and what happened is that the browser asked the server "hey, I have this with this identifier (the `ETag`)", and the server answered with a `304 Not Modified`: "it hasn't changed, don't download it again". It doesn't use many megabytes, but it makes a round trip to the server, and the application is slower.

The technique is to put a unique identifier in the file name, between the name and the extension (something like `app.3f9a2c.js`). Almost all *bundlers* do this (Webpack, Vite...): they minify your code and add a hash that changes every time the file changes. This way you can cache very aggressively, for example with a lifetime of one year:

```
Cache-Control: public, max-age=31536000, immutable
```

You're telling the browser "this file will never change, don't ask me for it". If it changes, it will have another name and be downloaded again. You can do the same with images. This way you save resources on your server, and loading time for your user the second and third time they visit, keeping the 100% in Lighthouse.

## Summary

| Optimization | Mobile | Desktop |
| --- | --- | --- |
| Initial version | 41 | 74 |
| 1. Lazy loading and code splitting | 59 | 92 |
| 2. Modern image formats | 60 | 95 |
| 3. Defer scripts and prioritize the main image | 72 | 97 |
| 4. Reduce network requests | 100 | 100 |
| 5. Compression and aggressive caching | 100 | 100 |

These are the five fundamental tricks you can apply today so your application loads faster and your users are happy. If you don't want to do it by hand, you can always give the video to an AI and tell it "apply these five points to my project".
