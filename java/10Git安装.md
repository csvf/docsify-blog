# Git 安装配置教程
## 一、前言
Git是一款代码版本控制工具。
## 二、安装
### 1、下载
**下载地址** 官网  [https://git-scm.com/downloads](https://git-scm.com/downloads)   
根据操作系统下载对应的最新版本 download标签有详细展示，我是windows10 64位 下载下面红框的版本  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20210304152825.png)  
下载完后的文件  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20201215114936136.png)  
### 2、安装
1. 鼠标双击安装文件, 如果有Windows拦截警告，允许即可   
2. 出现安装向导界面,点击下一步(Next)即可:  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304154342.png)  
3. 接着出现授权信息界面， Next即可  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304160648.png)  
4. 选择安装路径  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304160957.png)  
5. 选择文件关联  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304161035.png)  
6. 接着出现开始菜单文件夹,默认,下一步即可:  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304161106.png)  
7. 然后是是否配置Path的配置,选择中间一个,可以通过 Windows命令行(CMD)调用 git 命令。 然后点击下一步.  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304161200.png)  
8. 选择回车换行的格式。默认即可.(检出时转换为Windows风格,提交时转换为Linux风格.) 回车换行风格(CRLF-LF)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304161308.png)  
9. 然后是安装进度界面  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304161349.png)  
10. 安装完成. 去掉那个查看版本说明的复选框,点击完成(Finish)按钮即可.  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304161413.png)  
11. 可以在cmd里面测试是否设置了Path,是否安装成功. 在CMD中输入 git 或者 git --version 试试:  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210304161447.png)
12. 如果按照前面的步骤安装下来,那么 git 程序所在的路径已经添加到系统 PATH 中(path就相当于系统自动查找路径列表),  
所以可以直接在任意路径的 cmd 下执行 git 命令. 如果没有添加,则需要 cd 切换到Git所在的 bin 目录下,才能执行 git 命令.
## Git常用命令
1. 创建并且切换至dev分支:  
   `$ git checkout -b dev`  
   ```shell script
   Switched to a new branch 'dev' 
   ```
   git checkout命令加上-b参数表示创建并切换，相当于以下两条命令:   
   `$ git branch dev`  
   `$ git checkout dev`  
   ```shell script
   Switched to branch 'dev'
    ```  
2. 查看当前分支:
  `$ git branch`  
   ```shell script
   *dev  
    master 
   ``` 
   git branch命令会列出所有分支，当前分支前面会标一个*号。  
   然后，我们就可以在dev分支上正常提交，比如对readme.txt做个修改，加上一行：  
   `Creating a new branch is quick.`  
3. 提交当本地仓库: 
   `git add readme.txt`  
   ` git commit -m "branch test" `  
    [dev b17d20e] branch test 1 file changed, 1 insertion(+)   
4. 推送到远程仓库:  
   `git push --set-upstream origin dev`  
   
5. 切换分支:
   `git checkout master`  
   ``` Switched to branch 'master'  ```   
6. 合并dev成果到master分支上:
   `git merge dev`   
   ```
    Updating d46f35e..b17d20e
    Fast-forward 
    readme.txt | 1 +
    1 file changed, 1 insertion(+)
   ```
   `git merge`命令用于合并指定分支到当前分支。  
    合并后，再查看readme.txt的内容，就可以看到，和dev分支的最新提交是完全一样的。  
    注意到上面的`Fast-forward`信息，`Git`告诉我们，这次合并是“快进模式”，也就是直接把master指向dev的当前提交，  
    所以合并速度非常快。当然，也不是每次合并都能`Fast-forward`，我们后面会讲其他方式的合并。  
    合并完成后，就可以放心地删除dev分支了。  
7.  删除dev分支:   
    `git branch -d dev`  
    ```shell script
    Deleted branch dev(was b17d20e)
    ```
    删除后，查看`branch`,就只剩下 `master` 分支了:   
    `git branch `   
    ```shell script
    * master
    ```
8. 因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，   
   这和直接在master分支上工作效果是一样的，但过程更安全。  
9. 小结:   
   Git鼓励大量使用分支：   
   * 查看分支：`git branch`  
   * 创建分支：`git branch <name>`
   * 切换分支：`git checkout <name>`或者`git switch <name>`
   * 创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`
   * 合并某分支到当前分支：`git merge <name>`
   * 删除分支：`git branch -d <name>`