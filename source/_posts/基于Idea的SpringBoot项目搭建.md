---
title: 基于Idea的SpringBoot项目搭建
top: false
cover: false
author: DULULU oO
date: 2021-11-17 23:40:31
password:
summary:
tags: 
    - Maven
    - JAVA
categories: 环境配置
---

## 安装Spring Boot Helper插件

File > Settings > Plugins >

安装好之后File > new > Spring Initializr > 此时注意到选择初始化服务器的defalut那有一个start.spring.io
File > Appearance $ Beahavior > System Settings >HTTP Proxy > 选择 Auto-detect后点击左下角的check connection按钮，输入http://start.spring.io

## 建立项目

版本选择 2.2.6.RELEASE
包的名字记得改

## 配置Maven

File > Buil, Execution, Deployment < Build Tools > Maven 配置maven的setting.xml于repository， 在maven的安装路径下conf文件夹中找到settings.xml，再在maven根目录新建repository文件夹，并在 setting.xml 文件中添加（找到repository注释下一行，进行添加）

```XML
    <localRepository>
        D:\Environment\Maven\apache-maven-3.2.5\store
        </localRepository>
```
此处地址为自己创建的repository的地址

再找到mirror
将原代码更换为
```XML
    <mirror>  
      <id>alimaven</id>  
      <name>aliyun maven</name>  
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>  
      <mirrorOf>central</mirrorOf>          
     </mirror> 

```

再重新生成maven项目，拉开idea右侧maven栏，点击左上角刷新按钮

### 报错java: 程序包org.springframework.boot不存在

是依赖库仓库出现问题，cd到该项目目录下，mvn idea:idea 重新安装依赖即可