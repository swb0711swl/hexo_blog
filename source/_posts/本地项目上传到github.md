---
title: 本地项目上传到github
date: 2017-08-25 13:28:28
tags: github
categories: github
---

事情过程大概是这样发展的，我自己先写了个Demo，放在我本地，然后我为这个demo配了一篇博客，事后我想把本地这个项目放在github上托管，最后把这个过程记录下来，方便以后使用。

----------

<!-- more -->
**步骤1、**首先在自己的 github 上新建一个仓库 repository，自己命名好即可
**步骤2、**打开终端，进入本地项目目录
**步骤3、**执行命令：`git init`。(该命令是在本地项目目录下初始化一个repository，成功后，会在本地项目目录下生成一个.git的隐藏文件)
**步骤4、**执行命令：`git add .`。（命令后面有个点“.”，把本地项目目录下的所有文件加到本地暂存区中）
**步骤5、**执行命令：`git commit -m "随便写点提交注释"`。(该命令会把本地暂存区中的文件提交到本地历史区，注意只有在本地历史区中的内容才能提交到github，执行该命令后，所有的文件都只是在本地，跟github还没有关系)
**步骤6、**执行命令：`git remote add origin https://github.com/自己的github username/项目名.git`。（该命令是把本地历史区中的文件添加到github服务器的暂存区中，这一步是本地和远程服务器建立联系）
**步骤7、**执行命令：`git pull origin master`。（每次提交之前要进行pull，这个命令可能不成功，提示fatal:refusing to merge unrelated histories,需要执行下面的命令：`git pull origin master --allow-unrelated-histories`）这一步之后可能会出现编辑界面，输入`:wq`回车退出编辑即可，到此为止本地项目目录中多了一个README.md文件，这个文件就是从github上pull下来的
**步骤8、**执行命令：`git push -u origin master`。（这一步是真正向github提交，完事儿后本地和github上就同步了）

-----------

*到此结束，一把辛酸泪，好记性不如烂笔头。*


