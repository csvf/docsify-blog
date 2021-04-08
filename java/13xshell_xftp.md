# linux服务器连接工具 xshell 和 xftp 安装配置教程

## 一、前言
xshell用来在windows界面下访问远端不同系统下的服务器，从而实现较好地远程控制终端的目的。  
xshell是一个强大的安全终端模拟软件，它支持SSH1、SSH2、TELNET等多种协议。
## 二、安装
### 1、下载
下载xshell linux远程连接工具  和  xftp 上传工具安装程序，教育版(免费)，需要填写个人邮箱信息。  
官网下载地址 [https://www.netsarang.com/zh/free-for-home-school/](https://www.netsarang.com/zh/free-for-home-school/)
### 2、xshell安装
点击xshell5.exe进行安装，等待一会后，出现以下界面，一直点击下一步  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210315180333.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210315180357.png)  
选择我接受许可证协议的条款，然后点击下一步   
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210315180456.png)  
输入用户名 随意填写，点击下一步   
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210315180516.png)  
然后选择你要安装的目标文件夹(放在自己定义的目录)，点击下一步   
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210315180649.png)  
选择中文简体  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210315180701.png)   
选择语言，点击安装，等待一会后就可以安装完成了。  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210315180737.png)  
### 3、xshell连接配置
#### 3.1 用户名密码方式登录
- 获取需要连接的虚拟机IP地址，并记住。  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210315180900.png)  
- 创建连接。    
打开xshell ，新建连接。  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210315181023.png)  
编辑连接名称，主机，端口号和用户名密码。  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210315181043.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210315181109.png)  
 连接完成。  
 现在就可以使用xshell操作Linux服务器了。  
#### 3.2公钥方式登录
首先打开xshell5，点击 工具->新建用户密钥生成向导  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210315181228.png)  
选择密钥类型RSA，点击下一步  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210315181248.png)  
等待一会，用户密钥生成后，点击下一步  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210315181312.png)  
记住用户密钥名称，输入密码，点击下一步  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210315181333.png)  
公钥生成后，要保存为文件后，点击完成。  
然后，查看自己的公钥对并保存好。  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210315181442.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210315181501.png)  
选择刚才建立的用户密钥名称，点击导出，导出到刚才保存的公钥文件夹。    
导出的时候需要输入刚才设置的用户密钥的密码。    
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210315181527.png)  
### 4、xftp安装
点击xftp5.exe进行安装，等待一会后，出现以下界面，一直点击下一步  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210408134300.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210408134322.png)  
选择我接受许可证协议的条款，然后点击下一步  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210408134342.png)  
输入用户名，点击下一步  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210408134405.png)  
下一步  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210408134433.png)  
选择语言，点击安装，等待一会后就可以安装完成了。  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210408134453.png)  
### 5、xftp连接配置
#### 5.1 xshell界面快速启动的xftp
是直接通过xshell界面快速启动的xftp，不需要输入用户名和密码。  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210408134620.png)  
#### 5.1 运行xftp程序
运行xftp程序，新建会话。  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210408134641.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210408134701.png)  
确定后，点击连接，即可。    
出现下面的界面，就说明已经连接成功。在权限允许的情况下，可以拖动需要上传的文件到服务器上，   
也可以将下载的文件拖动到本地进行下载。  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210408134829.png)  
