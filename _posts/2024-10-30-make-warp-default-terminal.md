---
title: Make Warp as default Terminal [Mac]
author: edc0
#date: 2024-10-30 14:10:00 +05:30
categories: [Mac, Tutorial]
tags: [terminal, warp, mac, c, commandline]
render_with_liquid: false
---

[Warp](https://www.warp.dev/) is a native terminal app for macOS, reimagined to be fast, powerful, and remarkably user-friendly. Built with Rust for performance, Warp aims to provide a fresh, modern take on the traditional command-line experience, packed with features to streamline your workflow.

![warp-terminal](https://cdn.edc0.com/logbook/images/Make-Warp-default-Terminal.png)

However unlike iTerm2 there is no easy way to make it default termianl.

When I look into source code of Iterm2, I found a simple C [code](https://snippets.edc0.com/logbook/2014-10-30-make-warp-default-terminal/set-default-terminal.m) tha can do the job. First save it as `set-default-terminal.m`

<script src="https://gist.edc0.com/embed-v2.js?target=https%3A%2F%2Fgithub.com%2Fedczero%2Fsnippets.edc0.com%2Fblob%2Fmaster%2Fdata%2Flogbook%2F2014-10-30-make-warp-default-terminal%2Fset-default-terminal.m&style=base16%2Fparaiso&type=code&showBorder=on&showFileMeta=on&showCopy=on"></script>

Note, bundle id of an Mac app can simply be obtained from

```bash
osascript -e 'id of app "Warp"'
```


You can simply compile with 

```bash

# Make sure xcode commandline tools is installed. 
#If not run xcode-select --install

clang -framework Foundation -framework CoreServices -o SetDefaultTerminal set-default-terminal.m
```

Then simply execute the program from current path

```bash
./set-default-terminal
```

