---
title: CMake
description: 
published: true
date: 2022-03-26T22:34:32.871Z
tags: 
editor: markdown
dateCreated: 2022-03-26T22:34:04.394Z
---

# CMake
## Make a CMake wrapper

```bash
#!/usr/bin/env sh
source /home/user/the_script_you_want_to_source # or whatever other script you want to have executed
/usr/local/bin/cmake "$@"                       # don't forget to forward the passed parameters
```

Source: https://stackoverflow.com/questions/64462450/how-can-i-execute-a-script-prior-to-the-cmake-step-using-visual-studio-codes-cm