---
layout: fragment
title: 避免.git文件过大
tags: [Git]
descriptiotn: git repack -a -d -f --depth=250 --window=250
keywords: Git
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

我在git pull的过程中发现了我的.git很大，于是我就去stackoverflow去找问题怎么解决

答案（我是可以用的，使用了之后我的.git文件从31mb到不到1mb

git repack -a -d -f --depth=250 --window=250

​