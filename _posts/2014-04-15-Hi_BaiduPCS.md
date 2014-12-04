---
published: true
layout: post
title: Hi_BaiduPCS
img: pcs.jpg
tags: 
  - ['API']
description: 百度PCS简易外链列表,请注意这不是直连百度网盘，这会等同消耗你空间从百度网盘下载相应文件的流量！额，建议使用七牛镜像……
---
<audio preload="load" controls="controls"><source src="{{ site.media_url }}风雨无阻.mp3" type="audio/mpeg" /><embed src="http://sablogbae.cdn.duapp.com/mp3/dewplayer.swf?mp3={{ site.file_url }}风雨无阻.mp3&autostart=0&autoreplay=1&volume=90" type="application/x-shockwave-flash" width="185" height="20" quality="high" /></audio>

这个本代码只是我为了方便本站外链MP3和图片，基于Baidu PCS PHP SDK，因此仅适用于小文件分享。

问：我该怎样部署代码？

答：下载代码包[https://code.jd.com/kookerjp/Hi_BaiduPCS/archive/master.zip](https://code.jd.com/kookerjp/Hi_BaiduPCS/archive/master.zip)<br />步骤：[http://qiufupo.jd-app.com/Authorization/](http://qiufupo.jd-app.com/Authorization/)

问：必需是百度开发者才能使用本代码吗？

答：不是，只要你有百度账号及自己的PHP空间即可

问：如何上传文件

答：使用百度网盘客户端或网站，上传到我的应用数据->kooker文件夹中才能显示分享

问：需要数据库支持吗？

答：目前不需要

问：非开发者能否重命名百度网盘->我的应用数据->kooker文件夹

答：不能，这样就无获取你的应用数据分享

问：我是开发者如何使用本代码

答：到开发平台->你的应用名称->其他API->开启PCS API权限【不清楚现在需不需要申请】,用自己的 API Key + Secret Key 通过Authorization Code获取10年的有效期Refresh Token<br />
参考：http://developer.baidu.com/wiki/index.php?title=docs/oauth/authorization

问：我不小心向别人暴露了Access Token值或Refresh Token值，会如何

答：别人将可以随意修改你的应用数据，包括清空回收站！

* 应对措施
    * 先取消授权，再重新授权获取Access Token值或Refresh Token值。
    * 取消授权，申请开发者开启PCS API权限【参阅上一条】转移文件到你PCS API权限目录。

问：为什么没有分页功能

答：没有分页功能，因为不会^_^

问：如何使用七牛镜像

答：首先保证你有七牛账号，选择你镜像空间->空间设置->镜像存储->镜像源->你部署代码的URL。如果没有七牛账号，可以使用我的邀请链接[https://portal.qiniu.com/signup?code=3lpd8yu42qv6a](https://portal.qiniu.com/signup?code=3lpd8yu42qv6a)

问：如何更新七牛镜像文件

答：目前请到七牛后台选择你镜像空间->内容管理->手动删除你要更新的文件

enjoy