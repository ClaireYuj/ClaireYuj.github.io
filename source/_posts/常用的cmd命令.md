---
title: 常用的cmd命令
top: false
cover: false
author: DULULU oO
date: 2023-04-04 22:45:56
password:
summary:
tags: cmd
categories: Linux
---
netstat -ano|findstr "8080"
可用于查找8080端口是否被占用，若出现  TCP    [::]:8080              [::]:0                 LISTENING       40788

可用tasklist|findstr "40788"找到该进程名，假设进程为01.exe

可用taskkill /f /t /im 01.exe 强制结束该进程

net start mysql80可启动mysql服务