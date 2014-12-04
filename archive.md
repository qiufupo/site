---
layout: default
title: 存档
permalink: /archive/
---
<div class="s_left">
  {% capture post_year1 %}{{ site.time | date: '%Y' }}{% endcapture %}
<h2>&#160;&#160;{{ post_year1 }}</h2>
<ul>
{% for post in site.posts %}
{% capture post_year2 %}{{ post.date | date: '%Y' }}{% endcapture %}
{% if post_year1 != post_year2 %}
{% assign post_year1 = post_year2 %}
</ul>
<h2>&#160;&#160;{{ post_year1 }}</h2>
<ul>
{% endif %}
<li class="sa_abstract"><h3><span>{{ post.date | date: "%b %d, %Y" }}</span> &raquo;  <a href="{{ post.url }}">{{ post.title }}</a> <span class="ds-thread-count" data-thread-key="{{ post.id }}"></span>评论</h3></li>
{% endfor %}
</ul>
</div>

<div class="sa_right clearfix">
<div class="sa_right_title"><span>标签云</span></div>
<div class="sa_articleinfo top_box">
<div class="sa_articletag clearfix">  
{% for tag in site.tags %}
<a href="/tags/#{{ tag | first }}">{{ tag | first }}<sup>{{ tag.last.size }}</sup></a>
{% endfor %}</div>
</div>

<div class="sa_hotarticles">
<div class="sa_right_title_b">最新评论</div>
<ol class="ds-recent-comments" data-num-items="8" data-show-avatars="1" data-show-time="0" data-show-admin="0" data-excerpt-length="20"></ol>
</div>

<div class="sa_right_title"><span>友情推广</span></div>
<div class="clearfix">
<a href="http://www.8sa7.com/" target="_blank"><img src="http://qiufupo.qiniudn.com/img/qiufupo-ad-1.jpg" alt="推广" style="padding-left: 15px;" /></a>
</div>

<div class="sa_right_title"><span>二维码</span></div>
<div class="clearfix">
<img src='http://chart.googleapis.com/chart?cht=qr&chl={{ site.production_url }}{{ page.url }}&chld=H|0&choe=UTF-8&chs=200x200'  style="padding-left: 25px;max-width:200px; max-height:200px;" alt="{{ page.title | escape }} QR"  />
</div>

</div><!-- End  sa_right-->