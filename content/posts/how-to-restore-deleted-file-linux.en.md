---
title: "How to restore a deleted file in Linux"
description: "You have deleted a file from the terminal, and you don't know how to recover it... Luckily, now I'm going to tell you the definitive trick."
summary: "You have deleted a file from the terminal, and you don't know how to recover it... Luckily, now I'm going to tell you the definitive trick."
date: 2025-01-27T18:27:59+01:00
draft: false
tags: ["linux"]
---

Surely it has happened to you at least once that you executed `rm file.txt`, but in reality, you didn't want to delete that file... The problem is that the terminal doesn't have a trash bin; thus, the file can't be recovered, or maybe it can...

First of all, you need to know that when you delete a file, the 0s and 1s remain on the hard drive, meaning not everything is lost.

## Methods to recover a deleted file

I think it's obvious, but if you deleted it conventionally, **have you tried recovering it from the trash?**

If you deleted it using the `rm` command, or even if it was in the trash and you emptied it, there is specialized free software for this, such as **foremost**, **ext4magic**, or **extundelete**.

But the method I'm going to teach you next is undoubtedly the best because of its ease, simplicity, and elegance. Have you ever used the `grep` tool? If the answer is 'yes', you're halfway done.

## Recovering a deleted file using grep

In case you don't know it, `grep` is a tool for finding patterns. Try the following example:

```bash
cat <<EOF >> names.txt
Charlie
Arthur
John
Oliver
EOF
grep 'r' names.txt
```

You will have created a file called `names.txt`, and with `grep` you listed all the lines containing the letter `r`. You can add options to the `grep` command, for example `-A` (after) and `-B` (before):

```bash
grep -A 1 'u' names.txt
```

With this option, you listed the names containing the letter `u` and the one that follows.

And now comes what you wouldn't expect from `grep`: you can not only search within files but also **search within devices**, for example, **your entire hard drive**, or your USB. As I mentioned earlier, although you've deleted your file, the bytes are still on the hard drive, but your operating system doesn't know where they are.

If the file was on the hard drive, find out which one it is. In Linux, disks are located at `/dev/`. If it's an SSD, the name will start with `/dev/nvme...`; whereas if it's an HDD, it will be `/dev/sd...`. And a USB will likely have a name similar to the HDD.

Have you found the device? Now you need to remember something that was in the file. I'll use "some content on the file":

```bash
grep -a -A 200 -B 100 'some content on the file' /dev/nvme0n1
```

After several minutes (depending on the read speed of the device), the content of the file will appear on your terminal! 100 lines above and 200 below the pattern you wrote.

## How to completely delete a file?

Now I imagine that, like when I discovered this, you're wondering: so, how do I really delete a file?! The answer is easy: **fill it with zeros**.
I'll show you two Linux commands you can use:

```bash
dd if=/dev/zero of=file.txt bs=1M
rm file.txt
```

And if instead of zeros, you want to write random numbers:

```bash
shred -u file.txt
