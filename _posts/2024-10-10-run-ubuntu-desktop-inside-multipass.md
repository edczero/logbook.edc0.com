---
title: Run Ubuntu Desktop GUI inside Multipass
author: edc0
categories: [Ubuntu, Tutorial, Gnome]
tags: [multipass, xrdp, gnome, loginctl]
render_with_liquid: false
---

Multipass is an easy-to-use tool for running Ubuntu virtual machines (VMs). In this post, we’ll walk through setting up an Ubuntu Desktop environment inside a Multipass VM and configuring it for remote access via XRDP.

### Install Multipass

On Ubuntu, you can install it using:

```bash
sudo snap install multipass
```

### Launch an Ubuntu VM with GUI

Create and launch a new Ubuntu VM in Multipass. We’ll be using the --disk, --memory, and --cpus flags to allocate appropriate resources to the VM.

```bash
# For Ubuntu 24.04 LTS

multipass launch 24.04 --name nomadic-nuthatch --disk 50G --memory 4G --cpus 4

# Check info
multipass info
```


### Creating User & Install Ubuntu Desktop GUI

Now, inside the VM, install the Ubuntu Desktop environment.

```bash
# Entering inti the shell
multipass shell nomadic-nuthatch


# By default multipass uses a root user `ubuntu`.
# However we are going to create a regular user with
# admin privilages so that we can login remotely
# using username password.

sudo adduser xrdpuser
sudo usermod -aG sudo xrdpuser
sudo usermod -aG adm xrdpuser
su xrdpuser



sudo apt update
sudo apt install --no-install-recommends ubuntu-desktop -y
```

### Install XRDP for Remote Access

```bash
sudo apt install xrdp -y


# Set GNOME_SHELL_SESSION_MODE & XDG_CURRENT_DESKTOP
# in ~/.xsessionrc file (For xorg sessions only) 

cat << EOF >~/.xsessionrc
export GNOME_SHELL_SESSION_MODE=ubuntuexport XDG_CURRENT_DESKTOP=ubuntu:GNOMEexport XDG_CONFIG_DIRS=/etc/xdg/xdg-ubuntu:/etc/xdg
EOF

# Restart xrdp and xrdp-s
sudo systemctl enable xrdp
sudo systemctl enable xrdp-sesman
sudo systemctl start xrdp
sudo systemctl start xrdp-sesman
```

### Firewall Rules (Optional) & proxy setup

```bash
sudo ufw allow 3389/tcp

# For proxy like nginx, haproxy
sudo ufw allow 33890
```



### To access the VM using RDP, you’ll need its IP address.

```bash
multipass list
```



### Access the Desktop Using RDP
Now, open your RDP client (e.g., Remote Desktop Connection on Windows, Remmina on Linux) and enter the VM's IP address. For the username and password, use the default ubuntu user credentials or the ones you have set up inside the VM.


For errors check xrdp, xrdp-sesman, xsession & syslog.

```bash
lnav /var/log/syslog
lnav /var/log/xrdp.log
lnav /var/log/xrdp-sesman.log
``` 

----

### Dangling sesions and blank screen issue

If there are too many sessions, some time they are not closed (due to some bug) and xrdp can't connect to existing session. In that case check with `loginctl` and kill them manually.

<script src="https://gist.edc0.com/embed-v2.js?target=https%3A%2F%2Fgithub.com%2Fedczero%2Fsnippets.edc0.com%2Fblob%2Fmaster%2Fdata%2Flogbook%2F2024-10-10-run-ubuntu-desktop-inside%2Floginctl-dangling-sessions.yml&style=base16%2Fparaiso&type=code&showBorder=on&showFileMeta=on&showCopy=on"></script>

As we can see user `xrdpuser` has dangloing sessions whre **pts/2** defines terminal sessions (ssh connections).

Kill them with

```bash
loginctl terminate-session c14
loginctl terminate-session c13
loginctl terminate-session c12
```

And it will again connect with xrdp.






### Proxy Setup

If you are behing proxy like haoroxy or nginx, a simple tcp port forwarding to <multipass_private_ip>:3389 will work. Here is a sample config for haproxy,

<script src="https://gist.edc0.com/embed-v2.js?target=https%3A%2F%2Fgithub.com%2Fedczero%2Fsnippets.edc0.com%2Fblob%2Fmaster%2Fdata%2Flogbook%2F2024-10-10-run-ubuntu-desktop-inside%2Fhaproxy.cfg&style=base16%2Fparaiso&type=code&showBorder=on&showFileMeta=on&showCopy=on"></script>

**10.50.27.210** is the private ip of the multipass session.


