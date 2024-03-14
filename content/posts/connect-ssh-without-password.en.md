---
title: "How to connect to a server SSH without password"
description: "Never retype an insecure password to log in via SSH. Learn to use public-private keys."
summary: "Never retype an insecure password to log in via SSH. Learn to use public-private keys."
date: 2024-03-13T12:26:17+01:00
draft: false
---

To connect to a server, you would normally use `ssh user@domain.com`, and to copy a file `scp file.txt user@domain.com:/home/user/`. After running the command, it will ask you for the `user` password, but this is insecure and cumbersome. The correct way is to use a public-private key pair, and that way you won't have to re-enter the password.

## Generate SSH keys

The first step is to open your terminal and type `ssh-keygen`. This command accepts some configuration commands like `ssh-keygen -t rsa` to choose the encryption system (you can choose from dsa, ecdsa, ecdsa-sk, ed25519, ed25519-sk, rsa). The following prompt will appear:

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

The "randomart image" will vary, since the key that has been generated is different each time it is run. You can change the prompt values, such as the path where the key is saved. In this case, my public and private keys have been stored in `/home/arturo/.ssh/`, with the name `id_rsa.pub` and `id_rsa`.

## Log in to the server without using a password

The second (and last) step is to make the server identify you. To do this, you will have to copy your public key `/home/arturo/.ssh/id_rsa.pub` to the server. The easiest way to do this is `ssh-copy-id -i /home/arturo/.ssh/id_rsa.pub user@domain.com`. The following prompt will appear in which you have to enter the `user` password:

```sh
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/arturo/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@172.21.0.2's password: 
```

Simply enter the password for `user` and you can access the server simply with `ssh user@domain.com`, without having to type the password.

### Alternative without ssh-copy-id

An alternative way, if you don't want to (or can't) use `ssh-copy-id`, is to copy your public key manually to the server. To do this, you will have to copy the content of `/home/arturo/.ssh/id_rsa.pub`, to see it open it, or run `cat /home/arturo/.ssh/id_rsa.pub`:

```txt
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDCY+hLD34jAoCrin5sRN1mclVxhaykt0pRdvsLxFJxypkwALqb18nx3ryYNNKygWnpmR4hmD19wHGVZZi7nUrgUcMbES03RvOdigIasfgjGR/dijs3b+HhEZ+iyJJjkOQumEm+0en4lKsh8nWecrb6FsMLRXBvPsB5lhr4bu15dP7/Ui/55LRyP/6MpszhQufS6xlaWXa2lY1dRPY/XDuWE+datnsZAPqn6KM2TQOQAvo1IPj3lwShMLuyzEfwSMQKBM+y+ltu0k/ogra1pk+PRcGEqEnjkojTmS/tigOoa9u+Zo4CDBCsTjZViaFI6aRs/+FHmfrmlWz91J1dleMhp8feIlsfhnwAFRvRvd6yYzY8N10MnixwRjw1cyNDwJgBojmjfHsz879KtkF4lQ934e1nXIrIPos7thT7tx0e4TEpmNZiB5XpIPZe8AERzyYTNqFN9pOVRIlelakjATKxrjiiNZTVJrbcMA7yIXb8BgW0kyJb1AZgSUFqcpOsLQE= arturo@localhost
```

So, access the server, you may have a way to access without SSH, because some VPS providers will allow you to access in other ways. Otherwise, simply `ssh user@domain.com`. Open the file `/home/user/.ssh/authorized_keys`, or create it if it does not exist, by running `nano /home/user/.ssh/authorized_keys`.

Finally, paste the content you just copied. You should now be able to access the server with `ssh user@domain.com` from your machine.

## Add more security

Generally, you don't want users to access your `domain.com` server using SSH with a password. This could cause you to suffer SSH attacks to try to compromise the server. To do this, simply open the file with `nano /etc/ssh/sshd_config`, look for the line where it says `PasswordAuthentication`, remove the hash, and type `PasswordAuthentication no`.
