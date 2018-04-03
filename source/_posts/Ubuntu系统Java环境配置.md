---
title: Ubuntu下Java环境配置
tags: Ubuntu
---

## 一、Java 环境配置
- 去oracle官网下载安装包
[jdk下载地址](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 
- 解压包到要安装的目录 sudo tar xvf jdk-8u25-linux-x64.tar.gz
- 输入指令打开配置文件 gedit  /etc/profile 在文件末尾添加一下配置
```java
export JAVA_HOME=~/{对应自己jdk存放目录}/jdk1.8.0_112
export JRE_HOME=${JAVA_HOME}/jre
export  CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```
- 保存退出，然后输入下面的命令来使配置生效
source  ~/.profile

- 输入java -version查看版本 如果现实版本信息表示设置成功
