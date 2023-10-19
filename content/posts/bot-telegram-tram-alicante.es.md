---
title: "Bot de Telegram para el Tram de Alicante"
description: "¿Cómo creé un bot de Telegram? ¿De dónde saqué la información del Tram? Explicación con vídeos."
summary: "¿Cómo creé un bot de Telegram? ¿De dónde saqué la información del Tram? Explicación con vídeos."
date: 2023-10-18T16:51:46+02:00
draft: false
---

Este es un mini cursillo que te servirá para aprender los conceptos básicos de Go (crear un proyecto, instalar un paquete, hacer peticiones HTTP, etc.). La idea es hacer un bot de Telegram que te dice cuánto tiempo queda para que llegue el siguiente Tram a tu parada. Este es un proyecto de ejemplo, que puedes ampliar, o incluso, seguir mejorando con proyectos totalmente distintos.

Si quieres usar el bot, puedes hacerlo con una cuenta de telegram desde aquí: <https://t.me/tram_alicante_bot>

Todo el código está disponible en <https://github.com/go-telegram-bot-api/telegram-bot-api>

## Las herramientas que he utilizado

Como ya he mencionado, el lenguaje de programación será Go. Además, el editor que vamos a usar es Visual Studio Code. Y el sistema operativo Arch Linux.

{{< youtube Yj7oN6uoxl4 >}}

## Cómo funciona la librería de Telegram

Como ya hemos instalado una librería de Telegram para Go, el siguiente paso es usarla, para comunicarnos con los humanos. En este vídeo verás la estructura del código para responder mensajes en Telegram de manera automática.

{{< youtube attfoJFqL_k >}}

## Ingeniería inversa para "scrapear" datos

En este vídeo verás cómo usar tu navegador para ver cómo funciona una web por dentro, en este caso la del Tram. Con esta información podrás simular que eres un navegador en tu programa en Go, y recibir la información necesaria. Como recibirás un [JSON](/posts/what-is-json.es.md), también vas a aprender como manipular el JSON y herramientas que te van a facilitar la vida.

{{< youtube ug_TCdWIeXQ >}}

## Scraping de HTML en Go ¡sin librerías

Si te quedaste hasta el final del último vídeo, sabrás que un endpoint de la aplicación NO devuelve JSON, sino HTML. Normalmente usarías una librería de web Scraping como [Colly](https://github.com/gocolly/colly), pero aquí aprenderás a hacerlo sin librerías.

{{< youtube 0oYWLILevyc >}}

## Desplegar el bot GRATIS

Existen multitud de hostings donde podrás alojar tu aplicación de manera gratuita (siempre y cuando no sea muy muy usada). Algunos de estos sitios los podéis ver en [este tweet de Midudev](https://twitter.com/midudev/status/1694681012305899943)

**fl0․com** - **render․com** - **fly․io** - **koyeb․com** - **qoddi․com** - **netlify․com** - **vercel․com**

En este vídeo pretendo usar fl0.com para que veas un ejemplo de lo sencillo que es hacer accesible tu bot, para que tus amigos lo puedan usar.

{{< youtube N8rL6RLbkNM >}}
