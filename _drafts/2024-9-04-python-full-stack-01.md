---
layout: post
title: Python全栈开发流程
categories: [Python, FullStackDev]
description: Python大数据架构全栈开发
keywords: Python
mermaid: false
sequence: false
flow: false 
mathjax: false
mindmap: false
mindmap2: false
---

# 内部数据采集工具
## Scribe
## Logstash.
## 第三方流量数据采集分析系统
* Google Analytics (有免费版)
* 百度统计 (有免费版)

# 外部数据采集工具
对应的Python库
1.获取数据常用库
• urllib：除非要详细了解技术和原理实现细节，否则一般情况下很少使用。
• Requests和requests-cache：适用于简单或少量的网页、API的数据抓取。
• aiohttp:适用于海量网页的批量实现，对效率有更高的要求。
• httpx：适用于想要同时覆盖同步和异步的抓取场景。
• selenium：适用于动态页面内容的抓取，且抓取页面数量较少的场景。
• Splash：适用于动态页面内容的抓取，且抓取页面数量较多的场景。
2.解析数据常用库
• html和xml：适合简单、少量的HTML和XML的爬虫数据解析。
• lxml：适合较多的HTML和XML爬虫数据解析场景，且对解析效率有一定的要求。
• BeautifulSoup：同时支持简单和复杂的HTML与XML的解析，使用场景覆盖HTML和XML、LXML的库。
• re：适合以大量的非标准化文本解析为主的场景。
3.保存数据常用库
• 文本文件存储：适合抓取数据测试、DEMO及简单场景使用。
• SQLit：适合存储小数据量、数据的格式和字段很少变动的场景使用。
• aiosqlite：适合对存储的数据并发性有较高的要求、总体数据量较小、数据的格式和字段很少变动的场景使用。
• MongoDB：Web数据的主要存储方式，适合绝大部分场景使用。
• Motor：适合Web数据的存储且有高并发需求时使用，适合绝大部分场景使用。
4.爬虫常用框架
• Pyspider：适合中小型项目，在中小规模数据抓取、规则清晰、快速开发、简化运维和管理复杂度的场景下使用；同时需要考虑系统Python版本是否与Pyspider一致。
• Scrapy：适合大型项目，在有较多自定义功能需求、多功能集成、海量数据抓取场景下使用。
# 场景练习
1. 使用Requests+BeautifulSoup抓取数据并写入Sqlite
2. 使用Scrapy+XPath抓取数据并写入MongoDB