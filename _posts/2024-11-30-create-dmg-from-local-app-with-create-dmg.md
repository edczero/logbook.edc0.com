---
title: Create DMG From Applications with create-dmg [Mac]
author: edc0
#date: 2024-10-30 14:10:00 +05:30
media_subpath: /logbook/images/
categories: [Mac, Tutorial]
tags: [mac, command-line, dmg]
render_with_liquid: false
---

While inbuilt `hdutil` can create a dmg from app folder (Appname.app), it can't easily be used in scripts since it requires authentications.

Fortunately, [create-dmg](https://github.com/sindresorhus/create-dmg) is a beatiful little free command-line tool by awesome [Sindresorhus](https://sindresorhus.com/apps) who publishes cool little apps for Mac!


![Picview](Create-Dmg-Picview-[Logbook].png)

```sh
# Install nodejs using brew

brew install node


# Then install create-dmg (requires nodejs >=18)
npm install --global create-dmg

# Install graphic dependencies
brew install graphicsmagick imagemagick


# Create a dmg from an app /Applications
cd ~/Desktop
create-dmg /Applications/Picview.app

```


```md
$ create-dmg --help

  Usage
    $ create-dmg <app> [destination]
    

  Options
    --overwrite          Overwrite existing DMG with the same name
    --identity=<value>   Manually set code signing identity (automatic by default)
    --dmg-title=<value>  Manually set DMG title (must be <=27 characters) [default: App name]

  Examples
    $ create-dmg 'Lungo.app'
    $ create-dmg 'Lungo.app' Build/Releases
```