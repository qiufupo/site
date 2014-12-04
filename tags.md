---
layout: default
title: 标签
permalink: /tags/
---
<div class="s_left">
{% for tag in site.tags %}
<a name="{{ tag | first }}"><h2>&#160;&#160;{{ tag | first }}<sup>{{ tag.last.size }}</sup></h2></a>
    <ul>
    {% for post in tag.last %}
      <li class="sa_abstract"><a href="{{ post.url }}">{{ post.title }}</a> <span class="ds-thread-count" data-thread-key="{{ post.id }}"></span>评论</li>
    {% endfor %}
    </ul>
{% endfor %}
</div>

<div class="sa_right clearfix">
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