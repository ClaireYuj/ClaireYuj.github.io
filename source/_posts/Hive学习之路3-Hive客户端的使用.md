---
title: Hive学习之路3-Hive客户端的使用
top: false
cover: false
author: DULULU oO
date: 2022-08-21 14:13:43
password:
summary:
tags: 
    - Hive
categories: 数仓开发
---

推荐使用第二代客户端$HIVE_Home/bin/beeline，是一个JDBC客户端
官方强烈推荐的Hive命令行工具，和第一代客户端相比，性能安全性提高

![Hive客户端与服务的关系](/img/posts/DataRepos/hive客户端1&2.jpg)
图源：https://www.bilibili.com/video/BV1CU4y1N7Sh?p=62&spm_id_from=pageDriver&vd_source=f618be285e44e05f2b481fa28b9fe6ad

## HiveServer2服务

在远程模式下，启动HiveServer2必须先启动mtastore服务
Beeline客户端只能通过HiveServer2服务访问Hive问bin/hive是通过一代服务(metastore)访问的

```shell
nohup /export/server/apache-hive-3.1.2-bin/bin/hive --service metastore &

nohup /export/server/apache-hive-3.1.2-bin/bin/hive --service hiveserver2 &
```

### 其他机器连接一代客户端(metastore)

将客户端拷贝到其他机器上，此处以node3为例

```shell
scp -r /export/server/apache-hive-3.1.2-bin root@node3:/export/server/
```

再在node3的finalshell中输入
```shell
/export/server/apache-hive-3.1.2-bin/bin/hive
```
但此时连接的是metastore， 是第一代客户端
进入hive>
可以用show dtabases; show tables;分别查看数据库和表

ctrl+c退出hive
### 其他机器连接二代客户端(HiveServer2)

将beeline客户端连接到hive服务器

在node3的finalshell中输入

```shell
/export/server/apache-hive-3.1.2-bin/bin/beeline
```

- 进入beeline

- 输入 beeline> ! connect jdbc:hive2://node1:10000
    连接到hive服务
    输入用户名：root
    密码直接回车

- 连接成功
    可以直接输入 show databases;
                show tables;
    进行基本查询


## Hive可视化客户端

**DataGrip**,Dbeaver, Squirrel SQL client等

### DataGrip的使用

- 创建项目

- attach directory to project
    这样之后写的sql文件都存在这个目录下

- 关联数据库
    选择右边栏database-> 点击+号 -> data source -> Apache Hive

    点开后，左边选择栏再选择Hive，配置Hive启动jar，配置好后，点击上方localhost，更改host为node1，user名为root，之后测试connection，，显示ok后，点击apply



### 测试是否连接成功

- 在node1机器的finalshell中输入jps
runjar在运行

- 在datagrip中右侧的hive连接，此处是node1_hive，点击后按住（Fn+f4)或直接(f4)，进入命令行输入模式，输入show databases;选中后点击左上方绿色按钮进行运行。

成功后和说明环境配置成功。

### 正式写sql文件

将结构化语句映射到表中

- 新建文件“1.create_table.sql”

- 写建表语句

```sql
create table test.t_archar(
    id int,
    name string,
    hp_max int,
    mp_max int,
    attack_max int,
    defense_max int,
    attack_range string,
    role_main string,
    role_assist string
)
```

- 指定字段之间的分隔符

若不用分隔符，只有create table，会采用默认分隔符'\001'
\001是打不出来的，在vim编辑器的输入模式下是^A
用空格(\t)表示分隔符

```sql
row format delimited
fields terminated by "\t"
```

完整语句

```sql
create table test.t_archar(
    id int,
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

- 切换到对应的schema

![切换](/img/posts/DataRepos/Datagrip_switchSchema.jpg)

### 上传原数据

1. 暴力上传

进入node1:9870， browse file-> user->hive ->warehouse ->test.db ->upload

之后进入datagrip用select 语句进行查看

2. 通过将本地文件再finalshell中

```shell
hadoop fs -put 1.txt /user/hive/warehouse/test.db/t_1/
```
此处是将1.txt的淑君放到test数据库下的t_1表中


然后再beeline/datagrip中用select语句可以查看


3. 用load加载（推荐）
load命令是一个纯复制纯移动的树，hive不会对数据做任何形式的改变
语句格式如下
```SQL
LOAD DATA[LOCAL] INOATH 'filepath' [OVERWRITE] INTO TABLE tablename;
```

- Local的本地--如果是对Hiveserver2服务器所在的机器使用此命令，local本地文件系统指的是Hiveserver2服务所在机器的本地linux文件系统，而非Hive客户端所在的本地文件系统。
也就是node1上的本地文件系统

 

    