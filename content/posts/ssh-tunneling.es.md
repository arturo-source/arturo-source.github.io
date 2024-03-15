---
title: "Qué es y cómo configurar un tunnel ssh"
slug: "tunel-ssh"
description: "Comprende los túneles SSH y configura tu primer tunel en un par de minutos."
summary: "Comprende los túneles SSH y configura tu primer tunel en un par de minutos."
date: 2024-03-14T19:08:31+01:00
draft: false
tags: ["ssh"]
---

## ¿Qué es un tunel SSH?

SSH es una aplicación para comunicarte con otros ordenadores de manera cifrada. Normalmente te conectas al servidor con `ssh user@public.site`, pero si utilizas el parámetro `-L` crearás un tunel a través del servidor.

Y, ¿qué es un tunel SSH? Pues, como un tunel de la vida real, un tunel comunica un punto A, y un punto B. El punto A es tu ordenador, y el punto B **NO** es el servidor de `public.site`, sino el sitio al que quieras llegar, digamos `private.site`. Es decir, `public.site` no es el punto B, **¡sino que es el tunel!**

## ¿Qué utilidad tiene el tunel SSH?

La funcionalidad de un tunel SSH es muy similar a la de un VPN, nos permite hacer peticiones a `private.site` simulando que somos `public.site`. Al igual que con una VPN, nuestra conexión está cifrada. Y, además, podemos saltarnos restricciones de puertos por el firewall, ya que estamos accediendo mediante un puerto SSH abierto.

## ¿Cómo se crea un tunel SSH?

El comando es muy sencillo, una vez lo entiendes:

```sh
ssh -N -L localhost:8000:private.site:80 user@public.site
```

- La opción `-N` ejecuta SSH de manera no interactiva, porque no necesitamos abrir una shell en `public.site`. Prueba a no ponerlo y lo entenderás.
- La opción `-L` es la que crea el tunel. Seguida de ella configuraremos el tunel.
- `localhost:8000` es dónde vamos a colocar el origen del tunel (el punto A por el que vamos a entrar). Prueba a quitar `localhost`, y deja solo `8000:private.site:80`, debería funcionar igual.
- `private.site:80` es donde vamos a colocar el destino del tunel (el punto B, donde queremos llegar con el tunel).
- Y `user@public.site` es lo que siempre usamos en SSH para acceder, el nombre de tu usuario, arroba, y el nombre del dominio (o IP) del servidor.

Si ahora abres un navegador, y escribes <http://localhost:8000>, deberías ver `private.site`, aunque normalmente no pudieses, ya sea por restricciones de tu país, o porque es un sitio no accesible desde una red pública.

Si lo que quieres es acceder a un puerto dentro de `public.site` que no es público, y está cerrado por el firewall, puedes hacer lo mismo, pero cambiando `private.site`, por localhost:

```sh
ssh -N -L localhost:1234:localhost:1234 user@public.site
```

Ahora deberías poder acceder al servicio en el puerto `public.site:1234` desde `localhost:1234`.
