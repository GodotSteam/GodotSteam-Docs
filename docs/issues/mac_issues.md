---
title: Mac Issues
description: A list of common Mac issues you may run into.
icon: simple/apple
---

# Issues - Mac Platform

There are issues some users might run into on macOS when using Godot and GodotSteam.

{==
## :material-alpha-m-circle: M1 and M2 Machine Issues
==}

From user **canardbleu**, a handy tip about using the pre-compiled editors and macOS M1 and M2 machines.

Their team said "[we were] unable to launch any scene because of the 'files & folders' permission popup that keeps popping and popping. Fortunately, we were able to find a workaround by executing the following command."

```shell
codesign -s - --deep /Applications/GodotEditor.app
```

{==
## :material-network-off: Steam Client Error
==}

A user reported an issue on Mac where during initialization, they kept getting the following error:

> { "status": 1, "verbal": "Could not determine Steam client install directory." }

They reported that deleting the Steam client and the data folder "~/Library/Application Support/Steam", then re-installing the Steam client seemed to fix the issue.