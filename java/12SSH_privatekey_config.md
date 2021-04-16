# SSH密钥的配置方法
## 一、前言
注意：配置前提需要安装好[Git教程](java/10Git安装)以及[Tortoisegit教程](java/11TortoiseGit安装)
## 二、配置
1. 运行Git Bash程序  
在安装了Git的目标客户机上，如图运行Git软件包中的Git Bash程序  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304180729.png)  
2. 命令窗口中输入 ssh-keygen -t rsa -C “XXX@qq.com” 邮箱为注册Gitlab时的邮箱  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304180856.png)  
3. 选择保存公私钥路径，一般默认回车即可   
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304180925.png)  
4. 密钥已经存在是否覆盖，输入y  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304180944.png)  
5. 请输入密码（PS：一般为了以后操作方便，直接回车表示无密码）  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304181058.png)  
再次确认密码  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304181128.png)  
公私钥已生成  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304181149.png)  
6. 输入 此命令 clip < ~/.ssh/id_rsa.pub 复制出公钥  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304181248.png)   
7. 将公钥粘贴到Gitlab->profile settings->SSH Keys->Add SSh Key  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304181317.png)  
8. 在安装了TortoiseGit的目标客户机上，运行TortoiseGit软件包中的PuTTYgen程序,点击Load  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304181400.png)  
9. 把之前生成的私钥打开  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304181421.png)  
10. 点击save private key  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304181446.png)  
文件名命名为private后缀为.ppk  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304181521.png)  
11. 配置SSH登录密钥，运行TortoiseGit软件包中的Pageant程序，右键选择Windows桌面右下角（通知区域）的图标，  
出现如下菜单并双击打开：  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304181604.png)  
12. 选择Add Key菜单栏，出现Select Private Key File文件选择框，选择之前生成的对应的私钥文件（.ppk）文件，  
配置完成（安全起见，此时可删除该私钥文件了）  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304181647.png)   
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304181702.png)  
成功加入到Pageant中，执行Pageant  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304181732.png)  
13. 右键->TortoiseGit->Settings  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304181759.png)  
设置Network->SSH client (目录为Git安装目录中的ssh.exe路径)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304181827.png)  
14. 在要克隆代码所在本地文件夹中,右键Git clone ；URL:为Gitlab中项目SSH地址下载路径，点击ok，
即可下载成功。  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304181913.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304181928.png)   
## 三、git常用命令
1. 修改提交git时，提交记录的用户名  
* 找到project项目根路径，右键`git bash here`;  
* `git config`  
* `git config user.name  "你的名字"` ;   //这时候会显示已更改的名字  
*  也可以在 `idea` 控制台的`Terminal` 选项卡命令窗下执行以上步骤。