---
layout: post
title: Compile LLaMA.cpp on Ubuntu 24.04 with CUDA 12.6 Support
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


# Compile LLaMA.cpp on Ubuntu 24.04 with CUDA 12.6 Support

As of writing this note, I'm using llama.cpp version b4020.

In my previous [post](/2024/11/01/llama-cpp-ubuntu-22-04/) I implemented LLaMA.cpp on Ubuntu 22.04 with CUDA 11, 
but the system compiler is really annoying, saying I need to adjust the link of ```gcc``` and ```g++``` frequently for different purposes.


I then noticed LLaMA.cpp could support from a certain version, at least b4020. 

Thus I reinstalled my system with Ubuntu 24.04 and CUDA 12.6, the workflow is more fluent now.

Here are some packages that I frequently use, thus I would like to place them here for future reference.

```bash
sudo apt update
sudo apt install build-essential net-tools openssh-server 
sudo apt install vim screen htop git cmake ccache wget
```

I have 2 machines, one with Nvidia 3070, one with 3090, this version of cuda toolkit fits me well.

```bash
wget https://developer.download.nvidia.com/compute/cuda/12.6.2/local_installers/cuda_12.6.2_560.35.03_linux.run

sudo sh cuda_12.6.2_560.35.03_linux.run
```

It comes with the driver, but the installation will be a little bit complex. 
If we are not disabling the Nouveau manually, the driver installer could help us to do so.
It will first fail the installation, then help us to disable (check the log for confirmation), then reboot and re-run the runfile.



[homepage](/)
