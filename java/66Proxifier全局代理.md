# Proxifier实现全局代理

## 一、前言
* `Proxifier`是专门用来处理不支持代理的程序通过 `SOCKS` 或者 `HTTPS` 代理进行网络访问的一款应用，  
个人觉得这也是 ``Mac/Windows 上最好代理软件之一。  

* `Proxifier` 最大的特点就是强大并且简单，只需要很简单的设置，就可以实现整个系统的联网都通过代理服务接收和发送，  
而且还可以自定义规则，指定哪些应用通过代理，当然它还可以增加你访问网路的 “安全性” ，  
所以我强烈推荐经常使用网络不稳定应用的朋友安装 Proxifier 。

## 二、安装
官网下载地址 [https://www.proxifier.com/download/](https://www.proxifier.com/download/)


## 三、配置  
配置 Proxifier 的前提是你拥有必要的代理服务存在，Proxifier 仅仅是一款软件，而不是代理服务，  
本文不会涉及任何关于代理服务的购买或搭建，且教程也仅仅是为了日常学习能够访问一些网络不稳定的应用，  
而不会访问任何法律法规不允许的其它应用服务。
### 1. 配置代理服务
点击左上角的服务器配置按钮  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210415135006.png)
如下图所示，点击Add，添加代理服务器地址127.0.0.1（代表本地地址）配置代理服务器(可以是https socks5 socks4 等协议)中设置的本地端口，代理服务器端口1080  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210415134333.png)    
配置好后，点击Check，如果显示成功，则表示配置正确：  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210415134540.png)  
### 2. 添加代理规则
添加代理规则  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210415134713.png)  
点击Add添加规则，在Action中可以设置是直连，代理，还是阻止  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210415134736.png)  
至此，已经完成了Proxifier+代理服务器全局代理~首页图可以看到各个软件的连接情况  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210415134830.png)  
### 3. 添加域名解析规则
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210415135006.png)  
* 点击Name Resolution  
* 选择第二个Resolve hostnames through proxy（通过代理服务器解析域名）  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210415135244.png)  
至此，全局代理已经配置完毕。  
## 四、其他设置
* Proxifier的规则设置十分灵活强大，可以默认所有数据流量都通过代理，即以上设置。  
* 同时也可以通过对特殊应用或者端口进行更细粒度的代理设置，比如想要QQ数据包不通过代理，只需要添加规则，应用选择QQ,直连即可  
* 如果想尽量减小代理的流量 也可以修改默认规则为直连，然后添加需要的特殊应用为代理访问  
* 在使用Mobaxterm 和 Proxifier 进行配合的时候，使用ssh-tunnel 的时候，要把motty.exe 添加到使用 direct 进行直连，不使用代理；

