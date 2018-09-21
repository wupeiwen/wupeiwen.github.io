---
title: git的优势
date: 2015/6/29 15:22:35
tags:
- git
categories: git
---
### 本地的版本管理

不需要远程或架设服务器就能做到本地版本管理.

### 不污染子目录的track文件

svn每个子目录都要扔一个.svn.这个实在是.. .(我想很多人都碰到过svn lock folder的情况.实在让人气急败坏.实际上.svn文件就是罪魁祸首.各种clean up无果. delelte后svn up异常.真是..  摔!.去你妹的svn.)<!-- more -->

### 强大的branch

超轻量级的branch建立.(实际只是建立文件指针).推荐根据的git workflow的开发流程.将workspace分成几区.master dev feature hotfix区等.开发层次就出来了.

### merge工具的强力

git根据commit ticket依次再进行一次merge.提高了merge成功率.避免svn merge中的难堪.即使merge失败.也不会生成乱七八糟的版本文件.简单修改后.commit就是.

### 神奇的git gc

由于git本身不保存文件之前的差异文件.只保存每个文件的快照.所以在频繁修改大文件的情况下会造成git目录变得肥大不堪.git早就有了解决方案.git gc后,会在.git目录下生成一个packfile与idx文件.只保存文件差异.满塞!.

### 清爽实用的cli

清爽实用的命令行界面让我彻底抛弃了svn.

### 计算机世界所有的问题都可以通过添加一个间接层来实现