---
title: How to show folder structure in markdown ?
author: edc0
categories: [Notes, Tutorial, HowTo]
tags: [markdown, commandline, preview, coding]
render_with_liquid: false
---

One can generate folder structure manually by command and copy the structure and enclose in three backticks symbols in Markdown.

```bash
#For Ubuntu/Debian
sudo apt install tree

# On MacOs
brew install tree
```

```bash
cd /path/to/folder/static

tree --dirsfirst --charset=ascii
```

<script src="https://gist.edc0.com/embed-v2.js?target=https%3A%2F%2Fgithub.com%2Fedczero%2Fsnippets.edc0.com%2Fblob%2Fmaster%2Fdata%2Flogbook%2Fcommon%2Fdir_tree_markdown.yml&style=base16%2Fparaiso&type=code&showBorder=on&showFileMeta=on&showCopy=on"></script>

This can be saved to `yaml`.

To make the folder structure more readable in markdown, one can specify the tree command with options like -I (ignore specific files) or limit the depth with -L. For example:

```bash
tree -L 2 > structure.md
```

