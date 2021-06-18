---
layout: post
title: About Ubuntu 18.04 Gnome Desktop Remote
categories: System
description: 
keywords: Ubuntu, Gnome Desktop
---

# Enable RDP Service

On Ubuntu 18.04, the inner change has broken the original xrdp service, 

thus it will need these two packages to start a RDP service:

```bash
sudo apt install xrdp
sudo apt install xorgxrdp-hwe-18.04
```

without using the ```hwe``` version, the xrdp will be blank black when connect.


# Enable Dock and Change Icon

Install the tweaks tool first:

```bash
sudo apt install gnome-shell-extensions
```

or

```bash
sudo apt install gnome-tweaks
```

Then alter the desktop with the tweaks tool.


# Missing Dock when remote access Ubuntu 18.04 Gnome Desktop with XRDP

The Dock will occasionally disappear when login to the account, re-login or reboot could solve it temporary,

Lei Mao (https://leimao.github.io/blog/Ubuntu-Dock-Disappear/) provided a good way to solve it. It's not permanent, but it works.


>The workaround is to reboot the Gnome shell by following the protocols presented below.
> 
>1. Press ALT + F2.
>2. In the pop-up terminal, type 'r' and press Enter.
> 
>The Gnome shell will restart.


These steps will restart the gnome shell, and there's no need to reboot any more.

[homepage](/)
