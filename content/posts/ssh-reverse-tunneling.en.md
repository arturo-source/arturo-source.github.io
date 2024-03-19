---
title: "What is SSH Reverse Tunneling, and how to set it up"
description: "Understand what a reverse SSH tunnel is, how to configure it, and what the difference is from a normal tunnel."
summary: "Understand what a reverse SSH tunnel is, how to configure it, and what the difference is from a normal tunnel."
date: 2024-03-15T19:54:02+01:00
draft: false
tags: ["ssh"]
---

## What is a reverse tunnel?

A reverse tunnel is a technique that allows your server to access resources on your local computer. Even if you don't have a public IP, or you are behind a NAT. Additionally, by creating a tunnel via SSH, your computer's local resources are accessed by the server in an encrypted manner.

Maybe what you are looking for is to create a normal tunnel, and not a reverse one, and I explain this [in this other article](/posts/ssh-tunneling/).

## What is the difference between a normal SSH tunnel and a reverse tunnel?

With a normal tunnel you can access server resources, even if they are private. Or use the server as a proxy, to access resources that you cannot from your computer.

A reverse tunnel is just the opposite, the server is the one that accesses your resources. An example could be accessing your Raspberry Pi from outside your home, you would have to run a reverse SSH from your Raspberry Pi to a server with a public IP. Then, you could connect to the server with public IP via SSH, and within it, connect to your Raspberry Pi, using SSH again.

You could even use your Raspberry Pi as a proxy, through which the server can connect to a file server that you have hosted at home.

## How do you create a reverse tunnel?

The command is very simple, once you understand it:

```sh
ssh -N -R localhost:8888:fileserver.home:80 user@server.com
```

- The `-N` option runs SSH non-interactively, because we don't need to open a shell on `server.com`. Try not to put it on and you will understand.
- The `-R` option is what creates the reverse tunnel. Followed by it we will configure the tunnel.
- `localhost:8888` is where the server will call, to connect to the Raspberry. Try removing `localhost`, and leaving only `8888:fileserver.home:80`, it should work the same.
- `fileserver.home:80` is where I have my home file server running (my router resolves `fileserver.home` to the IP of my home file server).
- And `user@server.com` is what we always use in SSH to access, the name of your user, at, and the domain name (or IP) of the server.

If you now access the remote server with `ssh user@server.com`, you can run `curl localhost:8888`, you will see that you get a response from home. Of course, if you had a process running on port 8888 on your server, use change `localhost:8888` to one that is free.

If you don't want to access a device in your home, but directly to your Raspberry Pi (via VNC, for example), run the following:

```sh
ssh -N -R localhost:8888:localhost:5901 user@server.com
```

By changing `fileserver.home:80` to `localhost:5901`, you are telling the server that it can access your VNC service within the Raspberry Pi, through `localhost:8888`.
