---
title: "What is Hugo? The blazingly fast CMS"
description: "Take a look at this CMS created with Go, the fastest CMS in the world."
summary: "Take a look at this CMS created with Go, the fastest CMS in the world."
date: 2024-01-24T14:20:35+01:00
draft: false
tags: ["tiny pills"]
---


Hugo is a static website builder. It is incredibly fast, both building the site (>1ms per page), and serving the pages.

HUGO does not have a meaning per se, it is just the name of the tool. The key to its speed lies in being **static**. Generally you will use an HTTP server to serve your website (Apache HTTP server, NGINX, Caddy, etc.) to serve your PHP CMS (WordPress, Magento, Joomla, etc.). You can even use an HTTP server as a proxy for your web applications made with Python, NodeJS, Go, etc. But all the previous options have in common that they are **dynamic**, because they have to execute code on the server.

## How does Hugo work? What does it do so fast?

When we build a website with Hugo, we will write the content with **Markdown**, it compiles all the content into HTML, and links it. This way, we can upload the result to our favorite HTTP server, and when a user requests the page, the server doesn't have to execute any logic (connecting to a DB, processing data, etc.), which makes it so fast.

Therefore, Hugo is fast in three different aspects:

1. Serve content to users quickly (it's simple HTML).
2. Build the site quickly (less than 1ms per page).
3. You write the article very quickly (Markdown is incredibly easier to use than HTML).

## Advantages and disadvantages of Hugo

You already know the advantages: speed and simplicity in all aspects. The disadvantage is in turn its greatest advantage, which is **static**. This means that, no matter how fast it is, it does not allow you to execute code on the server. But this makes it perfect for creating sites that do not need logic, such as a portfolio, or a blog.

Other hidden advantages, which make it so powerful, are:

- You can create multilingual websites.
- Contains functions to optimize SEO.
- You can extend the functionality of Markdown, using Go templates.
- You have a [large number of free themes](https://themes.gohugo.io/), or you can build your own.

## If it is that easy, how do I use Hugo?

Of course, first you have to [install hugo](https://gohugo.io/installation/). To create a site, simply run:

```sh
hugo new site yourwebname
```

This creates a folder called `yourwebname` that contains everything you need. You can access it by doing `cd yourwebname`.

And next is [choose a theme](https://themes.gohugo.io/), for this example I will use **hugo-book**.

```sh
git init
git submodule add https://github.com/alex-shpak/hugo-book themes/hugo-book
echo "theme = 'hugo-book'" >> hugo.toml
```

And you are ready to write your first content, usually the `posts/` folder is used, but you can create them wherever you want, for the example I will use `blog/`.

```sh
hugo new content blog/my-first-post.md
```

And the file `yourwebname/content/blog/my-first-post.md` will have been created, which is a copy of `archetypes/default.md` with information about the article you are going to write.

If you also need to modify global information on the website, you can do so in `yourwebname/hugo.toml` (Hugo accepts **toml**, [**yaml**](/posts/what-is-yaml/), and [**json**](/posts/what-is-json/) for configuration). Here you can modify the settings of Hugo, and the theme you have chosen.

And now all that's left is to get the website up and running, you can do it with `hugo server`, or `hugo server -D` if you want to see the articles you have as `draft: true`, you'll see how fast it goes! But this will only work on your computer, if you want to make it public (deploy to production), [choose your favorite way among all these](https://gohugo.io/hosting-and-deployment/).

For more information, consult the [official Hugo website](https://gohugo.io/).
