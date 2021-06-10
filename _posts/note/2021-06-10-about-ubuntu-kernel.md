---
layout: post
title: Downgrade Ubuntu Kernel
categories: System
description: 
keywords: ubuntu, linux, nvidia
---


# Accidentally Updated System Kernel

Sometimes we want to use package managers to update 
packages but accidentally updated linux kernel packages as well.
The updated kernel may brake some softwares, 
for example it may brake the installed Nvidia GPU driver.
It is common that after installed the kernel and rebooted we found that Nvidia GPU driver stops running.

Unlike ```yum``` in CentOS, which could easily update packages without kernel-packages with command:

```bash
yum update --exclude=kernel*
```

the ```apt``` of Ubuntu doesn't have such functions. It would be a little complicate to achieve the same result.

The following is a note for altering Ubuntu kernels.

# Check Installed Kernel

First check currently installed kernel(s) with command:

```bash
dpkg --list | grep linux
```

it will show the packages that contains 'linux' in their names, we should focus on packages like 

```bash
ii  binutils-x86-64-linux-gnu             (some discriptions)
ii  linux-base                            (some discriptions)
ii  linux-firmware                        (some discriptions)
ii  linux-headers-5.4.0-65-generic        (some discriptions)
ii  linux-hwe-5.4-headers-5.4.0-65        (some discriptions)
ii  linux-image-5.4.0-65-generic          (some discriptions)
ii  linux-libc-dev:amd64                  (some discriptions)
ii  linux-modules-5.4.0-65-generic        (some discriptions)
ii  linux-modules-extra-5.4.0-65-generic  (some discriptions)
ii  linux-sound-base                      (some discriptions)
```

Focus on those packages with prefix ```linux-headers```, ```linux-hwe```,```linux-image```, ```linux-modules```, ```linux-modules-extra``` with version numbers in their names, 

If you have installe new kernels but haven't manually removed previous one, they will all be there, the system just simply used the latest one.

Using ``` uname -a``` could know the currently using kernel version.

# Remove Installed Kernel

use command
```bash
sudo apt purge linux-XX 
(relpace with package names of new version kernel)
sudo apt autoremove
```

After removed the newer version (remember remove all the related packages with the same version number), we still need to update ```grub``` for boot using:

```bash
sudo update-grub
```

I'm not sure which kernel version will be used if there are still multiple kernels appear after the removing, 
but I presume ```grub``` will use the latest in the remaining ones. For me I just left one kernel after removing new ones.

After update ```grub```, reboot and check the using kernel with ```uname -a```.

# Holding Kernel

After downgrade the kernel version, we could hold the kernel packages to prevent them from updating:

```bash
sudo apt-mark hold linux-XX (relpace with package names of kernel)
```

Remember hold all the related packages with the same version number. 

After holding, the result of 

```bash
dpkg --list | grep linux
```
will be like:

```bash
ii  binutils-x86-64-linux-gnu             (some discriptions)
ii  linux-base                            (some discriptions)
ii  linux-firmware                        (some discriptions)
hi  linux-headers-5.4.0-65-generic        (some discriptions)
hi  linux-hwe-5.4-headers-5.4.0-65        (some discriptions)
hi  linux-image-5.4.0-65-generic          (some discriptions)
ii  linux-libc-dev:amd64                  (some discriptions)
hi  linux-modules-5.4.0-65-generic        (some discriptions)
hi  linux-modules-extra-5.4.0-65-generic  (some discriptions)
ii  linux-sound-base                      (some discriptions)
```

Notice that the status of packages (the first two columns (actually three, the third one won't show if no error occurs)) has changed from ```ii``` to ```hi```, which indicates that the package has been hold (use ```man dpkg-query``` for more info).

From now on the ```apt update``` will still check the newer version of kernel but ```apt upgrade``` will not upgrade kernel anymore.

If need to unhold for upgrading kernels, use

```bash
sudo apt-mark unhold linux-XX
```


[homepage](/)
