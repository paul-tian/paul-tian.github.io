---
layout: post
title: Jetson Development Note, Part 1
categories: Research
description: 
keywords: python, hardware
---

As I am now working on Jetson hardware for my research, I would like to keep some notes for future usage.

# Jetson Related Resources

[Jetson nano guide](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit) 

[Orin nano guide](https://developer.nvidia.com/embedded/learn/get-started-jetson-orin-nano-devkit) 

[Official FAQ](https://developer.nvidia.com/embedded/faq) 

[Official software page](https://developer.nvidia.com/embedded/develop/software) 

[Official resources](https://developer.nvidia.com/embedded/community/resources) 

[Community porjects](https://developer.nvidia.com/embedded/community/jetson-projects) 

[Official forum](https://forums.developer.nvidia.com/c/agx-autonomous-machines/jetson-embedded-systems/jetson-projects/78) 

[Jetson zoo](https://elinux.org/Jetson_Zoo) 


# Hello World Project

The official hello world project could be found [here](https://github.com/dusty-nv/jetson-inference).
After following the [Setting up Jetson with JetPack](https://github.com/dusty-nv/jetson-inference/blob/master/docs/jetpack-setup-2.md), continue on [Building the Project from Source](https://github.com/dusty-nv/jetson-inference/blob/master/docs/building-repo-2.md).

> Note: The provided steps in [Building the Project from Source](https://github.com/dusty-nv/jetson-inference/blob/master/docs/building-repo-2.md) is <span style="color:red">**NOT**</span> compatible with virtual environment, as the command `sudo make install` will install the package in `/usr/lib/python*/dist-packages/` (the default global environment path, `*` stands for the python version number). Though it could be fixed by adding a file named as `global_packages.pth` and `/usr/lib/python*/dist-packages` as the content to the library directory of the corresponding virtual environment, I decide to use global environment for this project.

Run command:

```bash
sudo apt-get update
sudo apt-get install git cmake libpython3-dev python3-numpy
git clone --recursive --depth=1 https://github.com/dusty-nv/jetson-inference
cd jetson-inference
mkdir build
cd build
cmake ../
make -j$(nproc)
sudo make install
sudo ldconfig
```

The detailed explanation is in the [original GitHub repository](https://github.com/dusty-nv/jetson-inference/blob/master/docs/building-repo-2.md), currently there's no need for me to keep anything more in this note.


[homepage](/)
