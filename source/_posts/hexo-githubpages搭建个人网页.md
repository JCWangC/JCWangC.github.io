---
title: hexo+githubpages搭建个人网页
date: 2020-03-1 18:15:28
tags: 
    - hexo 
    - 网页
categories: 
- 网页


---

Hexo是一款基于Node.js的静态博客框架，依赖少，易于安装使用，可以方便的生成静态网页托管在GitHub和Coding上，是搭建博客的首选框架。大家可以进入hexo官网进行详细查看，因为Hexo的创建者是台湾人，对中文的支持很友好，可以选择中文进行查看。

# 安装Git
Git是目前世界上最先进的分布式版本控制系统，可以有效、高速的处理从很小到非常大的项目版本管理。

windows：到git官网上下载,Download git,下载后会有一个Git Bash的命令行工具，以后就用这个工具来使用git。

linux：对linux来说实在是太简单了，因为最早的git就是在linux上编写的，只需要一行代码
```  
sudo apt-get install git
```
安装好后，用`git --version` 来查看一下版本

# 安装nodejs
Hexo是基于nodeJS编写的，所以需要安装一下nodeJs和里面的npm工具。

windows：nodejs选择LTS版本就行了。

linux：
```
sudo apt-get install nodejs
sudo apt-get install npm
```
安装完后，打开命令行
```
node -v
npm -v
```
检查一下有没有安装成功

windows在git安装完后，就可以直接使用git bash来敲命令行了，不用自带的cmd，cmd有点难用。

# 安装hexo
前面git和nodejs安装好后，就可以安装hexo了，自己可以先创建一个文件夹blog，然后cd到这个文件夹下（或者在这个文件夹下直接右键git bash打开）。

输入命令
```
npm install -g hexo-cli
```
依旧用hexo -v查看一下版本

至此就全部安装完了。

接下来初始化一下hexo
```
hexo init myblog//文件夹名
```

```
cd myblog //进入这个myblog文件夹
npm install
```
新建完成后，指定文件夹目录下有：
```
node_modules: 依赖包
public：存放生成的页面
scaffolds：生成文章的一些模板
source：用来存放文章
themes：主题
```
```
** _config.yml: 博客的配置文件**
hexo g
hexo server
```

打开hexo的服务，在浏览器输入`localhost:4000`就可以看到自己生成的博客了。


使用ctrl+c可以把服务关掉。

# GitHub创建个人仓库
登录GitHub账户，在GitHub.com中看到一个New repository，新建仓库


创建一个和自己用户名相同的仓库，后面加.github.io，只有这样，将来要部署到GitHub page的时候，才会被识别，也就是xxxx.github.io，其中xxx就是自己注册GitHub的用户名。我这里是已经建过了。



点击create repository。

# 生成SSH添加到GitHub
回到自己的git bash中，

```
git config --global user.name "myname"
git config --global user.email "myemail"
```

这里的myname输入自己的GitHub用户名，myemail输入自己GitHub的邮箱。这样GitHub才能知道自己是不是对应它的账户。

可以用以下两条，检查一下自己有没有输对
```
git config user.name
git config user.email
```

然后创建SSH,一路回车
```
ssh-keygen -t rsa -C "myemail"
```
这个时候它会告诉自己已经生成了.ssh的文件夹。在自己的电脑中找到这个文件夹。



`ssh`，简单来讲，就是一个秘钥，其中，`id_rsa`是自己这台电脑的私人秘钥，不能给别人看的，`id_rsa.pub`是公共秘钥，可以随便给别人看。把这个公钥放在GitHub上，这样当自己链接GitHub自己的账户时，它就会根据公钥匹配自己的私钥，当能够相互匹配时，才能够顺利的通过git上传自己的文件到GitHub上。

而后在GitHub的`setting`中，找到`SSH keys`的设置选项，点击`New SSH key`
把自己的`id_rsa.pub`里面的信息复制进去。



在gitbash中，查看是否成功
```
ssh -T git@github.com
```
# 将hexo部署到GitHub
这一步，我们就可以将hexo和GitHub关联起来，也就是将hexo生成的文章部署到GitHub上，打开站点配置文件 _config.yml，翻到最后，修改为
mygithubName就是自己的GitHub账户

```
deploy:
type: git
repo: https://github.com/mygithubName/mygithubName.github.io.git
branch: master
```

这个时候需要先安装deploy-git ，也就是部署的命令,这样自己才能用命令部署到GitHub。
```
npm install hexo-deployer-git --save
```
然后

```
hexo clean
hexo generate
hexo deploy
```
其中 `hexo clean`清除了自己之前生成的东西，也可以不加。
`hexo generate` 顾名思义，生成静态文章，可以用` hexo g`缩写
`hexo deploy` 部署文章，可以用`hexo d`缩写

注意`deploy`时可能要自己输入`username`和`password`。

得到下图就说明部署成功了，过一会儿就可以在http://myname.github.io 这个网站看到自己的博客了！！


# 设置个人域名
现在自己的个人网站的地址是 githubname.github.io，如果觉得这个网址逼格不太够，这就需要自己设置个人域名了。但是需要花钱。

登录GitHub，进入之前创建的仓库，点击`settings`，设置`Custom domain`，输入自己的域名`jcwang.me`


然后在自己的博客文件source中创建一个名为CNAME文件，不要后缀。写上自己的域名。



最后，在`gitbash`中，输入
```
hexo clean
hexo g
hexo d
```
过不了多久，再打开自己的浏览器，输入自己自己的域名，就可以看到搭建的网站啦！

接下来自己就可以正式开始写文章了。
```
hexo new newpapername
```
然后在`source/_post`中打开markdown文件，就可以开始编辑了。当自己写完的时候，再
```
hexo clean
hexo g
hexo d
```
就可以看到更新了。



```
hexo n "Hello, World!"
```

