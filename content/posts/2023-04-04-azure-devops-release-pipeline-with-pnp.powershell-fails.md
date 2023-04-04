---
title: Azure DevOps release pipeline with PnP.PowerShell fails
description: ""
date: 2023-04-04T11:37:21.950Z
preview: ""
draft: false
tags:
  - devops
  - powershell
categories:
  - Azure
---

While deploying some changes to an existing solution the release pipeline in Azure DevOps suddenly failed while running a PowerShell script for releasing a SharePoint Framework (SPFx) app. This release pipeline uses the PnP.PowerShell module, and automatically installs the latest version each time it is run.

Lately there has been a mayor update to PnP.PowerShell moving from version 1 to 2. Some of the key changes is the support for PowerShell moving from 5.1 to 7.x and the use of .NET 6.

The pipeline stopped with this error:
![](/assets/2023/04/devops%20-%20failed%20pipeline.png)

Step failing:
![](/assets/2023/04/devops%20-%20failed%20script.png)

From the detailed logs:
```
The 'Connect-PnPOnline' command was found in the module 'PnP.PowerShell', but the module could not be loaded. For more information, run 'Import-Module PnP.PowerShell'.
```

## Solutions 1: Workaround

The reason the release pipeline in Azure DevOps fails is that it by defalt runs PowerShell version 5.1 even in the v2 of the PowerShell script task.

To quickly remediate the issue would be to force the use of the latests 1.x of PnP.PowerShell in the PowerShell script in the release pipeline:

```
$PnPModuleName = "PnP.PowerShell"
$PnPModuleVersion = "1.12.0"
Install-Module -Name $PnPModuleName -RequiredVersion $PnPModuleVersion -Scope "CurrentUser" -Force
```

Forcing the latest version supporting PowerShell 5.1 will make the release run as normalt again. If you don't have access to change the pipeline but only the release script this will be a workaround.

## Solution 2: Permanent fix (recommended!)

As earlier mentioned moving forward PowerShell 7.x is requried to use PnP.PowerShell 2.x. To fix this in Azure DevOps without changing the release script you need to enable this in the PowerShell task.

Ensure that you are running version 2 of the task:
![](/assets/2023/04/devops%20-%20task%20version.png)

Under *Advanced* enable *PowerShell Core*:
![](/assets/2023/04/devops%20-%20task%20ps%20core.png)

Make sure to remember to this for all of the stages.

If you are using a YAML based pipeline this can be enabled by setting *pwsh* to *true*:
![](/assets/2023/04/devops%20-%20task%20yaml.png)

## Summary
DevOps pipelines using latests and current versions of packages and modules will break from time to time. Unfortunately this can change when you have a critical deploy and cause issues. Since this can be hard to debug when you focus is on releasing software, I hope publishing these steps to find and mitiage such issues can be of help to others.