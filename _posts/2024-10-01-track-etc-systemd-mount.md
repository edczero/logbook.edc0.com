---
title: Track /etc From /home/user With Systemd Mount (bindfs) & Git
author: edc0
categories: [Ubuntu, Tutorial]
tags: [ubuntu, git, systemd, systemd-timer, bindfs, etc, configuration]
render_with_liquid: false
---



From [Another post](/posts/track-configuration-in-home-user-with-gitignore), long ago I created an automated tracking of configuration files under my user’s home with .gitignore. This time We add configuration /etc and track it under a single .`gitignore`


### Systemd Mount with Bindfs

```bash
sudo apt install -y bindfs

# Create folder root/etc under /home/user 
cd && mkdir -p root/etc

# As per systemd doc the mount unit name should be home-<user>-root-etc.mount. Create it under ~/.config/systemd/user

# Note everything will be user level. No sudo.

# Copy below & save under ~/.config/systemd/user/
```


### Systemd mount (replace with your username)

```systemd
[Unit]Description=Mount /etc under $HOMEAfter=network.targetConditionPathExists=/home/me/root/etc[Mount]What=/etcWhere=/home/me/root/etc/Type=fuse.bindfsOptions=ro,no-allow-otherTimeoutSec=30[Install]WantedBy=default.target
```

> ro -> For read only mount. bindfs requires `no-allow-other` unless we modify /etc/fuser.conf
{: .prompt-tip}

```bash
systemctl --user daemon-reload
sytemctl --user enable home-me-root-etc.mount
sytemctl --user start home-me-root-etc.mount
```


### Security

```bash
# First let exclude folder under /etc which are not publicly accessible. 

find /etc -maxdepth 1 -type d -xtype d -perm -o+x -printf "%f\n" | \
	sort -k1 | xargs -0 echo > ~/Desktop/gitignore-folders.txt


# This took out folders like sudoers.d and all from /etc at root level.
# But there can be folders and files within those folders,
# which doesn't have public read permisson. 
# Find those and add root level directory to gitignore-folders.txt


find /etc -type d -xtype d -not -perm -o=r -exec ls -ld {} \;

#drwx------ 2 root root 4096 Sep 19 22:18 /etc/multipath#find: ‘/etc/multipath’: Permission denied#drwxr-x--- 2 root root 4096 Sep 19 22:19 /etc/sudoers.d#find: ‘/etc/sudoers.d’: Permission denied#drwx------ 2 root root 4096 Apr 19 19:54 /etc/credstore#find: ‘/etc/credstore’: Permission denied#drwx------ 2 root root 4096 Apr 19 19:54 /etc/credstore.encrypted#find: ‘/etc/credstore.encrypted’: Permission denied#drwx--x--- 2 root ssl-cert 4096 Sep 19 22:52 /etc/ssl/private#find: ‘/etc/ssl/private’: Permission denied#drwxr-x--- 2 root polkitd 4096 Apr  3  2024 /etc/polkit-1/rules.d

# Hence exclude ssl, polkit-1 etc.


# Next for files, and put it in gitignore-files.txt

find /etc -maxdepth 1 -type f -xtype f -perm -o+r -printf "%f\n" | \
	sort -k1 | xargs -0 echo > ~/Desktop/gitignore-files.txt


# Recursively search files 
find /etc -type f -xtype f -not -perm -o=r -exec ls -ld {} \;

# It will list gshadow, ssh private host keys, ufw rules, some lock files 
# And /etc/security/opasswd. We want security but not opasswd. 
# In that case we can simply exclude the file after including the folder in .gitignore. 
# Exclude the rest.

# !security
# security/opasswd

# Also manually exclude /etc/passwd.
# passwd


# Merge both files and combined into gitignore.txt

# Append "!/root/etc/" at the beginning of each line
sed -i 's|^|!/home/root/etc/|' gitignore.txt

# Copy it to ~/.gitignote
```

The comple .gitignore (configuration files from both /home/user and mounted /etc) are availble [**here**](https://cdn.edc0.com/logbook/files/gitignore-with-etc.txt). (Excluded  objects are commented out instead removing it completely for reference.)