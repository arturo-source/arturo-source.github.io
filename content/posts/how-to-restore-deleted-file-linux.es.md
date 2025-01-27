---
title: "¿Cómo restaurar un fichero borrado en Linux?"
slug: "restaurar-fichero-borrado-linux"
description: "Has borrado un fichero un fichero desde la terminal, y no sabes cómo recuperarlo... Menos mal que ahora te voy a contar el truco definitivo."
summary: "Has borrado un fichero un fichero desde la terminal, y no sabes cómo recuperarlo... Menos mal que ahora te voy a contar el truco definitivo."
date: 2025-01-27T18:27:57+01:00
draft: false
tags: ["linux"]
---

Seguro que te ha sucedido alguna vez que has ejecutado `rm file.txt`, pero en realidad no querías borrar ese fichero... El problema es que la terminal no tiene papelera; entonces, el fichero no se puede recuperar, o a lo mejor sí...

Lo primero que tienes que saber es que cuando borras un fichero, los 0s y 1s siguen en el disco duro, o sea que no todo está perdido.

## Métodos para recuperar un fichero borrado

Creo que es obvio, pero si lo has borrado de manera convencional, **¿has probado a recuperarlo de la papelera?**

Si lo has borrado usando el comando `rm`, o incluso si estaba en la papelera y has vaciado la papelera, hay software libre especializado para esto, como **foremost**, **ext4magic** o **extundelete**.

Pero el método que te voy a enseñar a continuación es el mejor sin duda, por su facilidad, sencillez y elegancia. ¿Has usado alguna vez la herramienta `grep`? Si la respuesta es 'sí', ya tienes medio trabajo hecho.

## Recuperar archivo borrado usando grep

Por si no la conoces, `grep` es una herramienta para encontrar patrones. Reproduce el siguiente ejemplo:

```bash
cat <<EOF >> names.txt
Charlie
Arthur
John
Oliver
EOF
grep 'r' names.txt
```

Habrás creado un archivo llamado `names.txt`, y con grep has listado todas las líneas que contienen la `r`. Al comando grep le podemos añadir opciones, por ejemplo `-A` (after) y `-B` (before):

```bash
grep -A 1 'u' names.txt
```

Con esta opción habrás listado los nombres que contienen la `u` y el que haya a continuación.

Y ahora viene lo que no te esperas de grep: no solo puedes buscar en archivos, también **puedes buscar en dispositivos**, por ejemplo, **TODO tu disco duro**, o tu USB. Como he mencionado al principio, aunque hayas borrado tu archivo, los bytes siguen estando en el disco duro, pero tu sistema operativo no sabe dónde.

Si el fichero estaba en el disco duro, busca en cuál, en Linux los discos están en la ruta `/dev/`. Si es un SSD, el nombre comenzará por `/dev/nvme...`; mientras que si es un HDD será `/dev/sd...`. Y un USB probablemente será un nombre parecido al del HDD.

¿Ya has encontrado el dispositivo? Ahora tienes que acordarte de algo que pusiera en el archivo, yo usaré "some content on the file":

```bash
grep -a -A 200 -B 100 'some content on the file' /dev/nvme0n1
```

Después de varios minutos (depende de la velocidad de lectura del dispositivo), ¡saldrá en tu terminal el contenido del fichero! 100 líneas por arriba y 200 por debajo del patrón que has escrito.

## ¿Cómo borrar un fichero por completo?

Ahora imagino que, como cuando yo descubrí esto, te estarás preguntando: entonces, ¿¡cómo borro un fichero de verdad!? La respuesta es fácil: **llenándolo de ceros**.

Te mostraré dos comandos de Linux que puedes usar:

```bash
dd if=/dev/zero of=file.txt bs=1M
rm file.txt
```

Y si en lugar de ceros, quieres escribir números aleatorios:

```bash
shred -u file.txt
```
