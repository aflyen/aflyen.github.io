---
title: NuGet Package restore failed for .NET project in Visual Studio
description: ""
date: 2022-12-22T14:32:21.710Z
preview: ""
draft: true
tags:
    - .NET
    - Visual Studio
categories:
    - Azure
---

Moving to a new computer made me test out a new organization of my projects. Before I always have placed this on the root of the drive, in example C:\Projects. Now I though why not try out to follow my user's home directory like is more common when working in Linux (been doing a lot of work in 
WSL and Ubuntu lately). Change is not always good and this time this led med into trouble. Restoring a larger project in Visual Studio failed when installing the NuGet Packages:

![](/assets/2022/12/nuget-package-restore-failed-vs.png)

The error states "The NuGet Package restore failed for project ...". Checking the folder in the error message only showed me that it was empty - no .dll's to be found. Tried to rebuild, restore, re-this and re-that without any help.

## Solution

The problem ended up being the reason why you should keep to the pattern of storing the source code at a root folder. My projects folder hierarchy was simply to deep, making the folder path exceeding the limits of Windows.

Moving the folder back down to **C:\Projects** solved the NuGet package issue.