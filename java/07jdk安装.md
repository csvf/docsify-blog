# Jdk安装图文教程

目录详细见右边栏

## 一、前言

本文是关于如何安装JDK以及怎样以及怎样配置使用的文档。请准备一台windows 10 （x64位） 系统电脑。

## 二、安装

jdk官网下载地址: [https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)

### 1、下载
根据操作系统下载对应的最新版本 download标签有详细展示，我是windows10 64位 下载下面红框的版本  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/01.png)  
勾选复选框，点击开始Download绿色模块开始下载，（注没有账号的需要先注册账号）。  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303133429.png)  

### 2、安装

1. 在D盘 创建一个文件夹，文件夹名为java
2. 在D盘下java文件夹里，创建两个文件夹名字分别为JDK、JRE
3. 双击下载的.exe 文件开始安装JDK+JRE

#### 2.1 JDK安装
点击下一步开始安装JDK  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303134206.png)  
点击更改安装目录  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303134247.png)  
选择对应创建的D盘的文件  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303134347.png)  
等待安装  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303134425.png)  
#### 2.2 JRE安装
JDK安装完后自动跳出JRE的安装窗口  
点击更改目录  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303134519.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303134535.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303134552.png)  
更改成功，点击下一步  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303134616.png)  
等待JRE安装完成  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303134640.png)  

## 三、验证JDK
安装完成后测试JDK是否安装成功  
1. 打开运行窗口 win+R，输入cmd 回车，打开Dos窗口
2. 输入java 查看是否安装成功，若出现下图提醒则代表没问题

![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303134858.png)

## 四、配置环境变量
1. 桌面-----计算机----右键----属性------点击左边中间位置的高级系统设置

![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303144914.png)

2. 点击环境变量

![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303144952.png)

3. 在上面的窗口下新建JAVA_HOME,变量值为D盘jdk的安装目录;

![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303145057.png)

![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303145130.png)

4. 创建CLASSPATH,变量值为 英文的点（中文的句号）例 .;%JAVA_HOME%\lib;

![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303150311.png)

5. 在系统变量中找到Path变量 ，在变量值的最前面加上JDK的bin目录并加上一个英文的分号 例 ;%JAVA_HOME%\bin;

![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303151211.png)

![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303151404.png)

6. 测试：
Win键（ctrl和alt之间的键位）+  r 进入运行  
输入cmd 进入界面  
```
	1、运行java
	2、运行javac
```
输入javac出现以下界面配置成功  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303145408.png)

