---
layout:     post                     
title:      github账号切换以及https和SSH上传方式对比 （转载）           
subtitle:   github账号切换以及https和SSH上传方式对比
date:       2017-02-17              
author:     BY                        
header-img: img/post-bg-2015.jpg     
catalog: true                     
tags:                     
    - github账号切换
---

## Hey
>github账号切换以及https和SSH上传方式对比 

在这里我就不介绍什么是git！什么是github了！给大家两篇参考文章，先学会git再学github

git简明教程 (http://rogerdudler.github.io/git-guide/index.zh.html)

github简明教程 (http://www.runoob.com/w3cnote/git-guide.html)

废话少说，这篇文章主要讲什么呢？

这篇文章就是探讨一下github的https和SSH方式上传注意点，以及你想换一个github账号可能会遇到的问题。

说明一下，以下测试均在Mac电脑上

首先说一下账号切换

首先我现在有一个github账号叫A,但是我现在不想用这个账号了，于是我在github上面重新申请了一个账号叫B，我在B账号下新建了一个仓库，然后使用命令行上传我的本地代码出现以下错误

remote: Permission to B/test.git denied to A
fatal: unable to access 'https://github.com/B/test/': The requested eror: 403
这个意思是不允许账号A访问账号B下面的test仓库。

我使用git config命令更改了我的用户名和仓库 ，一样的错误。

但是这时候我使用Github Desktop把上面登录的账号换成B,首先把test仓库克隆下来，然后使用Github Desktop上传代码 成功

这说明了什么？？？？

因为Github Desktop有我们登录的用户名和密码，所以上传的时候Github Desktop会自动给出B的用户名和密码，
而当我们使用命令行的时候，Mac会一直使用A这个用户名和密码上传而不是B，这是为什么呢？
原因是Mac电脑上的钥匙串，每次我们设置好用户名和密码都会自动存储到Mac的钥匙串上面，每次使用命令行上传代码的时候，Mac会自动填充A账号和密码
这时候我只需要打开钥匙串，然后搜索git，把有关Github的账号密码都删除掉。

然后继续使用命令行上传代码这时候会需要让我们设置上传的用户名和密码，设置为B这个账户就行了。

还有一种上传成功的方式就是把A这个账号添加到B账号下的test仓库下的Collaborators这样不用更改Mac上的用户名和密码也可以上传成功。

大家可能疑问我怎么没有配置SSH就能上传代码？
因为上面这些方法都是使用的https方式上传的。

账号上传方式探讨

Github有两种上传代码的方式

https
SSH
大家也可以看一下这两种方式的URL是不同的。

SSH方式（git@github.com:xxx/test.git）
https方式 (https://github.com/xxx/test.git)

https方式提交代码的几种方式：

首先使用git clone后面跟https方式 把仓库克隆下来，然后提交代码(clone后面只能跟https的URL)
使用Github Desktop这个软件提交
使用SSH方式提交代码：

git init
git remote add origin git@github.com:TrackyTian/testSSH.git //连接远程仓库
git pull --rebase origin master //把远程仓库拉下来
git add . //将仓库所有文件都添加到版本控制库中
git commit -m "赋值Person" //提交
git push origin master //将代码添加到master分支
我总结以下几个情况：

B账户需要往B账户下的仓库提交代码

可以使用https方式，不需要配置SSH
如果使用SSH方式提交，但是没有配置在B账户下配置SSH的话，会出现类似下面的错误，这时候添加一下本机的SSH

解释一下SSH：我们只需要把SSH看成一台电脑的通行证，每个电脑都是固定的，把SSH配置到哪个账户，就表示可以使用这台电脑给这个账户下的仓库上传代码！

如果我想要给别人的仓库提交代码

我需要把我本机的SSH配置到别人账户下，然后采用SSH方式提交代码
把我的账号添加到那个仓库的Collaborators，直接使用https方式提交
总结

使用https方式提交的不需要添加SSH，但是使用SSH方式提交的必须要添加本机的SSH
A账户想要给A账户下的仓库提交代码，直接使用https方式就行
A账户想要给B账户下的仓库提交代码，1.添加Collaborators使用https方式 或者2.添加SSH，使用SSH方式提交。
请大家多关注我的博客 (http://www.codertian.com)
