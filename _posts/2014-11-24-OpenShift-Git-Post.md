---
published: true
layout: post
title: OpenShift-Git-Post
img: no_pic.gif
tags: 
  - ['OpenShift']
description: 这是在移动网络下用 OpenShift Git 发布到 GitHub 的文章
---
要发布文章到 GitHub Pages 手机上没装 Git 客户端，先用 OpenShift Git 发布。

Logecho是什么?

Logecho 可以生成的全静态网站，它可以运行于任何支援静态网站的服务器上（基本上是一切的）或者类似 github.io 之类提供静态托管的平台中。不需要任何服务器配置，写完了就可以直接发布。

如何开始

需要注意的是 Logecho 的本地运行环境需要 PHP 5.4+ 的支援，如果你运行的是最新的 Mac OSX 系统那么就不用关注这些。另外我们的命令行模式对 Windows 还不支援，这一点请注意。

因为目前还在测试中，因此你得先从 github 上克隆代码
<pre>git clone https://github.com/typecho/logecho.git</pre>
然后编译出一个可履行文件
<pre>cd logecho && php build.php</pre>
你现在可以在当前目录下看到 logecho 这样一个可履行文件，运行它可以看到一些基本用法
<pre>./logecho</pre>

具体请百度或谷歌<http://luoqunju.com>
