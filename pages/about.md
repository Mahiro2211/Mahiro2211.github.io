---
layout: page
title: About
description: 打码改变世界
keywords: Zhuang Ma, 马壮
comments: true
menu: 关于
permalink: /about/
---

## 我是谁
在山东的一名计算机专业的本科生，我喜欢使用python和javascript构建项目
## 我做了什么
我在大一和大二两个学年中学习了Python(了解Python的高级特性)并且做了一些深度学习相关的科研训练(主要是计算机视觉中的图像检索相关的任务)
## 我可以做
* 可以独立编写数据集加载脚本的能力，尽量去提升每一项任务的效率。
* 能够将度量学习等其他领域的相关算法迁移到其他方向，验证是否能提升性能。
* 能通过查阅相关技术文档快速入门一门新的框架和技术

## 联系

Github:[Mahiro2211](https://github.com/Mahiro2211)
Mail:dj4569103@gmail.com
WeChat:YUTT6678

## Skill Keywords

{% for skill in site.data.skills %}
### {{ skill.name }}
<div class="btn-inline">
{% for keyword in skill.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
