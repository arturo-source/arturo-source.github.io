---
title: "5 trucos para que tu frontend cargue 12 veces más rápido"
slug: "frontend-rapido-5-trucos"
description: "Cinco optimizaciones sencillas que puedes aplicar hoy en tu proyecto para pasar de 13 MB y 15 segundos de carga a 175 KB y 1 segundo."
summary: "Cinco optimizaciones sencillas que puedes aplicar hoy en tu proyecto para pasar de 13 MB y 15 segundos de carga a 175 KB y 1 segundo."
date: 2026-11-03T18:12:41+01:00
draft: false
tags: ["frontend", "web"]
---

{{< youtube eREOIdKyqmA >}}

Voy a enseñarte cinco trucos que usamos los ingenieros frontend para que una web cargue muy rápido, y que son muy fáciles de aplicar en tu proyecto actual. Para probarlos he usado una aplicación de ejemplo muy sencilla (una tienda con unos doce productos, una gráfica, opiniones y un buscador) que pasó de **13 MB y 15 segundos** de carga en una red 4G normal y corriente, a **175 KB y 1 segundo**. Con las mismas funcionalidades.

## Cómo medir: Chrome DevTools y Lighthouse

Antes de optimizar nada hay que medir. Abre las Chrome DevTools (con `F12`, o como yo, con `Ctrl + Shift + I`) y ve a la pestaña **Network**. Ahí tienes que hacer dos cosas:

1. Marcar **Disable cache**, porque queremos simular a un usuario que entra por primera vez.
2. En el desplegable de *throttling*, elegir **Fast 4G**, para simular un móvil con red 4G.

Ahora recarga la página y verás todo lo que se descarga: cuántos megas se transfieren, cuánto tarda en ser visible (el número azul, `DOMContentLoaded`), cuánto tarda en cargar todo, y puedes filtrar por tipo (imágenes, fuentes, JavaScript, CSS, y `Fetch/XHR`, que son las llamadas desde JavaScript al backend).

Además, Google nos da una herramienta llamada **Lighthouse** (también en las DevTools), que te puntúa la web con las métricas que Google usa para decidir si tu web es buena o no. La versión inicial de mi aplicación sacaba **41 puntos en móvil y 74 en escritorio**. Muy malo, esperaríamos tener 100.

## 1. Lazy loading y code splitting

Cuando un usuario entra por primera vez, no tiene por qué cargar todo, solo lo que ve. En mi aplicación, por ejemplo, las opiniones no se ven hasta que pulsas el botón de verlas. Entonces, ¿para qué descargar su JavaScript y sus datos al principio?

Con **code splitting** divides tu JavaScript en trozos, y con **lazy loading** solo descargas cada trozo cuando hace falta. Al pulsar el botón se descargan dos ficheros de JavaScript y se hace la llamada al backend que devuelve las reviews.

Lo que hemos hecho es diferir lo que no se ve inicialmente, para que la página cargue más rápido. Con esto pasamos a **59 en móvil y 92 en escritorio**. Mejora, pero la aplicación sigue midiendo prácticamente lo mismo (12,8 MB frente a 13,5 MB), así que hay algo más que podemos hacer.

## 2. Formatos de imagen modernos

En la pestaña Network, si filtras por imágenes verás que ocupaban **11 MB**. Es el sitio más claro donde optimizar. Las imágenes eran JPG, un formato de compresión antiguo. Ahora existen formatos como **WebP** y **AVIF**, que comprimen muchísimo más sin perder prácticamente calidad.

Lo más sencillo es convertir la imagen y cambiar el `src` de tu etiqueta `img`. Pero si quieres mantener compatibilidad, porque algún cliente tuyo solo sabe renderizar JPG, usa la etiqueta `picture`:

```html
<picture>
  <source srcset="producto.avif" type="image/avif" />
  <source srcset="producto.webp" type="image/webp" />
  <img src="producto.jpg" alt="Producto" />
</picture>
```

El navegador elige la primera que soporte: si no sabe AVIF, prueba con WebP, y si tampoco, usa el JPG de siempre. Con esto la aplicación pasó de 13 MB a **1,3 MB**.

Lo curioso es que en Lighthouse apenas cambió: 60 en móvil y 95 en escritorio (antes 59 y 92). Pero tú mismo lo ves, las imágenes cargan mucho más rápido, y para un usuario es mucho más satisfactorio ver la imagen al instante que esperar 10 segundos. **Las métricas no lo son todo**, y hay que ir al detalle.

## 3. No bloquear el renderizado

A Google no le gusta que bloquees la página mientras se carga. Aquí hay dos cosas que puedes hacer:

- **Diferir los scripts que no son necesarios ahora mismo.** En mi caso, `lodash` (una librería externa) y el *analytics* (que es código mío). Sin diferirlos, hasta que no terminan de descargarse no empieza a hacer lo demás. Con ellos diferidos, se cargan después, cuando la página ya es visible.
- **Dar prioridad máxima a la imagen principal.** La imagen del *hero* es la primera que necesita la aplicación, así que le ponemos `fetchpriority="high"` para que el navegador no la cargue después de renderizar todo, sino a la vez, porque es importante.

```html
<script src="analytics.js" defer></script>
<img src="hero.avif" fetchpriority="high" alt="Hero" />
```

Esto sube a **72 en móvil y 97 en escritorio**. No es tan decisivo como el truco anterior, no se siente que la aplicación haya mejorado muchísimo, pero Google sí lo aprecia, y no pierdes nada por hacerlo.

## 4. Reducir las peticiones de red

La regla general es que **menos peticiones es mejor**. Si pudieses devolver la aplicación ya renderizada sin hacer ninguna petición más, para Google sería perfecto. No es una regla absoluta, pero sirve de guía. Antes teníamos 52 requests y ahora tenemos casi la mitad. ¿Qué cambié?

- **Una sola llamada para los productos.** Antes pedía los productos uno por uno; ahora el backend devuelve los doce de golpe. Si tu backend lo permite, mejor (eso sí, lo normal es que estén paginados).
- **SVGs dentro del HTML.** Antes había ocho requests de SVG, y ahora ninguna porque están incrustados directamente en el HTML. Puede haber motivos para pedirlos aparte, pero si los incrustas tendrás mejor nota.
- **Quitar librerías innecesarias.** Estaba usando `lodash` (y algo parecido a `moment` para tratar fechas). Muchas de sus funciones llevan 5 o 6 años en JavaScript nativo, así que probablemente ni las necesites. Y si usas una función como `debounce` (que no hace la petición hasta que pasan unos milisegundos sin que escribas), no está en JavaScript nativo, pero es muy fácil de implementar. `lodash` ocupa medio mega; mi implementación, **0,6 KB**.

Con esto ya llegamos a **100 puntos en móvil y 100 en escritorio**.

## 5. Compresión y caché agresiva

Ya tenemos 100 puntos, ¿qué más podemos mejorar? Pues que Lighthouse mide la **primera carga**, pero la gente que usa tu aplicación entra una vez al día, o cinco veces cada hora, y para eso necesitamos otras optimizaciones. Lo anterior sirve para la primera carga; esto, para el día a día.

### Compresión

Si pones el ratón sobre un recurso en la pestaña Network, verás dos tamaños: lo transferido y el tamaño real del recurso. Mi HTML tenía 7,5 KB pero se transfirió con 2,6 KB. La librería de gráficas pasó de 200 KB a 60 KB. Eso es compresión.

El navegador envía en sus cabeceras algo como `Accept-Encoding: gzip, br, zstd`, es decir, "acepto que me lo envíes comprimido en gzip, o en Brotli o Zstandard", que son dos tipos de compresión modernos, mejores que gzip. En mi caso lo envío en **Brotli**, que para HTML y JavaScript (en general, todo lo que sea texto) suele ser la mejor opción.

### Caché

Hasta ahora siempre había marcado *Disable cache*. Si lo desmarco y recargo, verás que los recursos aparecen como `memory cache`: ni siquiera se hace la petición al servidor. Si miraras los logs de tu servidor (Nginx, Caddy, Apache...), no habría ninguna petición para la imagen ni para el JSON. Está todo en la caché del cliente.

Esto lo conseguimos con la cabecera `Cache-Control`. En la versión anterior yo obligaba al navegador a no cachear nunca, y lo que pasaba es que el navegador le preguntaba al servidor "oye, tengo esto con este identificador (el `ETag`)", y el servidor le respondía con un `304 Not Modified`: "no ha cambiado, no te lo descargues otra vez". No gasta muchos megas, pero hace un viaje de ida y vuelta al servidor, y la aplicación es más lenta.

La técnica es ponerle al fichero un identificador único en el nombre, entre el nombre y la extensión (algo como `app.3f9a2c.js`). Esto lo hacen casi todos los *bundlers* (Webpack, Vite...): minimizan tu código y le añaden un hash que cambia cada vez que cambia el fichero. Así puedes cachear de forma muy agresiva, por ejemplo con un año de vida:

```
Cache-Control: public, max-age=31536000, immutable
```

Le estás diciendo al navegador "este fichero no va a cambiar nunca, no me lo pidas". Si cambia, tendrá otro nombre y se descargará de nuevo. Es lo mismo que puedes hacer con las imágenes. Así ahorras recursos en tu servidor, y el tiempo de carga a tu usuario la segunda y la tercera vez que entra, manteniendo el 100 % de Lighthouse.

## Resumen

| Optimización | Móvil | Escritorio |
| --- | --- | --- |
| Versión inicial | 41 | 74 |
| 1. Lazy loading y code splitting | 59 | 92 |
| 2. Formatos de imagen modernos | 60 | 95 |
| 3. Diferir scripts y priorizar la imagen principal | 72 | 97 |
| 4. Reducir peticiones de red | 100 | 100 |
| 5. Compresión y caché agresiva | 100 | 100 |

Estos son los cinco trucos fundamentales que puedes aplicar hoy mismo para que tu aplicación cargue más rápido y tus usuarios estén contentos. Si no quieres hacerlo a mano, siempre puedes pasarle el vídeo a una IA y decirle "aplícame estos cinco puntos en mi proyecto".
