---
title: "Cómo conectarse por SSH sin usar contraseña"
slug: "conectarse-por-ssh-sin-contraseña"
description: "No vuelvas a escribir una insegura contraseña para acceder por SSH. Aprende a usar claves públicas-privadas."
summary: "No vuelvas a escribir una insegura contraseña para acceder por SSH. Aprende a usar claves públicas-privadas."
date: 2024-03-13T12:26:20+01:00
draft: false
tags: ["ssh"]
---

Para conectarte a un servidor, normalmente usarías `ssh user@domain.com`, y para copiar un archivo `scp file.txt user@domain.com:/home/user/`. Después de ejecutar el comando, te pedirá la contraseña de `user`, pero esto es poco seguro, y pesado. La forma correcta es utilizar un par de claves pública-privada, y de esa forma no tendrás que volver a ingresar la contraseña.

## Generar claves SSH

El primer paso es abrir tu terminal y escribir `ssh-keygen`. Este comando acepta algunos comandos de configuración como `ssh-keygen -t rsa` para elegir el sistema de cifrado (puedes elegir entre dsa, ecdsa, ecdsa-sk, ed25519, ed25519-sk, rsa). Te aparecerá el siguiente prompt:

```sh
Generating public/private rsa key pair.
Enter file in which to save the key (/home/arturo/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/arturo/.ssh/id_rsa
Your public key has been saved in /home/arturo/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:xJM27ZnRIMl/OSWA1B8H9cPDN4lSKGa8QKXs3VA3pmY arturo@localhost
The key's randomart image is:
+---[RSA 3072]----+
|      .+==oooB.  |
|      .o=Oo+B B..|
|       o@o=E.B Bo|
|      .o.=*+*   =|
|       .S.+o .   |
|                 |
|                 |
|                 |
|                 |
+----[SHA256]-----+
```

El "randomart image" variará, ya que la clave que se ha generado es distinta cada vez que se ejecuta. Puedes cambiar los valores del prompt, como la ruta donde se guarda la clave. En este caso, mis claves pública y privada han sido almacenadas en `/home/arturo/.ssh/` (abreviado `~/.ssh/`), con el nombre de `id_rsa.pub` y `id_rsa`.

## Logearte en el servidor sin usar contraseña

El segundo (y último) paso es hacer que el servidor te pueda identificar. Para ello, tendrás que copiar tu clave pública `~/.ssh/id_rsa.pub` en el servidor. La forma más sencilla de hacerlo es `ssh-copy-id -i ~/.ssh/id_rsa.pub user@domain.com`. Te aparecerá el siguiente prompt en el que tienes que poner la contraseña de `user`:

```sh
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/arturo/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@172.21.0.2's password: 
```

Simplemente introduce la contraseña de `user` y ya podrás acceder al servidor simplemente con `ssh user@domain.com`, sin tener que escribir la contraseña.

### Alternativa sin ssh-copy-id

Una forma alternativa, si no quieres (o no puedes) usar `ssh-copy-id`, es copiar tu clave pública manualmente en el servidor. Para ello, tendrás que copiar el contenido de `~/.ssh/id_rsa.pub`, para verlo ábrelo, o ejecuta `cat ~/.ssh/id_rsa.pub`:

```txt
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDCY+hLD34jAoCrin5sRN1mclVxhaykt0pRdvsLxFJxypkwALqb18nx3ryYNNKygWnpmR4hmD19wHGVZZi7nUrgUcMbES03RvOdigIasfgjGR/dijs3b+HhEZ+iyJJjkOQumEm+0en4lKsh8nWecrb6FsMLRXBvPsB5lhr4bu15dP7/Ui/55LRyP/6MpszhQufS6xlaWXa2lY1dRPY/XDuWE+datnsZAPqn6KM2TQOQAvo1IPj3lwShMLuyzEfwSMQKBM+y+ltu0k/ogra1pk+PRcGEqEnjkojTmS/tigOoa9u+Zo4CDBCsTjZViaFI6aRs/+FHmfrmlWz91J1dleMhp8feIlsfhnwAFRvRvd6yYzY8N10MnixwRjw1cyNDwJgBojmjfHsz879KtkF4lQ934e1nXIrIPos7thT7tx0e4TEpmNZiB5XpIPZe8AERzyYTNqFN9pOVRIlelakjATKxrjiiNZTVJrbcMA7yIXb8BgW0kyJb1AZgSUFqcpOsLQE= arturo@localhost
```

Entonces, accede al servidor, puede que tengas una forma de acceder sin SSH, porque algunos proveedores de VPS te permitirán acceder de otras formas. Sino, simplemente `ssh user@domain.com`. Abre el archivo `/home/user/.ssh/authorized_keys` (abreviado `~/.ssh/authorized_keys`), o créalo si no existe, ejecutando `nano ~/.ssh/authorized_keys`.

Por último, pega el contenido que acabas de copiar. Ya deberías poder acceder al servidor con `ssh user@domain.com` desde tu máquina.

## Añadir más seguridad

Por lo general, no querrás que los usuarios accedan a tu servidor `domain.com` mediante SSH con contraseña. Esto podría hacer que sufras ataques SSH para intentar vulnerar el servidor. Para ello simplemente abre el archivo con `nano /etc/ssh/sshd_config`, busca la línea donde ponga `PasswordAuthentication`, quitas la almohadilla, y escribes `PasswordAuthentication no`.
