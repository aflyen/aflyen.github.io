---
title: Update your Azure Devops and GitHub pipelines to support legacy projects
description: ""
date: 2024-09-06T12:00:00.708Z
preview: ""
draft: false
tags: [Azure DevOps, GitHub, Node.js, Gulp, SPFx]
categories: [Microsoft 365, SharePoint]
---

Ideally you would work with greenfield projects all the time, and not spend time working on legacy solutions and troubleshoot why things that worked before have stopped working! Over time hopefully some your solutions will stay running over years, and in this example for over 6 years!

## Challenges with DevOps

After the need to change some minor functions in this solution build on React with the SharePoint Framework, the build failed in Azure DevOps. Some weeks later, somewhat by chance, I ran into the same issue with GitHub Actions. This solution had not been update sine 2018, and used Node.js v8. While upgrading the project would be ideal, sometimes there’s neither the time nor the budget for that.

When running the Gulp commmand **gulp bundle --ship** this error occurred:

```bash
Error: yargs parser supports a minimum Node.js version of 10. Read our version support policy: https://github.com/yargs/yargs-parser#supported-nodejs-versions
    at Object.<anonymous> (/usr/local/lib/node_modules/gulp/node_modules/yargs-parser/build/index.cjs:1007:15)
```

Since the pipeline declares the correct version of Node.js, we could assume this would work as expected? The problem lies in the packages that follow the build images used as they have different packages available.

## Solution

In this case the problem was the version of Gulp required to run the build and packaging of the solution. By enforcing the correct version as part of the setup of the pipeline, this issue was solved.

### GitHub Actions

```yaml
- name: Install project dependencies
      run: |
        npm i -g --quiet gulp-cli@2.3.0
        npm ci
```

### Azure DevOps

```yaml
- script: |
    npm install -g --quiet gulp-cli@2.3.0
    npm ci
```

## Summary

In this instance, the root cause was an outdated version of the Gulp package. When working with legacy solutions that lag behind greenfield versions this can cause an issue in Azure DevOps and Github. So the recommendation is to try to enforce the correct versions of you dependencies so solve similar issues.