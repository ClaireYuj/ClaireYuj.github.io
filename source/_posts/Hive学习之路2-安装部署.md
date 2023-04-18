---
title: Hive学习之路2--安装部署
top: false
cover: false
author: DULULU oO
date: 2022-08-19 23:59:14
password:
summary:
tags:
    - Hive
categories: 数仓开发
---

参考：https://www.bilibili.com/video/BV1CU4y1N7Sh?p=60&spm_id_from=pageDriver&vd_source=f618be285e44e05f2b481fa28b9fe6ad 


在Linux环境下，保证Hadoop集群健康可用
在启动Hive之前必须先启动Hadoop集群，**需等待HDFS安全模式关闭之后再运行Hive**

```shell
start-dfs.sh
start-yarn.sh
```


Hive不是分布式安装运行的软件，其分布式特效主要借助于Hadoop完成，包括分布式存储，分布式计算

## Hadoop与Hive
需要在Hadoop中添加相关配置属性，以满足Hive在Hadoop上运行
修改Hadoop中core-site.xml,并且Hadoop集群同步配置文件重启生效


<!--整合Hive代理设置 -->
```xml
    <property>
        <name>hadoop.proxyuser.root.hosts</name>
        <value>*</value>
    </property>
    <property>
        <name>hadoop.proxyuser.root.groups</name>
        <value>*</value>
    </property>
```

## MySQL安装

1. 卸载Centos7自带的mariadb
  ```shell
  rpm -qa|grep mariadb
  # 结果mariadb-libs-5.5.64-1.el7.x86_64
  rpm -e mariadb-libs-5.5.64-1.el7.x86_64 --nodeps
  # 进行删除
  rpm -qa|grep mariadb                            
  # 再次查询，无结果
  ```

2. 安装mysql

  ```shell
  # 创建目录
  mkdir /export/software/mysql
  
  # 选择finalShell，找到/export。software/mysql 拖拽文件上传mysql-5.7.29-1.el7.x86_64.rpm-bundle.tar

  #到上述文件夹下  解压
  tar xvf mysql-5.7.29-1.el7.x86_64.rpm-bundle.tar
  
  #执行安装依赖
  yum -y install libaio
  
  # 进行mysql的安装
  rpm -ivh mysql-community-common-5.7.29-1.el7.x86_64.rpm    mysql-community-libs-5.7.29-1.el7.x86_64.rpm    mysql-community-client-5.7.29-1.el7.x86_64.rpm      mysql-community-server-5.7.29-1.el7.x86_64.rpm 
  
  warning: mysql-community-common-5.7.29-1.el7.x86_64.rpm: Header V3 DSA/SHA1 Signature, key ID 5072e1f5: NOKEY
  Preparing...                          ################################# [100%]
  Updating / installing...
     1:mysql-community-common-5.7.29-1.e################################# [ 25%]
     2:mysql-community-libs-5.7.29-1.el7################################# [ 50%]
     3:mysql-community-client-5.7.29-1.e################################# [ 75%]
     4:mysql-community-server-5.7.29-1.e################                  ( 49%)
  ```

3. mysql初始化设置

  ```shell
  #初始化
  mysqld --initialize
  
  #更改所属组
  chown mysql:mysql /var/lib/mysql -R
  
  #启动mysql
  systemctl start mysqld.service
  
  #查看生成的临时root密码
  cat  /var/log/mysqld.log
  
  [Note] A temporary password is generated for root@localhost: o+TU+KDOm004
  ```

4. 修改root密码 授权远程访问 设置开机自启动

  ```shell
  # 登录mysql
  mysql -u root -p

  # Enter password:     #这里输入在日志中生成的临时密码
  
  # Welcome to the MySQL monitor.  Commands end with ; or \g.
  # Your MySQL connection id is 3
  # Server version: 5.7.29
  
  Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.
  
  Oracle is a registered trademark of Oracle Corporation and/or its
  affiliates. Other names may be trademarks of their respective
  owners.
  
  Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
  
  mysql> 
  
  
  #更新root密码  设置为hadoop
  mysql> alter user user() identified by "hadoop";
  Query OK, 0 rows affected (0.00 sec)
  
  
  #授权
  mysql> use mysql;
  
  mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'hadoop' WITH GRANT OPTION;
  
  mysql> FLUSH PRIVILEGES; 

  # ctrl+D 结束 退出mysql
  
  #mysql的启动和关闭 状态查看 （这几个命令必须记住）
  systemctl stop mysqld
  systemctl status mysqld
  systemctl start mysqld
  
  #建议设置为开机自启动服务
  systemctl enable  mysqld    

  #Created symlink from /etc/systemd/system/multi-user.target.wants/mysqld.service to /usr/lib/systemd/system/mysqld.service.
  
  #查看是否已经设置自启动成功
  systemctl list-unit-files | grep mysqld
  
  # mysqld.service enabled 
  ```

## Hive安装

- 上传安装包 解压

  ```shell
  tar zxvf apache-hive-3.1.2-bin.tar.gz
  ```
  
- 解决Hive与Hadoop之间guava版本差异

  ```shell
  cd /export/server/apache-hive-3.1.2-bin/
  rm -rf lib/guava-19.0.jar
  cp /export/server/hadoop-3.3.0/share/hadoop/common/lib/guava-27.0-jre.jar ./lib/
  ```

- 修改配置文件

  - hive-env.sh

    ```shell
    cd /export/server/apache-hive-3.1.2-bin/conf
    mv hive-env.sh.template hive-env.sh
    
    vim hive-env.sh

    ## 大G小o 跳转到最后一行
    export HADOOP_HOME=/export/server/hadoop-3.3.0
    export HIVE_CONF_DIR=/export/server/apache-hive-3.1.2-bin/conf
    export HIVE_AUX_JARS_PATH=/export/server/apache-hive-3.1.2-bin/lib
    # Esc(退出输入模式)+shift+zz 快速保存

    ```

 - hive-site.xml

    ```shell
    vim hive-site.xml
    ```

    ```xml
    <configuration>
    <!-- 存储元数据mysql相关配置 -->
    <property>
    	<name>javax.jdo.option.ConnectionURL</name>
    	<value>jdbc:mysql://node1:3306/hive3?createDatabaseIfNotExist=true&amp;useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8</value>
    </property>
    
    <property>
    	<name>javax.jdo.option.ConnectionDriverName</name>
    	<value>com.mysql.jdbc.Driver</value>
    </property>
    
    <property>
    	<name>javax.jdo.option.ConnectionUserName</name>
    	<value>root</value>
    </property>
    
    <property>
    	<name>javax.jdo.option.ConnectionPassword</name>
    	<value>hadoop</value>
    </property>
    
    <!-- H2S运行绑定host -->
    <property>
        <name>hive.server2.thrift.bind.host</name>
        <value>node1</value>
    </property>
    
    <!-- 远程模式部署metastore metastore地址 -->
    <property>
        <name>hive.metastore.uris</name>
        <value>thrift://node1:9083</value>
    </property>
    
    <!-- 关闭元数据存储授权  --> 
    <property>
        <name>hive.metastore.event.db.notification.api.auth</name>
        <value>false</value>
    </property>
    </configuration>
    
    ```
- 上传mysql jdbc驱动到hive安装包lib下

  ```shell
  mysql-connector-java-5.1.32.jar
  ```

- 初始化元数据

检验安装是否正确
  ```shell
  cd /export/server/apache-hive-3.1.2-bin/
  
  bin/schematool -initSchema -dbType mysql -verbos
  #初始化成功会在mysql中创建74张表
  ```
  
- 在hdfs创建hive存储目录（如存在则不用操作）

  ```shell
  hadoop fs -mkdir /tmp
  hadoop fs -mkdir -p /user/hive/warehouse
  hadoop fs -chmod g+w /tmp
  hadoop fs -chmod g+w /user/hive/warehouse
  ```

  ## metastore启动

  ### 前台启动

  进程会一直占据终端，ctrl+c结束进程

  - 启动前台
  ```shell 
  /export/server/appache-hive-3.1.2-bin/bin/hive --service metastore
  ```

  - 前台启动开启debug日志
  ```shell
  /export/server/apache-hive-3.1.2-bin/bin/hive --service metastore --hiveconf hive.root.logger=DEBUG,console 
  ```

  ### 后台启动

  - 把程序当成一个进程

  ```shell
  nohup /export/server/apache-hive-3.1.2-bin/bin/hive --service metastore &
  ```
  回车后再按一次回车，就成功启动在后台了

  之后通过jps 查看进程

  使用kill -9杀死进程

