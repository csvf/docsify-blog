# Maven安装配置教程
## 一、前言
Maven是一个项目管理和综合工具，能很方便的管理JAR文件。
## 二、安装
### 1、下载
Maven官网下载地址: [https://maven.apache.org/download.cgi#](https://maven.apache.org/download.cgi#)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303161232.png)  
### 2、安装
下载后直接解压到对应的目录，如：D: \apache-maven-3.5.3   
## 三、配置
### 1、配置环境变量
开始配置环境变量：  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303161447.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303161509.png)  
添加系统变量MAVEN_HOME,变量值是你的maven存放路径：  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303161552.png)  
然后在Path中添加 %MAVEN_HOME%\bin  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303161617.png)  
测试安装成功，打开命令提示符，输入 mvn -version   (maven所有命令都是mvn)   
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303161658.png)  
MAVEN配置成功  
### 2、配置setting文件
修改D: \apache-maven-3.5.3\conf 路径下的settings.xml配置文件:  
如果是官网下载的maven-3.5.3：配置文件为空，需将以下配置文件代码全部替换复制进去  
(localRepository填写自己的实际路径）  
1. localRepository：构建系统本地仓库的路径,其默认值：~/.m2/repository  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303162122.png)  
改为自己的本地仓库地址 
```xml
 <localRepository>D:\maven_jar</localRepository>
```  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303162334.png)   
远程仓库改为阿里云的镜像  
```xml
  <mirrors>
       <mirror>
            <id>nexus-aliyun</id>
            <mirrorOf>*,!jeecg,!jeecg-snapshots</mirrorOf>
            <name>Nexus aliyun</name>
            <url>http://maven.aliyun.com/nexus/content/groups/public</url>
        </mirror>
 </mirrors>  
```
## 四、IDEA中MAVEN配置
### 1、单个项目配置Maven
File->Settings->搜索框输入Maven->点击Maven修改配置  
PS：以下Maven配置以ftp下载下来的maven-3.5.3路径为例子，官网下载的maven-3.5.3配置方法相同  
Maven home directory：D: \apache-maven-3.5.3  
User Settings file：D: \apache-maven-3.5.3 \conf\settings.xml  
Local repository：D:\ apache-maven-3.5.3 \.m2\repository  
【注意：这里路径，根据自己实际路径配置】  
点击apply，然后点击OK，配置完成。  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303162836.png)  
注意：以上配置只针对当前maven项目生效。  
### 2、所有项目配置Maven
想要对所有maven项目生效，则要在   
File->Other Settings->Default Settings 搜索框输入Maven->点击Maven修改配置   
Maven home directory：D: \apache-maven-3.5.3  
User Settings file：D: \apache-maven-3.5.3 \conf\settings.xml  
Local repository：D: \apache-maven-3.5.3 \.m2\repository   
【注意：这里路径，根据自己实际路径配置】  
点击apply，然后点击OK，配置完成，以后再新建maven项目配置都会生效。  
此时，新建一个maven项目，可看到地址已变成我们settings.xml中设置的地址。  
在IDEA中简单使用maven命令，可直接在此处输入maven命令：  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303163014.png)  
### 3、maven常用命令
也可在此处输入命令：  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210303163041.png)  
```shell script
创建Maven项目：mvn archetype:generate  
编译源代码： mvn compile   
清除产生的项目：mvn clean   
Maven项目打包： mvn package   
在本地Repository中安装jar： mvn install 
```  
想学习其他命令，可自行百度~   

