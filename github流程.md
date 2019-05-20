### 准备工作

1、去github官网注册账号

2、Linux下安装本地git，sudo apt/apt-get install git，window下去官网安装，选择默认选项

3、创建本地目录，linux下打开终端，建立或选择项目目录，一个目录即为一个仓库，在该目录下执行 git init来初始化目录，使得git可以管理其中文件的变动的记录，在该目录下面有.git/目录，此时一个本地git版本库(又名仓库)就建好了

4、配置git用户名：git  config  --global user.name "你自己github的用户名"     git  config  --global  user.email"你自己github的注册邮箱“

### 一、本地仓库的操作

在版本库中的监控分为，工作区(存放文件的地方即仓库目录下)、暂存区和版本库

#### 1、让git记录修改文件的记录

①、确定在哪条分支上进行操作，当进行文件修改时，修改记录可以串成一条时间记录线，上面的每个节点即为改动，这条线即为分支；当需要多人或者对文件进行多功能同时开发时，改动需要的时间很长，而又不影响别人使用原文件或者其它功能的开发 的话，就需要建立一个新分支，在新的分支上进行工作，等到工作完成，在合并到一条分支上，留下来的分支通常为主分支。**通常建立仓库后，系统和默认建立主分支master**

创建分支命令：git  branch   分支名称

创建分支并切换到新分支；git checkout -b 分支名称

切换分支：git  checkout  分支名称

查看分支:git  branch

②、进行文件的改动，取消文件的改动：git checkout  --filename ===》git add  filename 提交改动至暂存区，取消改动提交命令：git  reset HEAD filename此时退回至文件改动的状态，如需取消文件的改动则执行步骤一中的取消命令= ===》git  commit -m “改动说明”提交改动至版本库，此时取消改动即回退版本：git  reset  --hard  HEAD^回退至上个版本   git  reset --hard HEAD^^  回退上两个版本  git  reset --hard HEAD~100 回退上100个版本  git reset  --hard commitId   回退特定版本号

3、查看改动：git log可查看 当前版本至最初版本的记录   git  log  --pretty=online  每次改动显示为一行   git reflog   查看所有改动记录  git status 查看当前分支下改动的状况   git diff 查看工作区的改动与原文件的区别

5、删除文件：删除工作区的文件rm -rf filename ===》删除版本库中的记录 git  rm filename删除版本库中的文件记录

6、封存现场：当在修改功能进行到一半时，需要进行其它任务的改动，则可以使用分寸现场：git  stash，使用git  status不会提示用要完成的提交，使用git  stash  list查看当前被封存的现场，使用git stash  pop解封现场 

### 二、本地仓库与远程仓库的连接

#### Ⅰ、创建ssh key  

①、终端下输入 ssh-keygen -t rsa -C "你自己的GitHub注册邮箱"

②、会显示创建.ssh/目录，记录它的位置并找到，打开其中id_rsa.pub文件，复制除去邮箱账号前面的所有

3、登陆GitHub官网，右上角下拉菜单选择settings，选择SSH and GPG keys，选择SSH  keys绑定ssh  key，keys下面的框中粘贴之前再id_rsa.pub中复制的内容，如密钥不通过，则再去本地终端中重复1、2步骤

4、本地终端下输入ssh -T git@github.com查看是否与github连接上

#### Ⅱ、建立本地仓库与远程仓库的连接

##### 1、先有本地仓库

一、GitHub页面点击左上角头像，选择New repository选项

二、填入仓库名称时最好与本地仓库名称一致，选择piblic，initializae。。。。选项可选可不选

三、点击create repository

四、关联远程仓库  git remote add origin(origin不是固定的，一般约定写成，远程仓库的标志) 远程仓库地址  在仓库界面点击clone or download下面会出现远程仓库地址，一般格式为：git@github.com:账户名/仓库名.git  ;需要删除关联时命令 git remote remove origin(远程仓库的标志)

五、推送本地内容到远程仓库：git push origin 分支名  

--第一次推送时 git push -u origin master

六、拉去远程仓库内容到本地：git pull  origin分支名，  git pull 拉去远程仓库所有内容

七、git remote查看远程仓库的信息  git  remote -v查看远程仓库的版本信息  git  config  -l 查看仓库连接信息

##### 2、先有远程仓库

一、远程仓库建立时与1步骤类似，不过initializae。。。。。选项必须勾选

二、git clone  git@github.com:账户名/仓库名.git

### 三、分支管理

#### 1、单人下分支管理命令

创建分支命令：git  branch   分支名称

创建分支并切换到新分支；git checkout -b 分支名称

切换分支：git  checkout  分支名称

查看分支:git  branch

合并分支：git  merge 子分支名称  /git  merge  --no-ff  -m”自定义提交内容“ 子分支 不会在分支合并图中删除子分支的记录

查看分支合并图：git log --graph  /git log  --graph  --pretty=online一行显示分支合并图

删除本地分支：git branch -d 子分支名称   

删除远程分支：一般先删除本地分支再删除远程分支，git push origin  子分支名称  git push  origin  --delete  分支名称

分支冲突：当在两个分支 上进行文件的同一位置的的改动时，合并分支时便会发生冲突。此时只能手动解决冲突，要么取消分支合并git merge --abort；在主分支上手动解决冲突后，进行add和commit操作，在子分支上解决冲突后，进行add和conmit之后，进入主分支上进行merge操作

#### 2、多人协作模式

##### 一、分支策略

master分支应该时非常稳定的，仅仅用来发布新版本，平时不能在上面干活；干活的都在dev分支上，也就是说dev分支是不稳定的，到某个时候在合并到master上，每个人都在dev上干活，每个人都有自己的分支，时不时地往dev分支上合并就行了

![](C:\Users\郭金鑫\AppData\Roaming\Typora\typora-user-images\1551495828835.png)

##### 二、多人协作

1、推送分支：git  push  origin  master 、git  push  origin  dev

2、抓取分支：从远程clone时，默认只能看到master，想在dev分支上开发，就必须创建origin的dev分支到本地，git  checkou -b  dev  origin/dev

3、工作模式：

一、可以试图使用 git push origin  branch-name推送自己的修改

二、如果推送失败，则因为远程分支比你的本地更新，则需要先使用git  pull试图合并

三、如果有冲突，则解决冲突，并在本地提交

四、如果有冲突或者解决冲突后，再用git push origin branch-name

五、如果git  pull提示 no tracking information，则说明本地分支和远程分支的链接没有创建，用命令git  branch --set-upstream-to=origin/branch-name  branch-name

### 四、标签管理

标签相当于版本的一个快照

1、常用命令

给当前分支版本打标签：git  tag  标签名

查看所有标签：git  tag

指定commit id打标签：git  tag  标签名  commitId

指定标签信息：git  tag  -a  标签名  -m  “标签信息”

切换到指定标签：git  checkout  标签名

查看说明文字：git  show  标签名称

删除标签：git  tag  -d  标签名称

推送标签名到远程：git  push origin 标签名

一次性推送全部尚未推送到远程 的本地标签：git  push  origin  --tags

删除已经推送到远程的标签：先从本地删除：git  tag  -d  标签名 、再从远程删除：git  push origin  ：refs/tags/标签名  

### 五、搭建git内部服务器  

1、流程

1） 安装openssh-serverssh服务

2） 创建git用户，确定home路径

3） 在git的home创建git项目存放的repo

4） 在repo创建git的仓库

​	git init --bare swiper.git

5） 在开发环境下

​	创建本地git仓库

​	提交代码

​	配置远程仓库

​	git remote add origin git@10.12.152.83:repo/swiper.git

​	上传本地code

​	git push origin master

2、免密操作

2. 免密设置
    1) 本地 通过ssh-keygen生成公钥与私钥
    ​		ssh-keygen  命令执行时不需要输入任何信息，一路回车
    ​		生成成功之后， 在~/.ssh目录下
    ​		id_rsa 是私钥 
    ​		id_rsa.pub 是公钥
    ​	
      2)  通过scp 将id_rsa.pub上传到git服务器上
    ​		scp ~/.ssh/id_rsa.pub  git@10.12.152.3:~/
       3) 登录到git服务器，将上传的id_rsa.pub写入到~/.ssh/authorized_keys文件中
    ​		cat id_rsa.pub > authorized_keys

    		注意： 第一次使用 > 管道时，会生成新的文件
    					第二次使用 >> 表示，追加新的公钥到authorized_keys的尾部。



linux 添加用户

sudo useradd -d /home/git -m git

给添加的用户设置密码

sudo passwd git  

在仓库里面 进入  .ssh文件 -

chown git:git -R swiper.git 授权