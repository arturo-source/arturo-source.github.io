---
title: "Qué es un túnel SSH inversos y cómo configurarlo"
slug: "tunel-ssh-inverso"
description: "Comprende qué es un túnel SSH reverso, cómo configurarlo, y cuál es la diferencia con un túnel normal."
summary: "Comprende qué es un túnel SSH reverso, cómo configurarlo, y cuál es la diferencia con un túnel normal."
date: 2024-03-15T19:54:05+01:00
draft: false
tags: ["ssh"]
---

## ¿Qué es un túnel inverso?

Un túnel inverso es una técnica que permite a tu servidor acceder a recursos de tu ordenador local. Aunque no tengas una IP pública, o estés detrás de una NAT. Además, al crear un túnel a través de SSH, los recursos locales de tu ordenador son accedidos por el servidor de manera cifrada.

Puede que lo que buscas sea crear un túnel normal, y no uno inverso, y esto lo explico [en este otro artículo](/es/posts/tunel-ssh/).

## ¿Qué diferencia hay entre un túnel SSH normal, y un túnel reverso?

Con un túnel normal puedes acceder a recursos del servidor, aunque sean privados. O usar el servidor como proxy, para acceder a recursos que no puedes desde tu ordenador.

Un tunel reverso es justo lo contrario, el servidor es el que accede a tus recursos. Un ejemplo puede ser acceder a tu Raspberry Pi desde fuera de tu casa, tendrías que ejecutar un SSH reverso desde tu Raspberry Pi a un servidor con IP pública. Entonces, podrías conectarte al servidor con IP pública por SSH, y dentro de él, conectarte a tu Raspberry Pi, usando SSH otra vez.

Incluso, también podrías utilizar tu Raspberry Pi como proxy, a través del cual el servidor se puede conectar a un servidor de archivos que tengas alojado de tu casa.

## ¿Cómo se crea un túnel inverso?

El comando es muy sencillo, una vez lo has entendido:

```sh
ssh -N -R localhost:8888:fileserver.home:80 user@server.com
```

- La opción `-N` ejecuta SSH de manera no interactiva, porque no necesitamos abrir una shell en `server.com`. Prueba a no ponerlo y lo entenderás.
- La opción `-R` es la que crea el túnel inverso. Seguida de ella configuraremos el tunel.
- `localhost:8888` es donde el servidor va a llamar, para conectar con la Raspberry. Prueba a quitar `localhost`, y deja solo `8888:fileserver.home:80`, debería funcionar igual.
- `fileserver.home:80` es donde tengo corriendo mi servidor de archivos en casa (mi router resuelve `fileserver.home` con la IP de mi servidor de archivos casero).
- Y `user@server.com` es lo que siempre usamos en SSH para acceder, el nombre de tu usuario, arroba, y el nombre del dominio (o IP) del servidor.

Si ahora accedes al servidor remoto con `ssh user@server.com`, puedes hacer ejecutar `curl localhost:8888`, verás que obtienes una respuesta de tu casa. Por supuesto, si en tu servidor tenías un proceso corriendo en el puerto 8888, usa cambia `localhost:8888` por uno que esté libre.

Si no quieres acceder a un aparato de tu casa, sino directamente a tu Raspberry Pi (por VNC, por ejemplo), ejecuta lo siguiente:

```sh
ssh -N -R localhost:8888:localhost:5901 user@server.com
```

Al cambiar `fileserver.home:80` por `localhost:5901`, le estás diciendo al servidor que puede acceder a tu servicio VNC dentro de la Raspberry Pi, a través de `localhost:8888`.
