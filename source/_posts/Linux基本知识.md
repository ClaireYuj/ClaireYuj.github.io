---
title: Linux基本知识
top: false
cover: false
author: DULULU oO
date: 2022-03-16 17:12:29
password:
summary:
tags: Linux
categories: 
        - 操作系统
        - 面试
---

## linux 环境

Linux内核的源码开源网站(http://www.kernel.org)
Linux常用的发行版本： Ubuntu， CentOs， RedHat

## Linux 目录

~ home目录
/ root目录
/lib 系统使用的函数库的沐浴露
/etc 系统配置文件村对方的目录
/bin 二进制文件存放目录

## Linux 指令

### 目录指令

ls 列出目录下的文件 ls -a 显示包括隐藏目录 ls -l 显示文件的详细信息

mkdir 创建新的目录 /文件 mkdir -p

```Linux
mkdir test/test01.txt -p
```
若不存在test文件夹，会创建test文件夹后创建你test01文件

touch创建空白文件

```Linux
touch 01.txt
```

rm 删除 rm -f强制删除 rm -r删除目录及其下面的子文件

pwd 查询当前位置

cd 切换目录

mv 移动文件

cp 复制文件，若要同时移动该文件下的子文件要用cp -r
cp t.txt Document/t 
该命令将把文件t.txt复制到Document目录下，并命名为t。

### 查看文件中内容

cat 查看文件内容
```Linux
Cat 1.txt
```
查看1文件中的内容

more 查看大文件内容
```Linux
More 1.txt
```
当1为大文件时，可以用more查看，more支持分页查看（space切换下一页，b返回上一页）

tail 查看追加内容， 其中tail -f可以查看实时更新的结果（ctrl+c结束）
可以同时开两个窗口，在第一个窗口输入
```Linux
tail 1.txt
```
在第二各窗口输入
```Linux
date >> 1.txt
```
查看1文件中新追加的内容(当下时间)

echo 追加新的内容 
echo a > 1.txt  直接覆盖于
Echo a >> 1.txt 将a插入1.txt 文件的末端
echo a会直接将结果打印到控制台上

Date >> 1.txt 将当前时间插入文件中 date本身用于时间chakan

Cal 查看日历 cal >> 1.txt 将日历添加到1.txt中

### 管道命令

将一个命令的执行的结果作为内容提交给下一个命令处理， 类似嵌套
```Linux
    ps -ef | grep 1001
```

### 文件与进程交互命令

**grep 搜索**

grep命令是一种强大的文本搜索工具

使用实例：

ps -ef | grep sshd  查找指定ssh服务进程 
ps -ef | grep sshd | grep -v grep 查找指定服务进程，排除gerp身 
ps -ef | grep sshd -c 查找指定进程个数 

**find**：在目录结构总搜索文件，并对搜索结果进行指定的操作
ind 默认搜索当前目录及其子目录，并且不过滤任何结果（也就是返回所有文件），将它们全都显示在屏幕上。

使用实例：

find . -name "*.log" -ls  在当前目录查找以.log结尾的文件，并显示详细信息。 
find /root/ -perm 600   查找/root/目录下权限为600的文件 
find . -type f -name "*.log"  查找当目录，以.log结尾的普通文件 
find . -type d | sort   查找当前所有目录并排序 
find . -size +100M  查找当前目录大于100M的文件

find和grep都是在Linux系统中用于查找文件或字符串的命令，但它们的用途和功能是不同的。

find命令用于在指定的目录中查找文件，可以根据文件名、文件类型、文件大小、权限等属性进行过滤搜索。例如，可以使用以下命令在当前目录及其子目录中查找所有名为filename的文件：
```linux
find . -name filename
```
grep命令用于在指定的文件中查找包含指定字符串的行，可以根据字符串、正则表达式等进行过滤搜索。例如，可以使用以下命令在文件filename中查找包含字符串"searchtext"的行：
```linux
grep "searchtext" filename
```

### 压缩和解压文件
tar 解压
-c 创建一个新归档

-f 当与-c选项一起使用时，创建的tar文件使用该选项指定的文件名；当与-x选项一起使用时，则解除该选项指定的归档

-t 显示包括在tar文件中的文件列表

-v 显示文件的归档进度

-x 从归档中抽取文件

-z 使用gzip压缩tar文件

-j 使用bzip2压缩tar文件

**要创建一个tar文件，输入命令**：tar –cvf filename.tar directory/file/home/mine ： 将directory/file、/home/mine放入归档文件中。

要列出tar文件的内容，输入命令：

tar –tvf filename.tar

要抽取tar文件的命令，输入命令：

tar –xvf filename.tar

这个命令不会删除tar文件，但会把解除归档的内容复制到当前工作目录下，并保留归档文件所使用的任何目录结构。

**压缩文件**：tar默认不压缩文件。**要创建一个使用tar和bzip2来归档压缩的文件，使用-j选项**：

tar –cvfj filename.tbz file

**解压tbz格式**: tar -xvfj file.tar.tbz example.cpp //解压形成example.cpp的格式
**解压gz格式**：tar -zxvf  file.tar.gz  //解压到当前的文件夹


### netstat

查看网络状态

查看系统上的网络状态。
选项：
-a 显示所有正在或不在侦听的套接字
-n 显示数字形式地址而不是去解析主机、端口或用户名
-p 显示套接字所属进程的PID和名称
 
示例：
netstat -anp
netstat -anp | grep "进程名"
netstat -anp | grep "端口号" 



### 挂载后台

```linux
nohup command &

```

### 查找进程

```linux
ps -ef
```

可以显示用户所有进程

```linux
ps-ef | grep firefox
```
查找所有有火狐的进程



```linux
pkill firefox 
```
or
```linux
kill 12345
```
都可以杀死进程