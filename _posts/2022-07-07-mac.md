---
layout: post
title:  "Use GPU-accelerated PyTorch on Mac"
date:   2022-07-12 00:00:42 +0530
categories: Python Pytorch Intel
---

# Set up Environment
I have been used GPU accelerated TensorFlow on my MacBook Pro with AMD GPU. Now Pytorch has joined this field.

## MPS supported Pytorch requires MacOS 12.3+

To test the environment, you can run python and import **platform** package.

```
import platform

platform.platform()
```
Here on my macbook, it shows 'macOS-12.4-x86_64-i386-64bit'.

## Install Pytorch
In this webpage, [https://pytorch.org/get-started/locally/](https://pytorch.org/get-started/locally/), select Preview(Nightly), Mac, Pip, Python, and Default. It will generate the install command for you as follow:

```
# MPS acceleration is available on MacOS 12.3+
pip3 install --pre torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/nightly/cpu
```

## Check installation
- Check Pytorch version

```
import torch

torch.__version__
```

- Check if pytorch has mps

```
torch.has_mps
```
If your torch has mps, it will return **True**.

# How to use
## set device = torch.device('mps')

```
device = torch.device('mps')
model.to(device)
tokens.to(device)
```

# Reference
https://towardsdatascience.com/gpu-acceleration-comes-to-pytorch-on-m1-macs-195c399efcc1