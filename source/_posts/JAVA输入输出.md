---
title: JAVA输入输出
top: false
cover: false
author: DULULU oO
date: 2021-11-17 14:37:32
password:
summary:
tags: JAVA
categories: 基础知识
---

# JAVA I/O

在Java程序中要读取外部IO设备中的内容，要求先将数据传输到内存中，这一步需要借助操作系统实现。
JDK中提供的IO操作框架，根据流的传输方向和读取单位分为字节流InputStream和outputSteam以及字符流Reader和Writer

字节流一次**读取一个字节**（1 byte）
字符流一次**读取一个字符**（1 char =  2 byte）

# 文件流 

## 文件字节流


头文件
```Java
import java.io.FileInputStream;
import java.io.FileNotFoundException;
```

### 创建输入流
```Java
try{
    FileInputStream inputStream = new FileInputStream("test.txt"); 
}catch(FileNotFoundException e){
    e.printStackTrace();
}
```

在使用完成后，要关闭流进行资源释放，在catch后加入finally
```Java
        FileInputStream inputStream = null;
        try{
            inputStream = new FileInputStream("test.txt");
        }catch (FileNotFoundException e){
            e.printStackTrace();
        }finally {
            try{
                if(inputStream !=null) inputStream.close();
            }catch (IOException e){
                e.printStackTrace();
            }
        }
```

**但这样过于繁琐，尝试以下面方式进行简化**
```Java
    try(FileInputStream inputStream  = new FileInputStream("test.txt")){
            
    }catch (FileNotFoundException e){
            e.printStackTrace();
    }
```

将inputStream的初始化放到的括号中且不报错，原因是InputStream这个抽象类使用了Closeable的接口
而Closeable接口继承了AutoCloseable，如果一个对象是在try-with-resource(在JDK7中引入，JDK9中改进)代码块中生命的，则AutoCloseable的对象的close()方法会自动执行
字节流，字符流等都可以直接使用

### 读取文件

inputStream.read() 一次读取一个英文字母（读的是ASCII码，需要char转换）

一次性全部读完
```Java
try( FileInputStream inputStream = new FileInputStream("test.txt")){
            int tmp;
            while((tmp = inputStream.read()) != -1){
                System.out.println((char)tmp);
            }
        }catch (IOException e){
            e.printStackTrace();
        }
```