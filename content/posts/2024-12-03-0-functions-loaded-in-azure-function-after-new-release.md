---
title: 0 functions loaded in Azure Function after new release
description: ""
date: 2024-12-03T12:02:00.000Z
preview: null
draft: false
tags:
    - Azure DevOps
    - GitHub
    - Azure Functions
categories:
    - Azure
---

After updating a project running in Azure Functions with a pipeline in Github we encountered a problem that no functions was loaded. This shows how to troubleshoot and fix this issue.

## Problem

After the solution had been built and released through a pipeline in Github using Github Actions no functions were no longer loaded when visiting Azure Functions. The solution work as expected locally when adding some new features to the solution.

When checking Application Insights this showed up in the log:

![](/assets/2024/0-functions-01.png)

The message **0 functions loaded** indicates that either there is an issue with the package that was deployed or the configuration of the Azure Functions app. I started by checking the package that was build and deployed. This can be done either by downloading the package that was built from Github or by inspecting the package in Azure Functions. In this case we look at the latter option.

From Azure Functions we can open the developer tools from **Advanced tools**:

![](/assets/2024/0-functions-02.png)

Navigating to the **SitePackages** folder for this solution the latest packages released using ZipDeploy is found:

![](/assets/2024/0-functions-03.png)

I downloaded the latest package by looking at the timestamp:

![](/assets/2024/0-functions-04.png)

Looking at the content of this folder revealed an issue as we are missing the .azurefunctions folder that should be part of the released solution.

## Solution

After doing some research I turned out that this issue was caused by the change in a Github Action that was used when packaging this solution after it was built. In **upload-artifact** it has been a breaking change where hidden files and folder was by default excluded. In Azure Functions the metainformation about the functions in the solution is stored in the **.azurefunctions** folder and by this action marked as hidden.

In our Github Actions pipeline we now need to include **include-hidden-files**:

```yaml
- uses: actions/upload-artifact@v4
  with:
    name: FunctionApp
    include-hidden-files: true
```

More information about this change can be found here: https://github.com/actions/upload-artifact/releases/tag/v4.4.0

After re-submitting this build I inspected the released package and the **.azurefunctions** folder was now included as expected:

![](/assets/2024/0-functions-06.png)

In Application Insights we also saw a successful initialization of the function:

![](/assets/2024/0-functions-07.png)

## Summary

In this case there had been some months since the last release of the functions, and even if there was a minor change to the codebase an external dependency of the project cased some issues updating the solution. Always have a strategy for testing your solutions in either a dedicated test app or a deployment slot to avoid issues in production.