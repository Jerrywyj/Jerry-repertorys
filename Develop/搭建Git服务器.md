# 搭建Git服务器
## 1.具体思路
使用安装在VirtualBox上的Ubuntu 14.04 LTS系统作为服务器，网络连接方式为桥接，并使用SSH协议传输数据。
## 2.需准备的安装包
首先当然需要安装git包，然后安装实现SSH协议的openssh包，lighttpd是一个微型服务端，用于以Web的形式呈现各个仓库（repository）的内容。
    
```bash
$ sudo apt-get install git openssh-server openssh-client lighttpd
```
## 3.配置
1.创建一个git用户，用来运行git服务：

```bash
$ sudo adduser git
```
2.搜集所有需要登录的用户的公匙，并将其导入至/home/git/.ssh/authorized_keys文件里，一行一个。
3.先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令：

```bash
$ sudo git init --bare sample.git
```
4.Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。然后，把owner改为git：

```bash
$ sudo chown -R git:git sample.git
```
5.现在这个仓库的地址为git@source_path/URL:sample.git，之后就可以进行相关的推送了。

