---
title: "Crea un bot de Telegram desde 0"
date: 2022-10-24T17:31:52+02:00
slug: "crear-bot-de-telegram"
description: "Programa tu primer bot de Telegram sin ninguna dependencia, con puro código."
summary: "Programa tu primer bot de Telegram sin ninguna dependencia, con puro código."
draft: false
---

Algunos de vosotros conoceréis la aplicación Telegram. Se trata de una aplicación de mensajería instantánea con muchas funcionalidades, entre ellas, crear tus propios bots. Esto puede resultar muy útil en distintas ocasiones, que es posible que presente en futuros post, pero de momento esto lo dejo a vuestra imaginación.
‌
Lo único que necesitamos para crear un bot en Telegram es:

- Una forma de hacer peticiones **http**.
- Una **cuenta de Telegram**.

## Enviar mensajes en Telegram con un bot

No os preocupéis porque ambas cosas son muy sencillas de conseguir, y da igual el lenguaje de programación. Por lo tanto, al grano, así se envía un mensaje por Telegram siendo un bot en los distintos lenguajes:

### Código para hacer una petición http con Go, PHP, JS, Python y bash

Enviar un mensaje de texto con **Go**:

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

Enviar un mensaje de texto con **PHP**: [(referencia del código)](https://stackoverflow.com/a/32875341)

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

Enviar un mensaje de texto con **JS** (si usas node recuerda activar el flag --experimental-fetch):

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

Enviar un mensaje de texto con **Python**: [(referencia del código)](https://medium.com/codex/using-python-to-send-telegram-messages-in-3-simple-steps-419a8b5e5e2)

```python
import requests
TOKEN = "YOUR TELEGRAM BOT TOKEN"
chat_id = "YOUR CHAT ID"
message = "hello from your telegram bot"
url = f"https://api.telegram.org/bot{TOKEN}/sendMessage?chat_id={chat_id}&text={message}"
response = requests.get(url)
print(response.json())
```

Enviar un mensaje de texto con **curl**: [(referencia del código)](https://gist.github.com/dideler/85de4d64f66c1966788c1b2304b9caf1)

```bash
curl -X POST \
     -H 'Content-Type: application/json' \
     -d '{"chat_id": "YOUR_CHAT_ID", "text": "This is a test from curl"}' \
     https://api.telegram.org/bot$YOUR_BOT_TOKEN/sendMessage
```

En todos los lenguajes se hace de forma similar, sin embargo, para conocer todas las opciones que nos permite usar Telegram, [recomiendo leer su documentación](https://core.telegram.org/bots/api#sendmessage).

## Crear un bot en Telegram

> Vale Arturo, ya sabemos cómo se manda un mensaje pero, ¿qué son el TOKEN y el CHAT_ID?

Seguro que te estás preguntando eso ahora mismo, este es el segundo punto del que hablaba al principio. Lo siguiente que necesitas es **crear tu bot**, y para ello [tienes que hablarle a BotFather](https://t.me/BotFather), él te guiará para crear tu primer bot. Al final del todo te aparecerá un mensaje donde verás un token, este es el TOKEN del que hablábamos todo el rato, cópialo y guárdalo en un lugar seguro.

Ahora falta el CHAT\_ID. Este parámetro ya depende de lo completo que queramos hacer nuestro bot, pero inicialmente querremos saber nuestro CHAT\_ID para comenzar las pruebas, esto es tan sencillo como los pasos anteriores. Lo que tendremos que hacer es una petición GET a esta url: [https://api.telegram.org/bot\<token>/getUpdates](https://api.telegram.org/bot%3Ctoken%3E/getUpdates). Esto lo podemos hacer con el propio curl `curl https://api.telegram.org/bot<token>/getUpdates` o simplemente accediendo a esa url desde nuestro navegador. Resulta obvio pero **recuerdo que tienes que cambiar \<token> por el TOKEN que acabas de copiar en el paso anterior**.

![Obtener el ChatID](https://github.com/arturo-source/check-webpage-change/blob/main/images/get-chatid.png?raw=true)

Ya no hay nada más que hacer, copia estas dos variables en tu código, o guárdalas como variables de entorno (**recomendable** por seguridad), o pásalas como parámetro a la función como en el ejemplo en PHP. Enviar un mensaje con un bot es así de simple, el resto de la lógica te la dejo a ti, puedes combinarla con un arduino para que te avise de la humedad de la casa, para que te avise cuando tu equipo marque un gol… el límite está en tu imaginación.

### Bibliotecas para crear bots en Telegram

Por supuesto, si lo que quieres es hacer un bot más complejo, que envíe fotos, que responda a mensajes, u otras funcionalidades, no te recomiendo que implementes tú mismo el código. Con una simple búsqueda en Google como “Telegram bot library in …“ añades tu lenguaje favorito, encontrarás miles. También te recomiendo mirar directamente las [librerías recomendadas por el propio Telegram](https://core.telegram.org/bots/samples).
