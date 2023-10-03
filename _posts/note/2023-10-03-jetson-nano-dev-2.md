---
layout: post
title: Jetson Development Note, Part 2
categories: Research
description: 
keywords: python, hardware
---

# Hello World Project (continue)

After following the [Building the Project from Source](https://github.com/dusty-nv/jetson-inference/blob/master/docs/building-repo-2.md), continue on [Classifying Images with ImageNet](https://github.com/dusty-nv/jetson-inference/blob/master/docs/imagenet-console-2.md).

As I'm using ssh for remote access from my Mac to the Jetson machine, it is critical to set the environment correctly.

## Host Machine (Jetson) Settings

First, edit the file `/etc/ssh/sshd_config` with sudo permission, adding the following to the beginning:

```bash
UseDNS no
X11Forwarding yes
AllowTcpForwarding yes
AllowAgentForwarding yes
TCPKeepAlive yes
PasswordAuthentication no
ChallengeResponseAuthentication no
```

> Note: Only by adding the line `X11Forwarding yes` is enough for activating the remote X11 function, others are optinoal.

Then execute the following commands:
```bash
sudo systemctl restart sshd
sudo apt update
sudo apt install x11-apps
```

Now the Jetson is ready.

## Client Machine (Mac) Settings

First install the [XQuartz](https://www.xquartz.org/) for the Mac, a X11 support for macOS. After the installation, logout/reboot and login again to activate the support.

The following solution was found [here](https://unix.stackexchange.com/questions/429760/opengl-rendering-with-x11-forwarding): 

Run:

```bash
quartz-wm --help
```

which will output something like

```bash
usage: quartz-wm OPTIONS
Aqua window manager for X11.

--version                 Print the version string
--prefs-domain <domain>   Change the domain used for reading preferences
                          (default: org.xquartz.X11)
```

The last line shows the `defaults` domain. 
In this case, check it with

```bash
defaults read org.xquartz.X11
```

and change it with

```bash
defaults write org.xquartz.X11 enable_iglx -bool true
```

restart XQuartz now (by either killing it, logging out and in again or rebooting). 


## Client Machine (Mac) Checking

Start the ssh connection with `-X` option for enabling the X11 forwarding, and type in `xeyes` for start a lightweight X11 app.
If XQuartz is launched and shows a pair of eyes, the X11 environment is all set.


[homepage](/)
