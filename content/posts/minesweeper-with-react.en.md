---
title: "Minesweeper made with React"
description: "The well-known minesweeper game, created with React, try it now and see how it was made."
summary: "The well-known minesweeper game, created with React, try it now and see how it was made."
date: 2023-09-06T16:42:03+02:00
draft: false
---

This week I started a project to refresh my knowledge of React. But since it was a side-project, I wanted to take as little time as possible.

{{< link-button name="Play now" href="/minesweeper/" >}}

## How did I create the minesweeper?

You can [find the code on my github](https://github.com/arturo-source/minesweeper-react/), but the interesting thing is that it didn't take me long to set up the project. To create it I used vite, simply typing `npm create vite@latest` on the command line.

I chose React as the framework, and among the options to compile the project and develop it, I used SWC, since it is a faster bundler and allows me to speed up the work.

Instead of TypeScript I used JavaScript, because of what I mentioned before, I want it to be a quick project, and not take too long.

Since Vite already builds everything you need in the package.json, when I start developing I just run `npm run dev`. While building the project is `npm run build`.

## How to deploy the project?

To build the project you already know that simply running `npm run build` will create a folder called `./dist`. This folder contains everything you need to run the application. But my goal was to have the game available on my website, and just dragging the folder onto my website didn't work.

If you look at the `index.html` that is inside the `./dist` folder, the following lines appear that are responsible for linking the CSS and JS.

```html
<script type="module" crossorigin="" src="/assets/index-3d1a63c8.js"></script>
<link rel="stylesheet" href="/assets/index-bab0855b.css" />
```

The problem was that the paths were absolute because they started with a slash `/`, but if you remove the slash at the beginning, the browser understands that it has to use an absolute path, so it won't matter if your game is in a subfolder, as long as when the assets (CSS and JS) are in the same folder.

## Try the game yourself

{{< iframe src="/minesweeper/" >}}