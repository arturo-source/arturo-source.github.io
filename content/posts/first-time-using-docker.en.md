---
title: "First Time Using Docker"
description: "A quick tutorial, both useful if it's your first time, or if you need to refresh your knowledge of Docker."
summary: "A quick tutorial, both useful if it's your first time, or if you need to refresh your knowledge of Docker."
date: 2023-01-16T21:15:06+01:00
draft: false
---

Tired of having to search the Docker documentation every time you want to use it? Me too! So in this post we are going to review the basic knowledge, and you will understand EVERYTHING even if it is the first time you use Docker.

Just in case you don't know Docker: it is an application to **deploy OTHER APPLICATIONS**, and forget about having to configure them. It also works on all operating systems.

## How to run a Docker container

We are going to start with something simple: we are going to set up a MySQL database.

The first thing you should know is that all the best-known applications have their Docker version, you can search for them in the Docker hub. An example of them is this, which can be verified that it is official because it says "Docker Official Image". [https://hub.docker.com/_/mysql](https://hub.docker.com/_/mysql "smartCard-inline")

And you simply put the following in the terminal `docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql`

But this is a super long line, what does each thing mean?

- **run**: downloads the MySQL image (if you don't already have it), and runs the image in a container (ie runs it).
- **--name**: it is used to give the container a name, later we will see why.
- **-e**: used to assign environment variables, so we won't have to touch configuration files. In this case we tell the ROOT password of the database.
- **-d**: used to leave the container in detached mode. That is, the container is not turned off until the execution is finished (in the case of the database, until we stop it).

As a final straw, it should be clarified that docker run is equivalent to doing docker pull (download) and docker exec (run).

## Useful Docker commands

Now that I've blown the whistle on you, let's go with more useful commands. The next thing you need to know is how to see the containers that are active. With `docker ps` you will see them, and to see also the ones that are not active `docker ps -a` of all.

What we see to the left of each line is the ID of the container, we can use it to refer to it. On the other hand, on the far right you have the NAME, which is also used to refer to the container, and is assigned randomly unless you use **--name** that you have seen before.

So, it is as simple as, if you want to stop a container type `docker stop [ID]`, to restart a container `docker restart [ID]`, and if you want to delete it from your computer `docker rm [ID]`. Substituting [ID] for the id of the container or the name that you have assigned to it.

### How to get inside a container

You may want to get into a Docker container, there are many reasons to do so (for example, **create a custom image**). And since most Docker images use Linux, what you can do is open a terminal inside the container.

To do this, you are going to write in the terminal `docker exec -it [ID] bash`. Of course, changing the [ID] by the id of the container. And what does this command mean?

- **exec**: executes a command inside the container.
- **-it**: allows you to use the container **i**n**t**eractively.
- **bash**: is the command you run (opens a terminal session).

This way you can run the same commands you would run in your terminal, but inside the container you created.

## Ports and Volumes

It is essential that you understand the use of ports and volumes with Docker, because with most images you will want to use them.

To assign a port, you'll use the `-p 8080:80` option, so the port you want to expose on your computer will be the first one before the colon, and the container's listening port will be the second one. In this way we can put an HTTP server, which inside will be on port 80, listening on port 8080 on your server. Otherwise, if the HTTP server could not communicate with the outside, it would be practically useless.

And on the other hand we have the volumes, you can have many reasons to create a volume but the most important I consider is persistence. If you're running a database server, as in the MySQL example, and you don't want to lose all the contents of the database when you delete the container, you have to tell Docker where you want it to write that information on your server. The option we will use is `-v /path/your/computer:/path/in/container`, in this way the information will be stored in the path that you specify. Volumes are actually used in another way, but this is the easiest way.

## Difference Between Image and Container

This is really simple, but when you see it for the first time it can be confusing. An **image** is **the base** with which you are going to create your **container**. So if you want to set up a MySQL database server, you'll look for an **image** in the Docker Hub, and build a **container** with the database server.

## How do I normally use it?

The command that I use the most for the development of my applications is `docker run --rm -p 8080:80 -e VARIABLE=value -d image:tag`. Putting `--rm` in the command is not something that is done in production environments, but for development it will save you constantly deleting containers, as soon as the execution ends, the container is deleted. While `-p`, `-e` and `-d` I always use it because it is common to have to put a process to listen on a port, and to have to assign a password. And finally, I have not mentioned the `tag`, but if you want the same version of the image to always be downloaded, you need to assign it a tag, otherwise you will be downloaded the "latest", which may have changes that screw up your app.

To end this post, I just want to clarify that, although I have written this post in order to help the community, in reality it has also been written to remind me, every time I have to use Docker again, what is the minimum essential that I have to know so as not to die trying. So you're welcome for the help of Arturo from the future 😜.‌
