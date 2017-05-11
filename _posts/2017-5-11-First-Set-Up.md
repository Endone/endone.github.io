---
layout: post
title: 使用Jekyll在github.io上搭建本博客
---

之前一直想自己搭建一个博客写写游戏开发相关的东西，今天下午码完代码就折腾了一下。
看了一下，比较好的静态博客有Hexo、Pelican、Jekyll等，大致看了一下，最终选择了Jekyll，功能够用上手也快，运行平台直接选用了github.io的免费空间，另外域名是之前早就买好的了。

搭建的过程比较简单快速：
* 1. Fork Jekyll库到自己的master分支；
* 2. 重命名Fork的库为 `githubusername.github.io`，注意：大小写区分；
* 3. 搞定！然后就可以访问 `githubusername.github.io`来访问博客了，URL不区分大小写；

自定义域名映射：
* 1. 买域名，早就买了；
* 2. 配置Fork的Jekyll库，Repository->Setting->GitHub Pages->Custom domain填写完整域名，如 www.xxx.com；
* 3. 进入域名管理界面，添加解析域名，一条：`CNAME	www	默认 githubusername.github.io`；另一条：`CNAME * 默认 githubusername.github.io`；区分大小写;
* 4. 搞定！等待一会，就可以通过xxx.com和www.xxx.com来访问了！

接下来，希望能在这个博客上写一些游戏开发上遇到或学到的问题和知识！坚持！