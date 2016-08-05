---
layout: post
title:  "Set up Visual Code debugger for Angular CLI"
date:   2016-08-05 16:18:00
categories: angular2
tags: angular2
#image:
published: true
---

To set up debugger in Visual Code, the debugger must be configured to attach to a node process in `launch.json`  ( `⌘ + Shift + D` to open Debugger).

```json
{
      "name": "Attach",
      "type": "node",
      "request": "attach",
      "port": 5858,
      "address": "localhost",
      "sourceMaps": true,
      "outDir": "${workspaceRoot}/dist",
      "localRoot": "${workspaceRoot}"
    }
``` 

1. Run your app in node debug mode `node debug node_modules/angular-cli/bin/ng serve`
2. Run `Attach` in Debugger menu
