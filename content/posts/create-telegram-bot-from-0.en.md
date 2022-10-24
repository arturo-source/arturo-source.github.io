---
title: "Create Telegram Bot From scratch"
date: 2022-10-24T17:31:58+02:00
description: "First steps to create a Telegram bot from scratch. Without external dependencies."
summary: "First steps to create a Telegram bot from scratch. Without external dependencies."
draft: false
---

Some of you will know the Telegram application. It is an instant messaging application with many features, including creating your own bots. This can be very useful on different occasions, which I may present in future posts, but for now I leave this to your imagination.
‌
The only thing we need to create a bot in Telegram is:

- A way to make requests **http**.
- A **Telegram account**.

## Send messages on Telegram with a bot

Do not worry because both things are very easy to achieve, and the programming language does not matter. Therefore, to the point, this is how a message is sent by Telegram being a bot in the different languages:

### Code to make an http request with Go, PHP, JS, Python y bash

Send a text message with **Go**:

```go
func SendMessage(msg string) error {
 token := os.Getenv("TELEGRAM_TOKEN")
 chatID := os.Getenv("TELEGRAM_CHAT_ID")

 url := fmt.Sprintf("https://api.telegram.org/bot%s/sendMessage?chat_id=%s&text=%s", token, chatID, msg)
 resp, err := http.Get(url)
 if err != nil {
  return err
 }
 defer resp.Body.Close()

 return nil
}
```

Send a text message with **PHP**: [(referencia del código)](https://stackoverflow.com/a/32875341)

```php
function sendMessage($chatID, $messaggio, $token) {
    $url = "https://api.telegram.org/bot" . $token . "/sendMessage?chat_id=" . $chatID;
    $url = $url . "&text=" . urlencode($messaggio);
    $ch = curl_init();
    $optArray = array(
            CURLOPT_URL => $url,
            CURLOPT_RETURNTRANSFER => true
    );
    curl_setopt_array($ch, $optArray);
    $result = curl_exec($ch);
    curl_close($ch);
    return $result;
}
```

Send a text message with **JS** (si usas node recuerda activar el flag --experimental-fetch):

```javascript
const sendMessage = async (message) => {
    const url = `https://api.telegram.org/bot${process.env.TELEGRAM_TOKEN}/sendMessage`;
    const body = {
        chat_id: process.env.TELEGRAM_CHAT_ID,
        text: message,
    };
    const options = {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify(body),
    };
    const response = await fetch(url, options);

    return await response.json();
};
```

Send a text message with **Python**: [(referencia del código)](https://medium.com/codex/using-python-to-send-telegram-messages-in-3-simple-steps-419a8b5e5e2)

```python
import requests
TOKEN = "YOUR TELEGRAM BOT TOKEN"
chat_id = "YOUR CHAT ID"
message = "hello from your telegram bot"
url = f"https://api.telegram.org/bot{TOKEN}/sendMessage?chat_id={chat_id}&text={message}"
response = requests.get(url)
print(response.json())
```

Send a text message with **curl**: [(referencia del código)](https://gist.github.com/dideler/85de4d64f66c1966788c1b2304b9caf1)

```bash
curl -X POST \
     -H 'Content-Type: application/json' \
     -d '{"chat_id": "YOUR_CHAT_ID", "text": "This is a test from curl"}' \
     https://api.telegram.org/bot$YOUR_BOT_TOKEN/sendMessage
```

It is done in a similar way In all languages, however, to know all the options that Telegram allows us to use, [I recommend reading its documentation](https://core.telegram.org/bots/api#sendmessage).

## Crear un bot en Telegram

> Ok Arturo, we already know how to send a message, but what about the TOKEN and the CHAT_ID?

Surely you are wondering that right now, this is the second point I was talking about at the beginning. The next thing you need is to **create your bot**, and for that [you have to talk to BotFather](https://t.me/BotFather), he will guide you to create your first bot. At the end of everything, a message will appear where you will see a token, this is the TOKEN we were talking about all the time, copy it and keep it in a safe place.

Now the CHAT\_ID is missing. This parameter already depends on how complete we want to make our bot, but initially we will want to know our CHAT\_ID to start the tests, this is as simple as the previous steps. What we will have to do is make a GET request to this url: [https://api.telegram.org/bot\<token>/getUpdates](https://api.telegram.org/bot%3Ctoken%3E/getUpdates). We can do this with the curl itself `curl https://api.telegram.org/bot<token>/getUpdates` or simply by accessing that url from our browser. It's obvious but **remember you have to change \<token> to the TOKEN you just copied in the previous step**.

![Get the ChatID](https://github.com/arturo-source/check-webpage-change/blob/main/images/get-chatid.png?raw=true)

There is nothing else to do, copy these two variables in your code, or save them as environment variables (**recommended** for security), or pass them as a parameter to the function as in the PHP example. Sending a message with a bot is that simple, the rest of the logic I leave to you, you can combine it with an arduino to notify you of the humidity in the house, to notify you when your team scores a goal... limit is in your imagination.

### Bibliotecas para crear bots en Telegram

Of course, if what you want is to make a more complex bot, that sends photos, responds to messages, or other functionalities, I do not recommend that you implement the code yourself. With a simple Google search like “Telegram bot library in…” you add your favorite language, you will find thousands. I also recommend looking directly at [Telegram's own recommended libraries](https://core.telegram.org/bots/samples).
