---
title: Jupyter使用指北
top: false
cover: false
author: DULULU oO
date: 2022-03-17 14:04:37
password:
summary: Jupyter的安装、基本配置已经初级操作
tags: 
    - Jupyter
    - Python
categories: 环境配置
---

## 安装

使用pip进行安装，为了避免可能产生的配置错误，这里直接进入conda虚拟环境进行安装

```cmd
    conda activate jupyter
    pip install jupyter
```

## 使用

```cmd
    jupyter notebook
```

注意使用该命令所在的目录，如果直接在user目录下运行该指令，打开jupyter会出现
![C:\Users> jupyter notebook](/img/posts/config/Jupyter_desktop.jpg)
如果有要直接打开的项目，可cd到指定的目录下在运行jupyter notebook命令

## Kernel 内核配置

- 查看内核
    ```cmd
        jupyter kernelspec list
    ```
可以查看指定目录下的kernel.json文件，看python解释器的位置是否正确

- 添加内核(在该虚拟环境下)
    ```cmd
        python -m ipykernel install --name jupyerEnv --display-name jupyterEnv --prefix=E:\Software\Anaconda\Anaconda\envs\unity\
    ```
    会添加到E:\Software\Anaconda\Anaconda\share\jupyter\kernels\jupyterEnv目录下

- 删除内核
    ```cmd
        jupyter kernelspec remove jupyterEnv
    ```



## jupyter与python环境

一般jupyter会默认使用anaconda默认的python环境，也就是说，在某个虚拟环境中pip的包，在jupyter中不一定能成功地import
所有如果要在某个环境下使用jupyter
先 conda activate 该环境
再在该环境下jupyter notebook
