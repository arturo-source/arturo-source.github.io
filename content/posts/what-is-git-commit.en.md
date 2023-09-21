---
title: "What is git commit?"
description: "Git basics in a minute."
summary: "Git basics in a minute."
date: 2023-09-14T15:59:25+02:00
draft: false
tags: ["tiny pills"]
---

Committing in `git` means saving your file changes to your local repository. You always want to use a descriptive message to record the evolution of your project.

Making a commit in git is one of the fundamental concepts in **version control**. When you make a commit, you are creating a checkpoint in the history of your project. Each commit contains a snapshot of the files at that time, along with a descriptive message explaining the changes made.

## Steps to save your files

There are a lot of platforms that integrate `git` (GitHub, GitLab, Bitbucket, Gitea). However, first I am going to give you the basic knowledge of working from your PC.

In this case I will talk about a programming project. Typically, you will have several files and folders. When you want to save your progress, you may have changed several files, but only want to record changes to a few. With `.` you will record changes to the folder you are working on.

```bash
git add .
```

And if you only want to record the progress of three files, you will do it as follows.

```bash
git add file1 file2 folder1/file1
```

The second step, once you have registered the changes you want to save, is to commit the changes, what I will call 'making a commit'. It is very important to place a message that represents the changes since the last commit. We can do this in two ways.

The first, for me, the simplest. Write the command and a summary of the changes will appear in your window. You can now write the comment.

```bash
git commit
```

The second, faster. Add `-m` to the command to write the message directly.

```bash
git commit -m "Info about changes"
```

This is all you need to keep track of changes in your application. To see all checkpoints, simply type `git log`.

## Save your code in the cloud

There is one last fundamental command you want to know. It will help you save your code and all your changes in the cloud. Furthermore, when you are a professional, it will allow you to work with your colleagues, on the same code.

```bash
git push
```

Push means upload your changes. You will do this after you have committed your changes with your commit, after several commits. But what if your colleagues want to download your changes?

```bash
git pull
```

Pull means downloading the changes. You will want to do this whenever you want to have your changes in sync with the rest of the team.