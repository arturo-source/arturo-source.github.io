---
title: "¿Qué es Hugo? El CMS más rápido"
slug: "hugo-el-cms-hecho-con-golang"
description: "Echa un vistazo a este CMS creado con Go, el más rápido del mundo."
summary: "Echa un vistazo a este CMS creado con Go, el más rápido del mundo."
date: 2024-01-24T14:20:32+01:00
draft: false
tags: ["mini pildoras"]
---

Hugo es un constructor de sitios web estáticos. Es increíblemente rápido, tanto construyendo el sitio (>1ms por página), como sirviendo las páginas.

HUGO no tiene un significado per se, es sólo el nombre de la herramienta. La clave de su rapidez reside en ser **estático**. Generalmente utilizarás un servidor HTTP para servir tu web (Apache HTTP server, NGINX, Caddy, etc.) para servir tu CMS en PHP (WordPress, Magento, Joomla, etc.). Incluso, puedes utilizar un servidor HTTP como proxy para tus aplicaciones web hechas con Python, NodeJS, Go, etc. Pero todas las anteriores opciones tienen en común que son **dinámicas**, porque tienen que ejecutar código en el servidor.

## ¿Cómo funciona Hugo? ¿Qué lo hace tan rápido?

Cuando construimos una web con Hugo, escribiremos el contenido con **Markdown**, compila todo el contenido en HTML, y lo enlaza. De esta manera, podemos subir el resultado a nuestro servidor HTTP favorito, y cuando un usuario solicita la página, el servidor no tiene que ejecutar ninguna lógica (conexión a una BBDD, procesar datos, etc.), eso lo hace tan rápido.

Por lo tanto, Hugo es rápido en tres diferentes aspectos:

1. Sirve rápido el contenido a los usuarios (es simple HTML).
2. Construye rápidamente el sitio (menos de 1ms por página).
3. Escribes muy rápido el artículo (Markdown es increíblemente más fácil de usar que HTML).

## Ventajas y desventajas de Hugo

Las ventajas ya las conoces: velocidad y simpleza en todos los aspectos. La desventaja es a su vez su mayor ventaja, que es **estático**. Esto significa que, por muy rápido que sea, no permite ejecutar código en el servidor. Pero esto lo hace perfecto para crear sitios que no necesiten lógica, como un portfolio, o un blog.

Otras ventajas ocultas, que lo hacen tan poderoso, son:

- Puedes crear sitios web multilingüe.
- Contiene funciones para optimizar el SEO.
- Puedes extender la funcionalidad de Markdown, utilizando los templates de Go.
- Tienes una [gran cantidad de temas](https://themes.gohugo.io/) gratuitos, o puedes construir el tuyo propio.

## Si tan fácil es, ¿cómo uso Hugo?

Por supuesto, primero tienes que [instalar hugo](https://gohugo.io/installation/). Para crear un sitio, simplemente ejecuta:

```sh
hugo new site yourwebname
```

Esto crea una carpeta llamada `yourwebname` que contiene todo lo que necesitas. Puedes acceder a ella haciendo `cd yourwebname`.

Y lo siguiente es [elegir un tema](https://themes.gohugo.io/), para este ejemplo usaré **hugo-book**.

```sh
git init
git submodule add https://github.com/alex-shpak/hugo-book themes/hugo-book
echo "theme = 'hugo-book'" >> hugo.toml
```

Y ya estás listo para escribir tu primer contenido, habitualmente se utiliza la carpeta `posts/`, pero tú puedes crearlos donde quieras, para el ejemplo usaré `blog/`.

```sh
hugo new content blog/my-first-post.md
```

Y se habrá creado el archivo `yourwebname/content/blog/my-first-post.md`, el cual es una copia de `archetypes/default.md` con información sobre el artículo que vas a escribir.

Si además necesitas modificar información global de la página web, lo podrás hacer en `yourwebname/hugo.toml` (Hugo acepta **toml**, [**yaml**](/es/posts/que-es-yaml/), y [**json**](/es/posts/que-es-json/) para la configuración). Aquí podrás modificar la configuración de Hugo, y del tema que has elegido.

Y ya sólo queda poner a funcionar la web, puedes hacerlo con `hugo server`, o `hugo server -D` si quieres ver los artículos que tengas como `draft: true`, ¡verás qué rápido va! Pero esto sólo funcionará en tu ordenador, si quieres hacerlo público (desplegar a producción), [elige tu forma favorita entre todas estas](https://gohugo.io/hosting-and-deployment/).

Para más información, consulta la [página oficial de Hugo](https://gohugo.io/).
