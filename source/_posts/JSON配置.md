---
title: JSON配置
top: false
cover: false
author: DULULU oO
date: 2022-01-14 10:43:38
password:
summary:
tags:
    - JSON
    - maven
categories: 基础知识
---

## Maven 项目中以来的配置
在maven项目中的pom.xml
该项目有
<groupId>com.example</groupID>  //一般是公司名
<artifactID>project</artifactID>  //一般是项目名
<version>1.0</version>  

如果要配置依赖，需要在所有依赖外面先加dependencies，如

```xml

<dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>net.sf.json-lib</groupId>
            <artifactId>json-lib</artifactId>
            <version>2.4</version>
            <classifier>jdk15</classifier>
        </dependency>

        <!-- https://mvnrepository.com/artifact/com.alibaba/fastjson -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>2.0.1</version>
        </dependency>

    </dependencies>
```

若配置了依赖，maven install时仍然报错，勾选settings->buid,execution,deployment中buol tolls下的maven下的runner，勾选嘴上的delegate IDE build/run action to Maven

![Delegate IDE build/run action to Maven](/img/posts/config/trust_maven.jpg)


记得配置maven的setting file和repository

## 配置JSON

在maven项目中的pom文件中先配置相关依赖

```xml
 <!-- https://mvnrepository.com/artifact/com.alibaba/fastjson -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>2.0.1</version>
        </dependency>

```

然后到java程序中

```JAVA

import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONArray;
import com.alibaba.fastjson.JSONObject;

```