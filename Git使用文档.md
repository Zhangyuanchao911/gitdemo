[TOC]



### Git简介

Git是目前世界上最先进的分布式版本控制系统（没有之一）。

### Git常用的操作命令

#### 一   将一个文件夹创建为本地git仓库

打开git bush  定位到该文件夹下  

命令： `git init`

#### 二   将文件添加到版本库

##### 1.将文件添加到仓库

命令：`git add filename`

##### 2.将文件提交到仓库

命令：`git commit -m "message"`



​        简单解释一下`git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。 

为什么Git添加文件需要`add`，`commit`一共两步呢？因为`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件

#### git撤销修改

`git checkout -- file`

命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：

一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态

一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

#### 三   本地库和远程库建立连接

##### 1.第一次建立连接要手动添加ssh加密文件

`ssh-keygen -t rsa -C "911454016@qq.com"` 

在自己的GitHub账号设置中添加ssh密钥  将系统盘下.ssh文件夹下的id_rsa.pub中的代码复制到里面

GitHub上创建仓库  创建完成之后复制该仓库的ssh地址

<img src="https://github.com/Zhangyuanchao911/gitdemo/blob/master/img/20200208211439106.png" alt="image-20201011140059129" style="zoom: 80%;" />

##### 2.在本地使用命令

`git remote add origin git@github.com:Zhangyuanchao911/gitdemo.git`

##### 3.上传本地仓库

如果；远程仓库是空的并且是第一次上传要使用 

`git push -u origin master`

如果不是第一次

`git push origin master`

#### 四   从GitHub克隆仓库

##### 克隆仓库

命令：`git clone git@github.com:Zhangyuanchao911/gitdemo.git`

GitHub给出的地址不止一个，还可以用`https://github.com/Zhangyuanchao911/gitdemo.git`这样的地址。实际上，Git支持多种协议，默认的`git://`使用ssh，但也可以使用`https`等其他协议。

使用`https`除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用`ssh`协议而只能用`https`。

##### 小结

要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

Git支持多种协议，包括`https`，但`ssh`协议速度最快。
