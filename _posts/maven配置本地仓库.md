---
layout:     post
title:      "maven配置本地仓库"
subtitle:   "maven配置本地仓库"
date:       2020-11-24 11:58
author:     "ZXZ"
header-style:   "text"
header-img: "img/post-bg-rwd.jpg"
tags:
    - JAVA
    - maven
---
# maven配置本地仓库
#####它是什么？
Maven是一个项目管理工具，Maven项目对象模型(POM)，可以通过一小段描述信息来管理项目的构建，报告和文档的项目管理工具软件。
#####为什么用它？
最近在学习Mybatis框架，当老师配置pom.xml的时候，可以很快找到，而我的项目却找不到jar,摸索了一会儿，使用IDEA在网络库中下载了，但是很慢，因为maven的网络库是国外网站，也可以配置中央库比如阿里，网易等会快一点，今天这里使用的是本地库。
#####怎么用它？
1.去官网下载它,我是解压到一个新建文件夹下，然后在同级别下创建“LocalWarehouse”当作仓库

2.conf——>编辑settng.xml文件

如下图所示，在这里配置本地仓库路径

![img](/img/maven1.jpg)

3.配置环境变量

变量名：M2_HOME

变量值：D:\My\maven\apache-maven-3.6.3

path：%M2_HOME%\bin

检测是否成功：在CMD中输入mvn -v,如出现下列信息，表示配置成功。

4.IDEA关联maven本地仓库

在设置界面搜索"maven" 设置如下图

![img](/img/maven2.jpg)

