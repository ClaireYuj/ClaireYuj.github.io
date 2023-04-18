---
title: torch配置
top: false
cover: false
author: DULULU oO
date: 2021-07-21 11:25:24
password:
summary:
tags: CUDA
categories: 机器学习
---

> cuda:11.3.0（官网安装）https://developer.nvidia.com/cuda-11.3.0-download-archive?target_os=Windows&target_arch=x86_64&target_version=10&target_type=exe_local
> Nvida CUDA 11.4.141
> 显卡： NVDIA Getforce RTX 3050 Ti

troch安装网址：https://pytorch.org/get-started/locally/

## 镜像配置

直接在C:\\user\\.condarc中修改

```shell
channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  - defaults
ssl_verify: false
show_channel_urls: true
report_errors: true


```