---
title: 利用Gradle自动打包Android应用
tags: Android
---


除了在Android Studio下进行打包签名的操作，为了不用每次打包都要打开项目在打包的过程，通过利用Gradle脚本来实现apk打包，签名，自定义文件名，多渠道打包的自动打包功能

## 一、添加签名脚本,指定签名文件相关配置

```java
    //签名配置  
    signingConfigs {  
        debug {  
                keyAlias 'keyAlias' //key别名  
                keyPassword '123456' //key密码,最好不要配置在脚本里  
                storeFile file('/home/android/sign/androidkey.jks') //证书路径,证书最好不要放入项目源码  
                storePassword '123456' //证书密码,最好不要配置在脚本里  
            }  
        release {  
            keyAlias 'keyAlias' //key别名  
            keyPassword '123456' //key密码,最好不要配置在脚本里  
            storeFile file('/home/android/sign/androidkey.jks') //证书路径,证书最好不要放入项目源码  
            storePassword '123456' //证书密码,最好不要配置在脚本里  
        }  
    }  
```
## 二、添加编译类型脚本(AS会自动生成默认的release配置)

```java
    //编译类型  
    buildTypes {  
    
        debug {  
                minifyEnabled false  
                proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'  
                signingConfig signingConfigs.debug //加入签名配置  
            } 
    
        release {  
            minifyEnabled false  
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'  
            signingConfig signingConfigs.release //加入签名配置  
        }  
    }  
```
## 三、添加产品风格配置，用于多渠道打包

```java
    //产品风格配置,可用于多渠道打包  
    productFlavors {  
        //百度推广渠道  
        baidu {  
            applicationId "com.dhq.baidu"  //应用id
            versionCode 1  
            versionName "1.0"  
            manifestPlaceholders = [UMENG_CHANNEL_VALUE: "baidu"]  
      manifestPlaceholders = [  APP_NAME  : "app-百度"] //自定义App名称,需要把AndroidManifest.xml里的label改为: android:label="${APP_NAME}"  
        }  
        //360推广渠道  
        qh360 {  
            applicationId "com.dhq.qh"  
            versionCode 1  
            versionName "1.0"  
            manifestPlaceholders = [UMENG_CHANNEL_VALUE: "qh360"]  
            manifestPlaceholders = [  APP_NAME  : "app-360"] //自定义App名称,需要把AndroidManifest.xml里的label改为: android:label="${APP_NAME}"  
        }  
    }  
```

## 四.自定义apk名称

在本例中我们以如下规则命名：
module_flavor-version-time-buildtype.apk

- build.gradle里定义获取当前时间方法
```java
    //获取当前时间  
    def getCurrentTime() {  
        return new Date().format("yyyy-MM-dd", TimeZone.getTimeZone("UTC"))  
    }  
```
- 在android的android.applicationVariants.all方法里修改apk文件名
```java
    //apk文件重命名  
    android.applicationVariants.all { variant ->  
        variant.outputs.each { output ->  
            def outputFile = output.outputFile  
            if (outputFile != null && outputFile.name.endsWith('.apk')) {  
                def buildType = variant.buildType.name  
                //这里修改apk文件名,格式为 module_flavor-version-time-buildtype.apk  
                def fileName = "app_${variant.productFlavors[0].name}-V${defaultConfig.versionName}-${getCurrentTime()}-${buildType}.apk" 
                
                //output.outputFile = new File(outputFile.parent, fileName)
                //gradle 4.1以后用这个方式，之前的用上面的方式给apk命名
                outputFileName = fileName
            }  
        }  
    }  
```
## 五、编译命令
- 编译所有productFlavor及对应所有buildType的apk
```java
$gradle assemble   //仅仅执行项目打包所必须的任务集
$gradle build      //执行项目打包所必须的任务集,以及执行自动化测试,所以会较慢
```
- 编译指定productFlavor及buildType的apk
```java
$gradle assemble
$gradle assembleflavorRelease 
$gradle assembleflavorDebug
注:gradle支持命令缩写，上面两个命令也可以写成如下格式
$gradle a
$gradle ass
$gradle aR
$gradle assflavorR
$gradle aD
$gradle assflavorD
```
## 六、编写编译脚本（bat文件 Linux对应sh文件）
```java
% 快速编译打包apk脚本  %
    echo  " **************************打包开始 ************************** "  
    sleep 1  
    % 执行打包命令前,需要先定位到项目根目录（如果放到根目录下可以不写这一步）%  
    cd ..  
    %执行打包命令 此处命令可以根据上面的介绍适当修改 %
    gradle a
      
    echo -e "**************************打包完成************************** "  

    % 桌面右上角弹出通知 % 
    notify-send build.sh "打包完成!"  
```
## 七、不足
此处只是简单实现gradle的自动打包功能、应做成可视化打包工具、签名外置、线上打包等功能还需要研究一下

