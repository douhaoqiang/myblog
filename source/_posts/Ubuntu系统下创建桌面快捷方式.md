---
title: Ubuntu系统下创建桌面菜单
---

## 一、对于免安装软件
- 在桌面目录下创建{软件名}.desktop文件
- 使用命令 gedit {软件名}.desktop 打开文件写入一下内容

    ```java
    [Desktop Entry]
    Encoding=UTF-8
    Name=AndroidStudio
    Comment=Android IDE
    Exec=sh /usr/local/androidstudio/bin/studio.sh  #根据软件的具体执行路径
    Icon=/usr/local/androidstudio/bin/studio.png  #根据软件的图标
    Terminal=false  #软件打开时是否启动终端
    StartupNotify=false
    Type=Application
    Categories=Application;Development;
    ```
- 在这个快捷方式上右键 进入设置 给快捷方式添加执行权限，以后就可以双击打开该软件了

## 二、对于安装的软件
- 在/usr/share/applications 目录下找到想要创建快捷方式的软件对用的desktop文件
- 使用复制命令将该文件复制到桌面目录下。
cp /usr/share/applications/{软件}.desktop  /home/user/Desktop/
- 在这个快捷方式上右键 进入设置 给快捷方式添加执行权限，以后就可以双击打开该软件了


