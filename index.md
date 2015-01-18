---
layout: default
---	
<div class="s_left sa_articleinfo">
{% for post in site.posts limit:8 %}
<div class="clearfix"></div>
	<h1><a href="{{ post.url }}">{{ post.title }}</a>&#160;<small><span class="ds-thread-count" data-thread-key="{{ post.id }}"></span>评论&#160;{{ post.date | date:"%b %e, %Y" }}</small></h1>
		{% if post.img %}<span class="sa_right_index sa_articleinfo"><a href="{{ post.url }}"><img src="{{ site.cdn_url }}{{ post.img }}" style="max-width:200px; max-height:200px;" alt="{{ post.title | escape }}" /></a></span>{% endif %}
		<blockquote>{{ post.description }}</blockquote>
		{% for tag in post.tags %}
		<span class="sa_articletag"><a href="/tags/#{{ tag }}">{{ tag }}</a></span>
		{% endfor %}
			{% endfor %}
</div>

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
<div class="sa_avatar"><img src="{{ site.production_url }}/images/author.jpg" alt="" /></div>
<div class="sa_text">
<p><span>{{ site.author.name }} {{ site.author.info }}</span></p>
<p>{{ site.author.description }}</p>
<p style="min-height:10px;"></p>
</div>
<ul class="clearfix sa_authorinfo">
<li><span class="s">{{ site.posts.size }}</span>文章</li>
<li><span class="s">{{ site.tags.size }}</span>标签</li>
<span id="socials">
<li id="weibo"><a class="ir" href="http://weibo.com/mjjinc" title="新浪微博" rel="external">kooker 新浪微博</a></li>
<li id="github"><a class="ir" href="https://github.com/kooker" title="github" rel="external">github</a></li>
<!-- <li id="mp3"><a class="ir" href="http://kooker.duapp.com/mp3" title="MP3分享列表" rel="external">MP3分享列表</a></li> -->
<li id="mp3"><a class="ir" href="http://qiufupo.jd-app.com/audio" title="MP3分享列表" rel="external">MP3分享列表</a></li>
<li id="rss"><a class="ir" href="{{ site.production_url }}/atom.xml" title="RSS" rel="external">RSS</a></li>
</span>
</ul>
</div>
</div>

<div class="sa_right_title"><span>友情推广</span></div>
<div class="clearfix">
<a href="http://www.8sa7.com/" target="_blank"><img src="http://qiufupo.qiniudn.com/img/qiufupo-ad-1.jpg" alt="推广" style="padding-left: 15px;" /></a>
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
<li><a href="https://www.kooker.jp/" title="">技术支持</a></li>
<li><a href="http://www.8sa7.com/" title="Fresh Proxy List Updated Daily">Free Fresh Proxies</a></li>
<li><a href="http://www.qiwei.org/" title="气总的大站" target="_blank">苹果气味</a></li>
<li><a href="http://www.junduo.com/" title="通过二维码收集客户信息" target="_blank">君多二维码</a></li>
</ul>
</div>

<div class="sa_hotarticles">
<div class="sa_right_title_b">常用链接</div>
<ul>
<li><a href="https://affiliate-program.amazon.com/" title="Amazon联盟_海外">Amazon联盟海外</a></li>
<li><a href="https://associates.amazon.cn/" title="Amazon联盟_中国">Amazon联盟中国</a></li>
<li><a href="http://www.luoqunju.com/book/42377" title="完美世界 辰东 全文阅读">完美世界</a></li>
<li><a href="https://www.value-domain.com/" title="Domain" target="_blank">value-domain</a></li>
<li><a href="http://releases.mozilla.org/pub/mozilla.org/firefox/releases/" title="Firefox Releases" target="_blank">Firefox Releases</a></li>
<li>ID:6513394</li>
</ul>
</div>

</div><!-- End  sa_right-->