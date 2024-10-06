---
title: Track Configurations Under /home/user With .gitignore
author: edc0
categories: [Ubuntu, Tutorial]
tags: [ubuntu, git, systemd, systemd-timer]
render_with_liquid: false
---



For simple Ubuntu 24.04 desktop tracking configuration files is as simple as adding them selectively in .gitignore. That way it works best since we are only adding files we want to track. 




### Initialize git in user home ($HOME)

```bash
cd 
git init
```


### Gitignore Trick:

```
# First we ignore everything at our $HOME
/*

# Then add files & folders directly under root (/home/<me>)

!/.android/*
!/.bash_aliases
!/.bash_history
!/.bash_logout
!/.bash_ps1
!/.bashrc 

# If we want to exclude some subdir (i.e .config/rclone), then root folder along with sub folder needs to be included first.

!/.config

# Then again we exclude everything under .config
/.config/*

# And finally we again only add those folders under .config/ which we want

!/.config/autostart
!/.config/dconf 


#Same tactics for .local/share
#.local/share======================
!/.local
!/.local/share

/.local/share/*

!/.local/share/applications
!/.local/share/fonts

```

### Security


>- .ssh never included
>- .config/rclone & similar folders never included since we are first excluding everything and then including selective folders.
>- Must be published on private repositories
{: .prompt-info }

Full configuration for `.gitignore` for Ubuntu Desktop is available here. It needs to be modified according to the desktop sessions or os.

### Systemd Template for Service & Timer

**git-syncd@.service**
```systemd
[Unit]
Description=A single execution git-sync script for systemd timer.
After=network-online.target


[Service]
Type=simple
#RemainAfterExit=Yes
PIDFile=/tmp/gitsync.pid
#Set up environment
Environment=REMOTE_NAME="%i"
Environment=REMOTE_PATH="/"


#Default arguments Git sync repository path. Can be overridden in the environment file
Environment=GIT_SYNC_REPO_PATH=""
EnvironmentFile=/etc/environment
#Environment="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"


#Overwrite default environment settings with settings from the file if present
EnvironmentFile=-%h/.config/systemd/user/%i.env

#Prestart check for repositoty path
ExecStartPre=/bin/bash -c "cd ${GIT_SYNC_REPO_PATH}"


#Run git-sync once
ExecStart=git-syncd -s -n "${GIT_SYNC_REPO_PATH}"


[Install]
WantedBy=default.target
```


**git-syncd@.timer**
```systemd
[Unit]
Description=Data backup timer
Requires=git-syncd@%i.service

[Timer]
#OnCalendar=daily
#OnBootSec=1min
OnUnitActiveSec=12hr
Unit=git-syncd@%i.service

[Install]
WantedBy=timers.target
```

**home.env**
```systemd
[DEFAULT]
GIT_SYNC_REPO_PATH="/home/me/"
```


```sh
systemctl --user enable git-syncd@home.timer
systemctl --user start git-syncd@home.timer
```

That’s it. Now with simple command-line tools like git-syncd and systemd timer it is now automated!