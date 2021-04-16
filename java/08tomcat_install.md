# Tomcat8 安装配置教程
## 一、前言
本文是关于如何安装Tomcat以及怎样以及怎样配置使用的文档。  
注意：安装Tomcat之前一定要先[安装配置JDK](java/07jdk安装 "JDK安装配置")，否则无法运行。
## 二、安装
tomcat官方下载地址：[https://tomcat.apache.org/download-80.cgi](https://tomcat.apache.org/download-80.cgi)   
### 1、下载
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303152626.png)  
选择下载版本。（本文例子所用操作系统为64位，故下载64-bit Windows zip）  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303153334.png)  
### 2、安装
解压下载的Tomcat8压缩包  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303153542.png)  
解压后在bin文件夹中有exe安装文件，直接安装即可。  
Tomcat安装没有什么技巧，一直点击“下一步”即可。  
安装目录可自行选择，本例安装在F盘的Tomcat下：F:\Tomcat。  
## 三、启动tomcat服务
进入F盘Tomcat文件夹的bin子目录，双击打开startup.bat文件运行，弹出另一个控制台窗口，   
不停地在加载Tomcat服务信息最后一行会显示开启服务所用时间，如下图，表明Tomcat服务已经成功开启。  
验证结束后关闭此窗口即可。  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303153810.png)  
在浏览器地址栏输入：http://localhost:“端口号”/  回车，tomcat默认端口8080  进入Tomcat自带的web页面，  
包含Tomcat文档，表明Tomcat服务正常。  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303153842.png)  


