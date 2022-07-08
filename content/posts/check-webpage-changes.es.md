---
title: "Comprobar cambios en página web"
slug: "comprobar-cambios-en-una-pagina-web"
description: "Script en PHP que puedes configurar fácilmente para que compruebe los cambios de una página web, y te notifique mediante Telegram."
summary: "Script en PHP que puedes configurar fácilmente para que compruebe los cambios de una página web, y te notifique mediante Telegram."
date: 2022-07-08T19:44:14+02:00
draft: false
---

El script del que hablaré durante este post puede ser descargado [en el siguiente enlace](https://github.com/arturo-source/check-webpage-change).

## Un script muy simple que puedes configurar desde el json

¿Alguna vez has querido estar pendiente de los cambios de un sitio web? Puede que quieras ver cómo evoluciona el precio de un producto que te interesa, ¡o a saber qué cambios quieres ver!

Con este sencillo script puedes hacerlo en tan solo unos segundos. Veamos los sencillos pasos para configurar el script. Todo lo que tienes que hacer es configurar el archivo `settings.json`.

### Requerimientos

Primero, debes tener instalado el intérprete de PHP y crontab en su computadora o su servidor.

- PHP
- crontab (presente en los ordenadores Linux)
- Editor de texto

### Variables que hay que configurar

Las variables que tendrás que cambiar son:

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

- url **(obligatorio)**
- xpaths **(obligatorio)**
- chat_id (opcional, sirve para notificar mediante Telegram)
- bot_token (opcional, sirve para notificar mediante Telegram)

Obtener `url` es muy fácil, puedes copiarla desde la parte superior del navegador. Obtener los `xpaths` es un poco más difícil, una vez que estás en la página web, debes hacer clic derecho sobre ella. Luego selecciona la opción "Inspeccionar". Luego, verás un elemento de flecha como el siguiente (izquierda):

![Selector](https://github.com/arturo-source/check-webpage-change/blob/main/images/selector.png?raw=true)

Ahora tienes que seleccionar el elemento html en la página web, haz clic izquierdo sobre él.

![HTML seleccionado](https://github.com/arturo-source/check-webpage-change/blob/main/images/html-selected.png?raw=true)

Luego, el código html estará marcado, por lo que el último paso es hacer clic derecho sobre él y seleccionar "Copiar" > "XPath"

![Copiar Xpath](https://github.com/arturo-source/check-webpage-change/blob/main/images/copy-xpath.png?raw=true)

El último paso es pegarlo en la configuración json `"Price": "/html/body/main/div[4]/......."` (lo de la izquierda es un nombre de identificación y la derecha es el xpath), y tendrás el script configurado (recuerda que puedes agregar todos los xpaths, tantos como quieras).

Pero tal vez quieras recibir una notificación cuando ocurra algún cambio, por lo que debes configurar las notificaciones.

### Configurar las notificaciones

También es muy sencillo, si alguna vez has usado Telegram. Supondré que tienes una cuenta de Telegram y un cliente para usarla (la aplicación oficial del móvil u ordenador, por ejemplo).

1. Crea un bot: Habla con @BotFather, él te guiará.
2. Copia el token del bot: Puedes pegarlo ahora en la configuración json, de lo contrario tendrás que hacerlo más tarde.
3. Habla con tu nuevo bot: Puedes hablarle directamente o crear un grupo (o canal) con tus amigos y agregar el bot allí.
4. Accede a la siguiente URL en tu navegador (no olvides cambiar \<token\> con tu token): [https://api.telegram.org/bot\<token\>/getUpdates](https://api.telegram.org/bot<token>/getUpdates)

Obtendrás un json como el siguiente:

![Obtener el ChatID](https://github.com/arturo-source/check-webpage-change/blob/main/images/get-chatid.png?raw=true)

Luego, puedes elegir el Telegram ChatID y pegarlo también en la configuración json, recuerda que `notify_telegram` tiene que ser `true` para habilitar las notificaciones. Y tendrás el script totalmente configurado. Pero ahora tienes que decidir con qué frecuencia quieres que el bot te notifique.

### Configurar el crontab

Crontab es una herramienta realmente útil que puede tener instalada en tu ordenador o servidor Linux. Te ayuda a realizar tareas recurrentes automáticamente. Y es realmente fácil de configurar, pero el primer uso puede ser confuso. Puedes acceder a [esta página para configurar crontab fácilmente] (https://crontab.guru/).
La opción más común será el domingo, a las 12:00 por ejemplo, así que escribirás `0 12 * * 0` junto al comando. Pero tal vez quieras ejecutarlo siempre que enciendas la computadora, luego escribirás `@reboot` al lado del comando.

Para abrir la configuración de cron, abra una terminal y escriba `crontab -e`, le permite editar las configuraciones de cron. Abrirá un archivo con un editor, puede ser `nano`. Así que solo tienes que pegar el siguiente comando y dejarlo así:

```
* * * * * php /route/to/script/check-change.php
```

Y finalmente guarda la configuración con `ctrl+o` y cierra el editor con `ctrl+x`.

Otra configuración posible es consultar la página web todos los días a una hora determinada, pero quieres que te avisen aunque haya cambiado o no. También es fácil. Solo tiene que establecer `check_changes` en `false` en la configuración json, esto hará que no distinga si hay cambios o no, y si tiene `notify_telegram` con valor `true`, te notificará de todos modos. Este es un ejemplo para ser notificado todos los días a las 12:00

```
0 12 * * * php /route/to/script/check-change.php
```

Y eso es todo. Gracias por llegar hasta aquí, espero poder haberte ayudado.
