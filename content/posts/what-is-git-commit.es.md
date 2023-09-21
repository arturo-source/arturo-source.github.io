---
title: "¿Qué es git commit?"
slug: "que-es-git-commit"
description: "Conceptos básicos de git en un minuto."
summary: "Conceptos básicos de git en un minuto."
date: 2023-09-14T15:59:23+02:00
draft: false
tags: ["mini pildoras"]
---

Hacer un commit en `git` significa guardar los cambios de tus archivos en tu repositorio local. Siempre querrás usar un mensaje descriptivo para registrar la evolución de tu proyecto.

Hacer un commit en git es uno de los conceptos fundamentales en el **control de versiones**. Cuando haces un commit, estás creando un punto de control en la historia de tu proyecto. Cada commit contiene una instantánea de los archivos en ese momento, junto con un mensaje descriptivo que explica los cambios realizados.

## Pasos para guardar tus archivos

Existe un montón de plataformas que integran `git` (GitHub, GitLab, Bitbucket, Gitea). Sin embargo, primero voy a darte los conocimientos básicos para trabajar desde tu PC.

En este caso hablaré de un proyecto de programación. Normalmente, tendrás varios archivos y carpetas. Cuando quieres guardar tu progreso, puede que hayas cambiado varios archivos, pero sólo quieras registrar los cambios de unos pocos. Con `.` registrarás los cambios de la carpeta en la que estás trabajando.

```bash
git add .
```

Y si solo quieres registrar el progreso de tres ficheros, lo harás de la siguiente manera.

```bash
git add file1 file2 folder1/file1
```

El segundo paso, una vez registrados los cambios que quieres guardar, es confirmar los cambios, lo que llamaré 'hacer un commit'. Es muy importante colocar un mensaje que represente los cambios desde el último commit. Esto lo podemos hacer de dos formas.

La primera, para mí, la más sencilla. Escribir el comando y te aparecerá un resumen de los cambios en tu ventana. Ya podrás escribir el comentario.

```bash
git commit
```

La segunda, más rápida. Añadir `-m` al comando para escribir el mensaje directamente.

```bash
git commit -m "Info about changes"
```

Esto es todo lo que necesitas para mantener un control de los cambios en tu aplicación. Para ver todos los puntos de control, basta con escribir `git log`.

## Guardar tu código en la nube

Existe un último comando fundamental que quieres conocer. Te servirá para guardar tu código, y todos tus cambios en la nube. Además, cuando seas profesional, te permitirá trabajar con tus compañeros, sobre el mismo código.

```bash
git push
```

Push significa subir tus cambios. Lo harás después de haber confirmado tus cambios con tu 'commit', después de varios 'commit'. Pero, ¿y si tus compañeros se quieren descargar tus cambios?

```bash
git pull
```

Pull significa descargar los cambios. Querrás hacerlo siempre que quieras tener tus cambios sincronizados con el resto del equipo.
