---
title: Rename Screenshots from Finder context menu using Shortcuts & Chat-Gpt
author: edc0
#date: 2024-10-30 14:10:00 +05:30
image: https://cdn.edc0.com/logbook/images/[Header]-Quick-Actions-Menu-With-AI-Rename-Option.png
media_subpath: /logbook/images/
categories: [Mac, Tutorial]
tags: [mac, shortcuts, finder, chat-gpt, ai]
render_with_liquid: false
---

Tired of screenshots named "Screenshot 2023-10-27 at 10.30.00 AM"? Me too! Let's face it, those default names are useless for finding anything later. But what if you could right-click a screenshot and instantly rename it with a descriptive title generated by AI? Turns out, you can! Here's how to leverage the power of Shortcuts, ChatGPT, and your Mac's Finder context menu to rename screenshots like a pro.

### What You'll Need:

```md
- macOS Monterey or later: Shortcuts app is built-in.

- An OpenAI API Key: You'll need this to access ChatGPT. Get one at https://platform.openai.com/account/api-keys).
(Note: Using the API incurs costs, so be mindful of your usage.)

- There **has to be** some amount of credit avaialable in openai account. Minimum $5.

- [AI Action](https://sindresorhus.com/ai-actions) shortcuts extension from Sindre Sorhus. 
It makes it easy parsing request and respose with gpt api calls.
```


### Build the Shortcuts

![AI-Rename-Action-Setup-Interface-0](AI-Rename-Action-Setup-Interface-0.png){: width="972" height="494" .w-50 .right}


- Create a New Shortcut: Rename it "**Rename with AI**". Ckick the {i} button. Select use as Quick Action and enable Finder and service menu under it.
- For input select only images
- Click + search for **Ask Ai** and drag it into main window
- We are going to use folowing system prompt (one can use diffrent one)

<script src="https://gist.edc0.com/embed-v2.js?target=https%3A%2F%2Fgithub.com%2Fobjectstack%2Fobjectstack.github.io%2Fblob%2Fmain%2Fdata%2Flogbook%2Farchives%2FOpenai-screenshot-renamer-system-prompt.yml&style=base16%2Fhopscotch&type=code&showBorder=on&showLineNumbers=on&showFileMeta=on&showFullPath=on&showCopy=on"></script>


- Build the shortcuts as shown the image:

![Screenshot-Renemer-Config](Screenshot-Renemer-Config.png)


### Access it from Right click menu

Now it's available in the right-click menu under Quick Actions!

![]([Header]-Quick-Actions-Menu-With-AI-Rename-Option.png){: width="950"}