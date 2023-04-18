---
title: Hive学习之路4-HQL的应用1-建表与show语句
top: false
cover: false
author: DULULU oO
date: 2022-08-22 22:15:22
password:
summary:
tags: 
    - Hive
categories: 数仓开发
---

SQL中DDL(Data Defeinition Language)语言，主要是Create，ALter，DROP。DDL并不涉及表内部操作。

## Hive中的数据库

在默认情况下，Hive的默认数据库default ,位于HDFS的/user/hive/warehouse目录下
用户自己创建的数据库位于/user/hive/warehouse/databse_name.db下

## 创建数据库

create databse用于创建新的数据库
COMMENT: 数据库的注释说明语句
LOCATION: 指定数据库再HDFS存储位置，默认为/user/hive/warehouse/dbname.db
With DBPROPERTIES: 用于指定一些数据库的属性配置

```SQL
CREATE (DATABASE|SCHEMA)[IF NOT EXISTS]database_name
[COMMENT database_comment]
[LOCATION hdfs_path]
[WITH DBPROPERTIES(property_name=property_value,...)];
```
## 切换数据库

use database 进行切换

## 删除数据库

要求数据库下没有标，为空时才可以删除

```SQL

DROP (DATABSE|SCHEMA)[IF EXSITS]database_name;

```
## 创建表

同理SQL

[可选内容]
```SQL
CREATE TABLE[IF NOT EXSITS][db_name.]table_name
(col_name datatype [COMMENT col_comment],...)
[COMMENT table_comment]
[ROW FORMAT DELIMITED...];
```
最低限度

```SQL
CREATE TABLE table_name (col_name datatype,...);
```

例
```SQL
create table test.t_archar(
    id int comment "ID编号",
    name string,
    hp_max int,
    mp_max int,
    attack_max int,
    defense_max int,
    attack_range string,
    role_main string,
    role_assist string
)
row format delimited
fields terminated by "\t";
```


### show语法

用于查看schemas，tables和databases

show tables;
show databases;
show schemas;
show tables in database1; // 此处表示展示database1中的表
show tables in schema1; // 此处表示展示schema1中的表

查看元数据类型
desc formatted table1;