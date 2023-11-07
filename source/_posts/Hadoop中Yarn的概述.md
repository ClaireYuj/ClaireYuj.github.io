---
title: Hadoop中Yarn的概述
top: true
cover: false
author: DULULU oO
date: 2022-08-19 13:54:56
password:
summary:
tags: 
    - Hadoop
    - Yarn
categories:
    - 数仓开发
---

学习参考[Hadoop入门](https://www.bilibili.com/video/BV1CU4y1N7Sh?p=49&spm_id_from=pageDriver&vd_source=f618be285e44e05f2b481fa28b9fe6ad)

## Yarn的简介

Hdfsd大数据存储系统中的资源管理器主要为MapReduce与Yarn，是一个通用的**资源管理系统**和**调度**平台，可为上层应用提供统一的资源管理和调度，为集群再利用率，资源统一管理和数据共享等方面带来了巨大好处

**资源管理系统**: 管理集群的硬件资源，包括内存，CPU等
**调度平台**： 多个程序同时申请资源，如何进行合理的分配与调度
Yarn作为一个通用平台，说明它不仅仅支持MR（MapReuce）程序，理论上支持各种计算程序（Spark，HBase，Storm...）

可以把Yarn理解为一个分布式的操作系统平台，HDFS是应用最广泛的大数存储系统，Yarn功不可没

## Yarn中的组件

主要是三大组件

集群物理层面：   ResourceManager
                NodeManager
APP层面：       ApplicationMaster

### ResourceManager(RM)

Yarn集群中的著角色，决定系统中所有应用程序之间的资源分配器的**最终**权限，接收用户的作业提交，并通过NM分配，管理机器上的计算资源

### NodeManager(NM)

Yarn中的从角色，一个机器上一个，负责管理本机器上的计算资源使用情况
根据RM命令，启动Container容器，监视容器的资源使用情况，并且向RM著角色汇报资源使用情况

### ApplicationMaster(AM)

用户提交的每个应用程序均包含一个AM
应用程序中的“老大”，负责程序内部个资源的申请，监督程序的执行情况
AM程序是应用程序内部启动的第一个程序

## Yarn程序提交流程

核心交互流程主要有四步：
MR作业提交  Client -> RM
资源的申请  MRAppMaster -> RM
MR作业状态汇报 Container(Map | Reduce Task) -> Container (MRAppMaster)
节点的状态汇报(Yarn集群内布)  NM ->RM 

### 整体概述

当用户向YARN中提交一个应用程序后，YARN将分为了两个阶段运行该应用程序
第一个阶段：客户端申请资源启动运行被刺程序的AppMaster
第二个阶段有AppMaster更具本次程序内部具体具体情况为她申请资源，并健康它的整个运行过程，知道运行完成

![Yarn程序提交流程](/img/posts/Yarn/Yarn程序提交流程.jpg)


第一步:用户通过客户端向Yarn中的ResourceManager提交应用程序（如Hadoop，jar提交MR程序）

第二步：RM为该应用程序分配第一个container，并于对应的NM同学，要求它再这个container中启动该应用的AM

第三步：AM启动成功后，向RM注册并保持同学，这样用户可以直接通过RM查看应用程序的运行状态

第四步：AM为本次程序内部的各个Task向RM申请资源
 
第五步：一旦**AM**申请到资源，便于对应的NM通信，要求他启动任务

第六版：NM为任务设置好运行环境之后，将任务启动命令写入到一个脚本之中，并通过运行该脚本启动任务

第七步：各个人物通过某个[RPC协议](https://baike.baidu.com/item/RPC%E5%8D%8F%E8%AE%AE/5019569?fr=aladdin)向AM汇报自己的状态和进度，以让AM随时掌握各个人物的运行状态，可以再任务失败时重新启动任务。
在应用程序运行过沉重，用户可随时通过RPC向AM查询应用程序当前运行状态

第八步：应用程序运行完成后，AM向RM注销并关闭自己

## Yarn的资源调度器Scheduler

在Yarn中，负责给应用分配资源的是schduler，是RM的内部核心组件之一。Schduler完全用于调度作业，无法跟踪应用程序的状态。Yarn提供了多种调度器和可配置的策略供选择，可在yarn-sit.xml中的yarn.resourcemanager.schduler.class进行配置


### FIFO Schduler

先入先出，拥有一个控制全局的队列Queue
不适合共享集群
### Capacity Scheduler

Apache版本一般默认Capacity Scheduler
**允许多个组织共享集群资源**，每个组织都可以获得一部分集群计算能力。通过为每个组织分配专门的队列，然后再为每个队列分配一定的集群资源，整个集群可以通过设置多个队列给多个组织提供服务。

Capacity 可以理解为一个个的资源队列，这个资源队列是用户自行分配。
再在队列内部进行垂直划分，使得一个组织内部分多个成员共享这个队列资源。
**在一个队列内部，资源调度是FIFO的**

![文件划分](/img/posts/Yarn/Scheduler_task_div.jpg)

### Fair Scheduler

提供了yarn应用程序中共享大型资源的方式

***
例如：
用户A、B都有自己的队列 -> A启动一个作业，而B还没有，此时A分配了集群中所有可用的资源 -> B在A仍在运行时启动了一个作业，一段时间后，A、B各自作业都使用了一半的资源 -> 若B此时再开启第二个作业，它将于B的另一个作业共享资源。A的一个作业仍占有1/2的资源，而B的两个作业各自占有1/4的资源
***

Fair Scheduler支持资源抢占、基于用户的映射
