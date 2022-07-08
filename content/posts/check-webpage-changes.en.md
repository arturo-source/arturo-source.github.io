---
title: "Check Webpage Changes"
description: "A PHP script that you can easily configure to check for changes to a web page, and notify you via Telegram."
summary: "A PHP script that you can easily configure to check for changes to a web page, and notify you via Telegram."
date: 2022-07-08T19:44:19+02:00
draft: false
---

The script that I will talk about in this post [can be downloaded here](https://github.com/arturo-source/check-webpage-change).

## A really simple php script to notify html changes in static pages

Have you ever wanted to be aware of the changes of a website? You may want to see how the price of a product that interests you evolves, or you know what changes you want to see!

With this simple script you can do it in just a few seconds. Let's look at the simple steps to configure the script. All you have to do is to set up `settings.json` file.

First, you need to have installed PHP interpreter, and crontab on your computer or your server.

### Variables

The variables you'll need to change are:

```json
{
  "url": "",
  "check_changes": true,
  "notify_telegram": true,
  "chat_id": "",
  "bot_token": "",
  "xpaths": {
    "Price": "/html/body/main/div[4]/.......",
    "Units for sale": "/html/body/main/div[4]/.......",
    "Page Title": "/html/head/title"
  }
}
```

- url **(required)**
- xpaths **(required)**
- chat_id (optional, for telegram notify)
- bot_token (optional, for telegram notify)

Get `url` is very easy, you can copy it from the top of the browser.
Get `xpaths` is a little harder, once you're in the webpage, you have to right click on it. Then you select "Inspect" option. Then, you'll see an arrow item as next one (left):

![Selector](https://github.com/arturo-source/check-webpage-change/blob/main/images/selector.png?raw=true)

Now you have to select the html item in the webpage, left click on that.

![HTML selected](https://github.com/arturo-source/check-webpage-change/blob/main/images/html-selected.png?raw=true)

Then, the html code will have been marked, so your last step is to right click on it, and select "Copy" > "XPath"

![Copy Xpath](https://github.com/arturo-source/check-webpage-change/blob/main/images/copy-xpath.png?raw=true)

The final step is pasting it on json settings `"Price": "/html/body/main/div[4]/......."` (left is an identificator name, and right one is the xpath), and you'll have the script configured (remember you can add all xpaths, as many as you want). But maybe you want to be notified when any change ocurred, so you have to configure notifications.

### Notifications

It's really easy too, if you've ever used Telegram. I asume you have Telegram account and a client to use it.

1. Create a bot. (Talk to @BotFather, it will guide you)
2. Copy bot token. (You can paste it now on json settings, otherwise you will have to do it later)
3. Talk your new bot. You can talk it directly, or create a group (or channel) with your friends and add the bot there.
4. Access next url on your browser (don't forget change \<token\> with your token): [https://api.telegram.org/bot\<token\>/getUpdates](https://api.telegram.org/bot<token>/getUpdates)

You will get a json like the next one:

![Get ChatID](https://github.com/arturo-source/check-webpage-change/blob/main/images/get-chatid.png?raw=true)

Then you can pick the Telegram ChatID and paste it in the json settings too, remember `notify_telegram` has to be `true` to enable notifications. And you'll have script totally configured. But now you have to decide how often you want to be notified.

### Set up crontab

Crontab is a really usefull tool that you may have installed in your Linux computer or server. It helps you to do recurring tasks automatically. And it's really easy to set up, but your first time use may will be confusing. You can access [this page to configure crontab easily](https://crontab.guru/).
The most common option will be on Sunday, at 12:00 for example, so you will type `0 12 * * 0` next to the command. But maybe you want to execute it always you turn on the computer, then you will type `@reboot` next to the command.

To open cron configuration you will open a terminal and type `crontab -e`, it allows you to edit cron configurations. You will open a file with an editor, it may will be `nano`. So you only have to paste the next command and let it be:

```
* * * * * php /route/to/script/check-change.php
```

And finally save set up with `ctrl+o` and close editor with `ctrl+x`.

Other posible configuration is to check webpage all days at certain hour, but you want to be notified even if it's been changed or not. It's easy too. You only have to set `check_changes` to `false` in json settings, this will make it not distinguish if there are changes or not, and if you have `notify_telegram` with `true` value, it'll notify you anyway. This is an example to be notified all day at 12:00

```
0 12 * * * php /route/to/script/check-change.php
```

And that's it. Thanks to arrive since here, I hope i could help you.
