---
title: "Buscaminas hecho con react"
slug: "buscaminas-hecho-con-react"
description: "El conocido juego buscaminas, creado con React, pruébalo ya y mira cómo ha sido hecho."
summary: "El conocido juego buscaminas, creado con React, pruébalo ya y mira cómo ha sido hecho."
date: 2023-09-06T16:42:04+02:00
draft: false
---

Esta semana comencé un proyecto para refrescar mis conocimientos sobre React. Pero como se trataba de un side-project, quería tardar lo mínimo posible.

{{< link-button name="Jugar ya" href="/minesweeper/" >}}

## ¿Cómo hice el buscaminas?

El código puedes [encontrarlo en mi github](https://github.com/arturo-source/minesweeper-react/), pero lo interesante es que no tardé nada en montar el proyecto. Para crearlo usé vite, simplemente escribiendo en la línea de comandos `npm create vite@latest`.

Elegí React como framework, y entre las opciones para compilar el proyecto y desarrollarlo utilicé SWC, puesto que es un bundler más rápido y me permite agilizar el trabajo.

En lugar de TypeScript utilicé JavaScript, por lo que mencioné anteriormente, quiero que sea un proyecto rápido, y que no se alargue demasiado tiempo.

Como Vite ya te construye todo lo necesario en el package.json, cuando empiezo a desarrollar solo ejecuto `npm run dev`. Mientras que construir el proyecto es `npm run build`.

## ¿Cómo desplegar el proyecto?

Para construir el proyecto ya sabes que simplemente ejecutando `npm run build` se te creará una carpeta llamada `./dist`. Esta carpeta contiene todo lo necesario para que puedas ejecutar la aplicación. Pero mi objetivo era tener el juego disponible en mi web, y no funcionaba sólo con arrastrar la carpeta dentro de mi web.

Si te fijas en el `index.html` que está dentro de la carpeta `./dist`, aparecen las siguientes líneas que se encargan de enlazar el CSS y JS.

```html
<script type="module" crossorigin="" src="/assets/index-3d1a63c8.js"></script>
<link rel="stylesheet" href="/assets/index-bab0855b.css" />
```

El problema era que las rutas eran absolutas porque empezaban por un slash `/`, pero si le quitas el slash del principio, el navegador entiende que tiene que utilizar una ruta absoluta, entonces dará igual si tu juego está en una subcarpeta, siempre y cuando los assets (CSS y JS) estén en la misma carpeta.

## Prueba el juego tú mismo

{{< iframe src="/minesweeper/" >}}