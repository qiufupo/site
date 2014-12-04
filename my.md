---
layout: my
permalink: /my/
---

<div class="s_left_c clearfix">
{% for post in site.posts limit:15 %}
<div class="s_left_l clearfix"><div class="s_i_article">
		{% if post.img %}<div class="image clearfix"><a href="{{ post.url }}"><img src="{{ site.cdn_url }}{{ post.img }}" style="max-width:200px; max-height:200px;" alt="{{ post.title | escape }}" /></a></div>{% endif %}
		<div class="title"><a href="{{ post.url }}">{{ post.title }}</a></div>
<!--<div class="text">{{ post.description }}</div>-->
</div></div>
{% endfor %}</div></div>

<div class="sa_right clearfix">
<div class="sa_right_title"><span>邮箱订阅</span></div>
<div class="sa_search">
<form method="post" target="_blank" action="http://list.qq.com/cgi-bin/qf_compose_send">
<input value="qf_booked_feedback" name="t" type="hidden">
<input value="a74128e4e2870a02306bf01133ae8f61d7fd559751b992bd" name="id" type="hidden">
<input class="feed-mail-input" name="to" value="输入邮箱 订阅求富婆" onblur="if (this.value == '') {this.value = '输入邮箱 订阅求富婆';}" onfocus="if (this.value == '输入邮箱 订阅求富婆') {this.value = '';}" id="to" type="text"><input value="订阅" class="btn btn_slide" type="submit">
</form></div>

<div class="sa_articleinfo top_box">
<script type="text/javascript" src="http://exmail.qq.com/zh_CN/htmledition/js_biz/outerlogin.js"  charset="gb18030"></script>
<script type="text/javascript">
writeLoginPanel({domainlist:"qiufupo.com", mode:"vertical"});
</script>
</div>

<div class="s_i_r_title"><span>作者信息</span></div>
<div class="sa_articleinfo">
<div class="clearfix">
<div class="sa_avatar"><img src="{{ site.production_url }}/images/author.jpg"></div>
<div class="sa_text">
<p><span>{{ site.author.name }} {{ site.author.info }}</span></p>
<p>{{ site.author.description }}</p>
<p style="min-height:10px;"></p>
</div>
<ul class="clearfix sa_authorinfo">
<li><span class="s">{{ site.posts.size }}</span>文章</li>
<li><span class="s">{{ site.tags.size }}</span>标签</li>
<aside id="socials">
<li id="weibo"><a class="ir" href="http://weibo.com/mjjinc" title="新浪微博" rel="external">kooker 新浪微博</a></li>
<li id="github"><a class="ir" href="https://github.com/kooker" title="github" rel="external">github</a></li>
<li id="mp3"><a class="ir" href="http://kooker.duapp.com/mp3" title="MP3分享列表" rel="external">MP3分享列表</a></li>
<li id="rss"><a class="ir" href="{{ site.production_url }}/atom.xml" title="RSS" rel="external">RSS</a></li>
</aside>
</ul>
</div>
</div>

<div class="sa_right_title"><span>赞助商推广</span></div>
<div class="sa_articleinfo top_box">
<a href="http://s.click.taobao.com/t?e=m%3D2%26s%3D%2FSLDAqoF9AccQipKwQzePCperVdZeJviEViQ0P1Vf2kguMN8XjClAmb49CDgx3duPBt%2BcTtnoaIr3oTizfLsGtPgpUq3lV81gB%2FY1CvM8eurTn%2B86ghju%2Bdn1BbglxZYxUhy8exlzcq9AmARIwX9K%2BnbtOD3UdznPV1H2z0iQv9NkKVMHClW0QbMqOpFMIvnvjQXzzpXdTHGJe8N%2FwNpGw%3D%3D" target="_blank"><img src="http://gtms01.alicdn.com/tps/i1/T14a8XFrplXXXQQFY4-200-200.jpg" /></a>
</div>

<div class="sa_right_title"><span>二维码</span></div>
<div class="clearfix">
<img src="{{ site.production_url }}/images/er.jpg" alt="求富婆二维码"  style="padding-left: 25px;" />
</div>

<div class="sa_hotarticles">
<div class="sa_right_title_b">最新评论</div>
<ol class="ds-recent-comments" data-num-items="8" data-show-avatars="1" data-show-time="0" data-show-admin="0" data-excerpt-length="20"></ol>
</div>

<div class="sa_hotarticles">
<div class="sa_right_title_b">友情链接</div>
<ul>
<li><a href="http://www.kooker.jp" title="">技术支持</a></li>
<li><a href="https://list.kooker.jp/" title="Free Fresh Proxy List Updated Daily" target="_blank">Free Fresh Proxies</a></li>
</ul>
</div>

</div><!-- End  sa_right-->