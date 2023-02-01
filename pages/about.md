---
layout: page
title: About
description:
keywords:
comments: true
menu: 关于
permalink: /about/
---

认真做事做好事 用心做事做对事

## 联系

<ul>
{% for website in site.data.social %}
<li>{{website.sitename }}：<a href="{{ website.url }}" target="_blank">@{{ website.name }}</a></li>
<li>
Email:    wendaocs@hotmail.com
</li>
{% endfor %}
{% if site.url contains 'mazhuang.org' %}
{% endif %}
</ul>


## Skill Keywords

{% for skill in site.data.skills %}
### {{ skill.name }}
<div class="btn-inline">
{% for keyword in skill.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
