---
title: 联机托管说明
permalink: index.html
layout: home
---

# 内容目录

下面列出了指向每个案例研究的超链接。


## 为 2023 年 5 月内容更新重新整理的案例研究

{% assign casestudy= site.pages | where_exp:"page", "page.url contains '/Instructions/CaseStudyv2/'" %}
| 模块 | CaseStudy |
| --- | --- | 
{% for activity in casestudy  %}| {{ activity.casestudy.module }} | [{{ activity.casestudy.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}


## 旧案例研究整理

{% assign casestudy= site.pages | where_exp:"page", "page.url contains '/Instructions/CaseStudy/'" %}
| 模块 | CaseStudy |
| --- | --- | 
{% for activity in casestudy  %}| {{ activity.casestudy.module }} | [{{ activity.casestudy.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}