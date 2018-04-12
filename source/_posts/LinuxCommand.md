---
title: Linux常用指令
date: 2018-04-11 14:53:07
tags: Linux

---

# 关机重启（shutdown）

- shutdown -hnow，立即关机
- shutdown -h+10，10 分钟后关机
- shutdown -h23:30 23:30 关机
- shutdown -rnew 立即重启

# 压缩解压缩（zip,tar）
- zip mytest.zip /opt/test/，把 /opt 目录下的 test/ 目录进行压缩，压缩成一个名叫 mytest 的 zip 文件
- unzip mytest.zip，对 mytest.zip 这个文件进行解压，解压到当前所在目录
- unzip mytest.zip -d /opt/setups/，对 mytest.zip 这个文件进行解压，解压到 /opt/setups/ 目录下
- tar -cvf mytest.tar mytest/，对 mytest/ 目录进行归档处理（归档和压缩不一样）
- tar -xvf mytest.tar，释放 mytest.tar 这个归档文件，释放到当前目录
- tar -xvf mytest.tar -C /opt/setups/，释放 mytest.tar 这个归档文件，释放到 /opt/setups/ 目录下

# 文件查询
- ls，列出当前目录下的所有没有隐藏的文件 / 文件夹。
- ls -a，列出包括以．号开头的隐藏文件 / 文件夹（也就是所有文件）
- ls -R，显示出目录下以及其所有子目录的文件 / 文件夹（递归地方式，不显示隐藏的文件）
- ls -a -R，显示出目录下以及其所有子目录的文件 / 文件夹（递归地方式，显示隐藏的文件）
- ls -al，列出目录下所有文件（包含隐藏）的权限、所有者、文件大小、修改时间及名称（也就是显示详细信息）
- ls -ld 目录名，显示该目录的基本信息
- ls -t，依照文件最后修改时间的顺序列出文件名。
- ls -F，列出当前目录下的文件名及其类型。以 / 结尾表示为目录名，以 * 结尾表示为可执行文件，以 @ 结尾表示为符号连接
- ls -lg，同上，并显示出文件的所有者工作组名。
- ls -lh，查看文件夹类文件详细信息，文件大小，文件修改时间
- ls /opt | head -5，显示 opt 目录下前 5 条记录
- ls -l | grep '.jar'，查找当前目录下所有 jar 文件
- ls -l /opt |grep "^-"|wc -l，统计 opt 目录下文件的个数，不会递归统计
- ls -lR /opt |grep "^-"|wc -l，统计 opt 目录下文件的个数，会递归统计
- ls -l /opt |grep "^d"|wc -l，统计 opt 目录下目录的个数，不会递归统计
- ls -lR /opt |grep "^d"|wc -l，统计 opt 目录下目录的个数，会递归统计
- ls -lR /opt |grep "js"|wc -l，统计 opt 目录下 js 文件的个数，会递归统计
- ls -l，列出目录下所有文件的权限、所有者、文件大小、修改时间及名称（也就是显示详细信息，不显示隐藏文件）。显示出来的效果如下：





