---
layout: post
title: Compile LLaMA.cpp on Ubuntu 22.04 with CUDA 11.8 Support
categories: [Research]
description: 
keywords: llama, ubuntu
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---


# Compile LLaMA.cpp on Ubuntu 22.04 with CUDA 11.8 Support

As of writing this note, the latest llama.cpp version is b3995.

When compiling this version with CUDA support, I was firstly using Ubuntu 20.04. However, there are some incompatibilities (gcc version too low, cmake verison too low, etc.) and I have to update the system.

I first tried to update to 24.04. However, Ubuntu 24.04 is not supporting CUDA 11.X which is required by llama.cpp for CUDA support.

Thus I downgraded to 22.04, but there lies a very deep and tricky problem with Ubuntu 22.04. 
On Ubuntu 22.04, the default GNU C compiler version is gcc-11.
However, the latest default kernel version (6.8.0-48-generic as of writing this note) is built using gcc-12.
More info can be found [here](https://askubuntu.com/questions/1500017/ubuntu-22-04-default-gcc-version-does-not-match-version-that-built-latest-defaul).
In this case the Nvidia driver cannot compile for the kernel. 

We need to install the gcc12 and g++12

```bash
sudo apt install gcc-12 g++-12
```

then 

```bash
sudo ln -s -f /usr/bin/gcc-12 /usr/bin/gcc
sudo ln -s -f /usr/bin/g++-12 /usr/bin/g++
```

for switching the system default compiler to 12.

After install the driver and cuda 11.8 toolkit with runfile, we need to 

```bash
sudo ln -s -f /usr/bin/gcc-11 /usr/bin/gcc
sudo ln -s -f /usr/bin/g++-11 /usr/bin/g++
```
for switching the system default compiler to 11, otherwise the llama.cpp cannot compile.

We need to switch the compiler to 12 if we need to update the kernel 
and let the dkms to update the nvidia driver, 
but maybe if the system is stable there's no need to update the kernel.


[homepage](/)
