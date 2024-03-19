---
title: "What is and how to set up SSH Tunneling"
description: "Understand SSH tunnels and set up your first tunnel in a couple of minutes."
summary: "Understand SSH tunnels and set up your first tunnel in a couple of minutes."
date: 2024-03-14T19:08:29+01:00
draft: false
tags: ["ssh"]
---

## What is an SSH tunnel?

SSH is an application to communicate with other computers in an encrypted way. Normally you connect to the server with `ssh user@public.site`, but using the `-L` parameter will create a tunnel through the server.

And what is an SSH tunnel? Well, like a real-life tunnel, a tunnel communicates point A, and point B. Point A is your computer, and point B is **NOT** the `public.site` server, but the site you want to reach, let's say `private.site`. That is, `public.site` is not point B, **but rather the tunnel!**

If what you want is to know how to make a reverse tunnel, I explain it [in this other article](/posts/ssh-reverse-tunneling/).

## What is the use of the SSH tunnel?

The functionality of an SSH tunnel is very similar to that of a VPN, it allows us to make requests to `private.site` pretending that we are `public.site`. As with a VPN, our connection is encrypted. And, in addition, we can bypass port restrictions by the firewall, since we are accessing through an open SSH port.

## How do I create an SSH tunnel?

The command is very simple, once you understand it:

```sh
ssh -N -L localhost:8000:private.site:80 user@public.site
```

- The `-N` option runs SSH non-interactively, because we don't need to open a shell on `public.site`. Try not to put it on and you will understand.
- The `-L` option is what creates the tunnel. Followed by it we will configure the tunnel.
- `localhost:8000` is where we are going to place the origin of the tunnel (point A through which we are going to enter). Try removing `localhost`, and leaving only `8000:private.site:80`, it should work the same.
- `private.site:80` is where we are going to place the destination of the tunnel (point B, where we want to reach with the tunnel).
- And `user@public.site` is what we always use in SSH to access, the name of your user, at, and the domain name (or IP) of the server.

If you now open a browser, and type <http://localhost:8000>, you should watch `private.site`, although normally you couldn't, either due to restrictions in your country, or because it is a site not accessible from a public network .

If what you want is to access a port within `public.site` that is not public, and is closed by the firewall, you can do the same, but changing `private.site`, for localhost:

```sh
ssh -N -L localhost:1234:localhost:1234 user@public.site
```

You should now be able to access the service on port `public.site:1234` from `localhost:1234`.
