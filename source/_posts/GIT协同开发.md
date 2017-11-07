---
title: GIT协同开发
date: 2017-08-20 11:46:07
tags: github
description: 用了一段时间github,一直想用时间来对git的使用来做一段笔记，前段时间比较忙，现在沉下心来学习也是极好的。
---


##### 用了一段时间github,一直想用时间来对git的使用来做一段笔记，前段时间比较忙，现在沉下心来学习也是极好的。
很多项目开发会采用git这一优秀的分布式版本管理工具来进行项目版本管理。因为git的使用非常灵活，所以在实际操作中会有许多不同的工作流程。不同团队对于不同项目会有不同的协作方式。掌握git版本管理开发，对以后的学习和开发都有很多好处。

##### 首先基本名词要懂：

> 仓库（Repository）、分支（branch）、工作流（workflow）

##### 仓库（Repository）

在项目的开始到结束，我们会有两种仓库，一种是源仓库（origin），一种是开发者仓库。

###### 源仓库（origin）的有两个作用：

- 汇总参与该项目的各个开发者的代码
- 存放趋于稳定和可发布的代码

> 源仓库应该是受保护的，开发者不应该直接对其进行开发工作。只有项目管理者（通常是项目发起人）能对其进行较高权限的操作。

###### 开发者仓库：
> 任何开发者都不会对源仓库进行直接的操作，源仓库建立以后，每个开发者需要做的事情就是把源仓库的“复制”一份，作为自己日常开发的仓库。这个复制，也就是github上面的fork。

> 每个开发者所fork的仓库是完全独立的，互不干扰，甚至与源仓库都无关。  每个开发者仓库相当于一个源仓库实体的影像，  开发者在这个影像中进行编码， 提交到自己的仓库中，这样就可以轻易地实现团队成员之间的并行开发工作。  而开发工作完成以后,   开发者可以向源仓库发送pull request，请求管理员把自己的代码合并到源仓库中，这样就实现了分布式开发工作，和最后的集中式的管理。

 

##### 分支（branch） 

在git中，分支操作则是每个开发人员日常工作流。利用git的分支，可以非常方便地进行开发和测试。

我们为git定下一种分支模型，在这种模型中，分支有两类，五种：

######  永久性分支

- master branch：主分支

- develop branch：开发分支

###### 临时性分支

- feature branch：功能分支

- release branch：预发布分支

- hotfix branch：bug修复分支

> master：主分支从项目一开始便存在，它用于存放经过测试，已经完全稳定代码；在项目开发以后的任何时刻当中，master存放的代码应该是可作为产品供用户使用的代码。每一次master更新的时候都应该用git打上tag，说明你的产品有新版本发布了。

> develop：开发分支，一开始从master分支中分离出来，用于开发者存放基本稳定代码。开发者把功能做好以后，是存放到自己的develop中，当测试完以后，可以向管理者发起一个pull request，请求把自己仓库的develop分支合并到源仓库的develop中。

> 归纳：所有开发者开发好的功能会在源仓库的develop分支中进行汇总，当develop中的代码经过不断的测试，已经逐渐趋于稳定了，接近产品目标了。这时候，我们就可以把develop分支合并到master分支中，发布一个新版本。


> feature：功能性分支，是用于开发项目的功能的分支，是开发者主要战斗阵地。开发者在本地仓库从develop分支分出功能分支，在该分支上进行功能的开发，开发完成以后再合并到develop分支上，这时候功能性分支已经完成任务，可以删除。功能性分支的命名一般为feature-*，*为需要开发的功能的名称。

> release：预发布分支，当产品即将发布的时候，要进行最后的调整和测试，这时候就可以分出一个预发布分支，进行最后的bug fix。测试完全以后，发布新版本，就可以把预发布分支删除。预发布分支一般命名为release-*。

> hotfix：修复bug分支，当产品已经发布了，突然出现了重大的bug。这时候就要新建一个hotfix分支，继续紧急的bug修复工作，当bug修复完以后，把该分支合并到master和develop以后，就可以把该分支删除。修复bug分支命名一般为hotfix-*。

 

> 示范：举一个例子，A正在做一个团队项目，已经把源仓库fork了，并且clone到了本地。现在要开发网站的某个功能。A在本地仓库中可以这样做：

##### 切换到develop分支 ：``` git checkout develop```

##### 分出一个功能性分支： ```git checkout -b feature-discuss```

在功能性分支上进行开发工作，多次commit，测试以后...

##### 把做好的功能合并到develop中：

```
git checkout develop    # 回到develop分支    

git merge--no-ff feature-discuss# 把做好的功能合并到develop中    

git branch -d feature-discuss    # 删除功能性分支    

git push origin develop    # 把develop提交到自己的远程仓库中
```

 

##### 工作流（workflow）

- 源仓库的构建，创建一个项目，初始化了两个永久性分支master和develop.

- 开发者fork源仓库

- 把自己开发者仓库clone到本地，命令：git clone

- 构建功能分支进行开发，完成后合并到自己的develop分支。

##### 进入仓库中，按照前面说所的构建功能分支的步骤，构建功能分支进行开发、合并，假设我现在要开发一个“讨论”功能：
```
git checkout develop    # 切换到`develop`分支   

 git checkout -b feature-discuss    # 分出一个功能性分支    

touch discuss.js    # 假装discuss.js就是我们要开发的功能    

git add .    

git commit -m 'finish discuss feature'# 提交更改    

git checkout develop    # 回到develop分支    

git merge--no-ff feature-discuss# 把做好的功能合并到develop中    

git branch -d feature-discuss    # 删除功能性分支    

git push origin develop    # 把develop提交到自己的远程仓库中
```

 - 向管理员提交pull request。经过测试以后，觉得没问题，就可以请求管理员把自己仓库的develop分支合并到源仓库的develop分支中，这就是传说中的pull request。

##### 管理员测试、合并

对代码进行review，github提供非常强大的代码review功能

在本地测试新建一个测试分支，测试pull request的代码
```
git checkout develop    # 进入本地的develop分支    

git checkout -b livoras-develop    
```

##### 从develop分支中分出一个叫livoras-develop的测试分支测试pull request代码    

git pull https://github.com/livoras/git-demo.git develop    

##### 把pull request的代码pull到测试分支中，进行测试

##### 判断是否同意合并到源仓库的develop中，如果经过测试没问题，可以把我的代码合并到源仓库的develop中：
```
git checkout develop    

git merge--no-ff livoras-develop    

git push origin develop
```