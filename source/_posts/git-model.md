---
title: git-flow工作流模型
date: 2018/9/18 11:00:00
tags:
- git
- git flow
categories: git
---

### 分支介绍
#### master
* 主分支 , 产品的功能全部实现后 , 最终在master分支对外发布
* 该分支为只读唯一分支 , 只能从其他分支(release/hotfix)合并 , 不能在此分支修改
* 另外所有在master分支的推送应该打标签做记录,方便追溯
* 例如release合并到master , 或hotfix合并到master<!-- more -->

#### develop
* 主开发分支 , 基于master分支克隆
* 包含所有要发布到下一个release的代码
* 该分支为只读唯一分支 , 只能从其他分支合并
* feature功能分支完成 , 合并到develop
* bugfix补丁分支完成 , 合并到develop
* develop拉取release分支 , 提测
* release/hotfix 分支完成 , 合并到develop并推送

#### feature
* 功能开发分支 , 基于develop分支克隆 , 主要用于新需求新功能的开发
* 功能开发完毕后合到develop分支
* feature分支可同时存在多个 , 用于团队中多个功能同时开发 , 属于临时分支 , 功能完成后删除

#### release
* 测试分支 , 基于feature分支合并到develop之后  , 从develop分支克隆
* 主要用于提交给测试人员进行功能测试 , 测试过程中发现的BUG在本分支进行修复 , 修复完成上线后合并到develop/master分支并推送(完成功能) , 打tag
* 属于临时分支 , 功能上线后删除
* 命名格式x.x.x(主版本号.次版本号.修订号)
* 大版本发布, 修改第一位主版本号，第二位置为0, 第三位置为0
* 小版本发布, 第一位不变, 修改第二位次版本号, 第三位置为0

#### hotfix
* 补丁分支 , 基于master分支克隆 , 主要用于对线上的版本进行BUG修复
* 修复完毕后合并到develop/master分支并推送 , 打tag
* 属于临时分支 , 补丁修复后删除
* 所有hotfix分支的修改会进入到下一个release
* 命名格式x.x.x(主版本号.次版本号.修订号)
* 修复版本发布, 第一位不变, 第二位不变, 修改第三位修订号

#### bugfix
* 补丁分支 , 基于develop分支克隆 , 主要用于对develop分支的代码进行BUG修复
* 修复完毕后合并到develop分支并推送
* 属于临时分支 , 补丁修复后删除
* 所有hotfix分支的修改会进入到下一个release

### 初始化
通过`git flow init`命令把当前目录变成Git FLow可以管理的仓库。
初始化过程会<未完待续...>

### 工作流示意图
![git-model](https://raw.githubusercontent.com/wupeiwen/wupeiwen.github.io/source/image/git-model%402x.png)