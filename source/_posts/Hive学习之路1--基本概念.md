---
title: Hive学习之路1--基本概念
top: false
cover: false
author: DULULU oO
date: 2022-08-19 20:21:21
password:
summary:
tags: 
    - Hive
categories: 数仓开发
---

## Hive的优点

1.接口操作采用类SQL语法，提供快速开发的能力，简单且容易上手
2.擅长存储分析海量数据，与Hadoop相似
3.避免直接写MapReduce，减少开发人员学习成本

Hive利用HDFS存储数据，利用MapResuce查询分析数据

## Hive功能的实现

1. Hive能将结构化文件映射成一张表，Hive并不承担存储数据功能，存储数据是由HDFS实现(将元数据信息描述清楚，转化成一个表)

2. 用户写完sql后，Hive对sql进行校验，并且更具元数据信息解读sql背后的含义，最后将执行计划转换成MApReduce程序来具体执行

有点类JDBC？
Hive是基于Hadoop的数仓工具 ![Hive工作原理](\img\posts\DataRepos\Hive1.jpg)

## Hive架构

![Hive架构](\img\posts\DataRepos\Hive2.jpg)

用户接口：包括CLI。JDBC/ODBC，WebGUI

元数据(metadata)存储 mysql/derby Hive中的元数据(描述数据的数据)包括表明，列的分区及属性，表的属性（是否为外部表等），表的数据所在的目录等

Driver驱动程序：包括语法解析器，计划编译器，优化器，执行器

执行引擎：MapReduce/Tez/Spark，Hive并不直接处理数据，而是通过执行引擎处理数据

Hadoop Yarn

HDFS/HBase

## Hive与元数据

Hive的安装模式与元数据服务(metastore)有关

metasore服务器配置有三种模式内嵌模式、本地模式、**远程模式**

![metastore](\img\posts\DataRepos\Hive3.jpg)