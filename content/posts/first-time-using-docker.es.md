---
title: "Todo lo que necesitas saber para empezar a usar Docker"
slug: "primera-vez-usando-docker"
description: "Un rápido tutorial, útil tanto si es la primera vez, como si necesitas refrescar tus conocimientos de Docker."
summary: "Un rápido tutorial, útil tanto si es la primera vez, como si necesitas refrescar tus conocimientos de Docker."
date: 2023-01-16T21:15:10+01:00
draft: false
---

¿Cansado de tener que buscar la documentación de Docker cada vez que quieres utilizarlo? ¡Yo también! Así que en este post vamos a repasar los conocimientos básicos, y vas a entenderlo TODO aunque sea la primera vez que usas Docker.

Por si acaso no conoces Docker: es una aplicación para **desplegar OTRAS APLICACIONES**, y olvidarte de tener que configurarlas. Además funciona en todos los sistemas operativos.

## Cómo correr un contenedor de Docker

Vamos a empezar por algo sencillito: vamos a montar una base de datos MySQL.

Lo primero que debes de saber es que todas las aplicaciones más conocidas tienen su versión de Docker, puedes buscarlas en el hub de Docker. Un ejemplo de ellas es esta, que se puede verificar que es oficial porque pone “Docker Official Image“. [https://hub.docker.com/_/mysql](https://hub.docker.com/_/mysql "smartCard-inline")

Y simplemente pones lo siguiente en la terminal `docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql`

Pero esto es una línea súper larga, ¿Qué significa cada cosa?

- **run**: descarga la imagen de MySQL (si no la tienes ya), y ejecuta la imagen en un contenedor (es decir, la corre).
- **--name**: sirve para ponerle un nombre al contenedor, luego veremos para qué.
- **-e**: sirve para asignar variables de entorno, así no tendremos que tocar ficheros de configuración. En este caso le decimos la contraseña de ROOT de la base de datos.
- **-d**: sirve para dejar el contenedor en modo detached. Es decir, el contenedor no se apaga hasta que no termina la ejecución (en el caso de la BBDD, hasta que no la paremos).

Cómo puntilla final, cabe aclarar que docker run es equivalente a hacer docker pull (descarga) y docker exec (corre).

## Comandos útiles de Docker

Ahora que ya te he soltado el tostón, vamos con más comandos útiles. Lo siguiente que tienes que saber es cómo ver los contenedores que están activos. Con `docker ps` los verás, y para ver también los que no están activos `docker ps -a` de all.

Lo que vemos a la izquierda de cada línea es el ID del contenedor, lo podemos utilizar para referirnos a él. Por otro lado, a la derecha del todo tienes el NAME, que sirve también para referirte al contenedor, y se asigna de manera aleatoria a no ser que utilices **--name** que has visto antes.

Por lo tanto, es tan sencillo como, si quieres parar un contenedor escribas `docker stop [ID]`, para reiniciar un contenedor `docker restart [ID]`, y si quieres borrarlo de tu ordenador `docker rm [ID]`. Sustituyendo [ID] por el id del contenedor o el nombre que le hayas asignado.

### Cómo entrar dentro de un contenedor

Puede que quieras entrar en un contenedor de Docker, hay muchos motivos para hacerlo (por ejemplo, **crear una imagen** personalizada). Y como la mayoría de imágenes de Docker utilizan Linux, lo que puedes hacer es abrir una terminal dentro del contenedor.

Para ello vas a escribir en la terminal `docker exec -it [ID] bash`. Por supuesto, cambiando el [ID] por el id del contenedor. ¿Y qué significa este comando?

- **exec**: ejecuta un comando dentro del contenedor.
- **-it**: te permite usar el contenedor de forma **i**n**t**eractiva.
- **bash**: es el comando que ejecutas (abre una sesión de la terminal).

De esta forma puedes ejecutar los mismos comandos que ejecutarías en tu terminal, pero dentro del contenedor que has creado.

## Puertos y Volúmenes

Es esencial que conozcas el uso de los puertos y los volúmenes con Docker, porque con la mayoría de imágenes querrás utilizarlos.

Para asignar un puerto, vas a utilizar la opción `-p 8080:80`, de forma que el puerto que quieres exponer en tu ordenador será el primero antes de los dos puntos, y el puerto de escucha del contenedor será el segundo. De esta manera podremos poner un servidor HTTP, que dentro estará en el puerto 80, escuchando en el puerto 8080 en tu servidor. Sino, si el servidor HTTP no se pudiera comunicar con el exterior, no serviría para prácticamente nada.

Y por otro lado tenemos los volúmenes, puedes tener muchos motivos para crear un volumen pero el más importante yo considero que es la persistencia. Si estás corriendo un servidor de base de datos, como en el ejemplo con MySQL, y no quieres perder todo el contenido de la base de datos al borrar el contenedor, tienes que decirle a Docker dónde quieres que escriba esa información en tu servidor. La opción que usaremos es `-v /ruta/tu/ordenador:/ruta/en/contenedor`, de esta forma se almacenará la información en la ruta que especifiques. Realmente los volúmenes se suelen utilizar de otra manera, pero esta es la forma más sencilla.

## Diferencia entre imagen y contenedor

Esto es realmente sencillo, pero cuando lo ves por primera vez puede llegar a confundir. Una **imagen** es **la base** con la que vas a crear tu **contenedor**. De tal manera que si quieres montar un servidor de base de datos MySQL, buscarás una **imagen** en el Hub de Docker, y generarás un **contenedor** con el servidor de base de datos.

## Cómo lo uso yo normalmente

El comando que más suelo utilizar para el desarrollo de mis aplicaciones es `docker run --rm -p 8080:80 -e VARIABLE=valor -d imagen:tag`. Poner `--rm` en el comando no es algo que se haga en entornos de producción, pero para el desarrollo te va a ahorrar el estar borrando contenedores constantemente, nada más finalizar la ejecución, se borra el contenedor. Mientras que `-p`, `-e` y `-d` lo uso siempre porque es común tener que poner un proceso a la escucha en un puerto, y tener que asignar una contraseña. Y por último, el `tag` no lo he mencionado, pero si quieres que siempre se te descargue la misma versión de la imagen, es necesario que le asignes un tag, sino se te descargará el "latest", que puede tener cambios que fastidien tu aplicación.

Para finalizar este post, sólo quiero aclarar que, aunque este post lo he escrito con la finalidad de ayudar a la comunidad, en realidad también ha sido escrito para recordarme a mí, cada vez que tengo que volver a usar Docker, qué es lo mínimo indispensable que tengo que saber para no morir en el intento. Así que de nada por la ayuda Arturo del futuro 😜.‌
