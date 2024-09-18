---
layout: page
title: About
description: 打码改变世界
keywords: Zhuang Ma, 马壮
comments: true
menu: 关于
permalink: /about/
---
## 技能关键词
{% for skill in site.data.skills %}
### {{ skill.name }}
<div class="btn-inline">
{% for keyword in skill.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}


##  👋  关于我
> 潍坊学院的计算机科学与技术专业的一名本科生，我喜欢使用Python和JavaScript来写东西
> 我在大一和大二两个学年中学习了Python（了解Python的高级特性），并且做了一些深度学习相关的科研训练（主要是计算机视觉中的图像检索相关的任务）。

---

## 🚀 技能关键词
{% for skill in site.data.skills %}
### {{ skill.name }}
<div class="btn-inline">
{% for keyword in skill.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}

---

##  💪  我的技能
*  可以独立编写数据集加载脚本的能力，尽量去提升每一项任务的效率。
*  能够将度量学习等其他领域的相关算法迁移到其他方向，验证是否能提升性能。
*  能通过查阅相关技术文档快速入门一门新的框架和技术。

---

##  💻  项目经验
### 1. GradCam-Auto  [Github](https://github.com/Mahiro2211/GradCAM_Auto) | [博客文章](https://mahiro2211.github.io/2023/12/08/grad-cam-auto/)
在处理实验结果数据时，通过将训练好的模型的权重导入模型后，使用自动化脚本批量处理图像，可以快速得到结果。
<br>
*   **自动化脚本**: 支持批量处理图像。
*   **可视化结果**: 通过Grad-CAM技术生成可视化结果，便于分析模型的关注区域。
*   **易用接口**: 提供易于使用的接口，适合快速实验和结果验证。

### 2. CrossHash  [Github](https://github.com/Mahiro2211/CrossHash)
这是一个使用CrossFormer作为骨干网络，改进交叉熵作为损失函数的大规模图像哈希检索方法。
<br>
*   **增强特征提取**: 使用CrossFormer作为骨干网络，增强特征提取能力。
*   **优化训练过程**: 改进的交叉熵损失函数，优化模型训练过程。
*   **高效检索**: 适用于大规模图像数据集的高效检索。

### 3. Auto-Retrieval-Util  [Github](https://github.com/Mahiro2211/Eval_Image_Retrieval_and_Cross_Modal_Retrieval)
这是一个自动脚本，通过保存的哈希码自动计算所有需要的性能指标，输出格式为CSV，方便快速粘贴到Visio进行实验数据可视化。
<br>
*   **自动计算指标**: 自动计算图像检索和跨模态检索的性能指标。
*   **便捷输出格式**: 输出格式为CSV，便于数据处理和可视化。
*   **提高效率**: 提高实验效率，简化数据分析过程。

---

**💻  技术爱好:**

*   💡 学习新技术：  "我对学习新技术充满热情 💡"
*   💻  编程： "喜欢用代码来自动化一些繁琐的任务。对Python比较擅长。 💻"
*   🤖  人工智能： "大一大二做过计算机视觉相关的一些科研训练，在实验室处理实验结果或者拼接一些代码 🤖"

**🎨  创意爱好:**

*   🎶  音乐： "喜欢Jpop 最喜欢的歌手是Vaundy🎶"
*   ✍️ 写作： "我喜欢用文字记录生活，分享我的思考和感悟。 ✍️"

**✈️  其他爱好:**

*   📚  阅读： "一些人文社科或者是心理学的书籍我很喜欢，比如《刻意练习》，《稀缺》 📚"

##  📞  联系我

*   **Github**:  [Mahiro2211](https://github.com/Mahiro2211)
*   **邮箱**: dj4569103@gmail.com
*   **微信**: YUTT6678 
