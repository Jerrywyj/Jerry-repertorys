# Python开发环境配置
##1.配置的目的
使用python进行程序开发时，需解决的两大问题为版本兼容问题和包管理问题。
（1）版本兼容问题，当我们在开发环境中同时开发几个不同的项目时有可能A项目使用的是python2.7.5版本，B项目使用的是python3.1.0版本，C项目使用的是python2.7.10版本，这时我们就必须同时安装这三种不同版本的python解释器。
（2）包管理问题，不同的项目需依赖的包不同，如果所有的包都安装在同一个lib目录下势必造成混乱。
##2.解决方案
对于版本兼容以及包管理问题，我们分别使用pyenv和virtualenv这两个工具来解决。
pyenv可以帮助你在一台开发机上建立多个版本的python环境，并提供方便的切换方法。
virtualenv顾名思义，它可以提供一个虚拟的python环境，用户可以建立多个虚拟环境，每个环境里面的python可以是不同的，也可以是相同的，而且环境之间相互独立。以下为示意图。
![pyenv+virtualenv](/Users/jerry/Documents/GitHub/Learn-way/Develop/pyenv+virtualenv.png)
##3.具体配置过程
根据之前的示意图我们便有了十分清晰的配置思路，首先使用pyenv安装上我们项目所需版本的python并切换至该版本，再用virtualenv创建一个虚拟环境，最后在虚拟环境中安装上项目所需的包。
###3.1安装pyenv和virtualenv
pyenv和virtualenv可以分别单独安装，但pyenv的作者写了一个bash脚本，可以让我们一次安装完毕，如需分别单独安装可以到这两个工具的github库中自行寻找安装说明。

```bash
$ curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash
```
执行完以上命令后注意查看执行完成后的说明。
###3.2安装项目所需版本的python
假设需安装2.7.10版本

```bash
$ pyenv install 2.7.10
```
###3.3创建虚拟开发环境
假设需创建一个名为“envDjango”的虚拟开发环境。

```bash
$ pyenv virtualenv 2.7.10 envDjango
```
###3.4切换到所创建的虚拟开发环境

```bash
$ pyenv activate envDjango
```
###3.5切换回系统环境

```bash
$ deactivate enDjango
```
###3.6删除虚拟开发环境
直接删除其目录就可以了，其余的命令可以查看工具的文档。





