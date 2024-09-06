---
layout: page
title: About
description: 打码改变世界
keywords: Zhuang Ma, 马壮
comments: true
menu: 关于
permalink: /about/
---
# 个人简介

在山东的一名计算机专业的本科生，我喜欢使用Python和JavaScript构建项目。  
我在大一和大二两个学年中学习了Python（了解Python的高级特性），并且做了一些深度学习相关的科研训练（主要是计算机视觉中的图像检索相关的任务）。

## 目录
1. [我目前会的技能](#我目前会的技能)
2. [项目介绍](#在实验室期间完成的开源代码)
   - [GradCam-Auto](#1-gradcam-auto)
   - [CrossHash](#2-crosshash)
   - [Auto-Retrieval-Util](#3-auto-retrieval-util)
3. [联系](#联系)
4. [技能关键词](#技能关键词)

## 我目前会的技能
* 可以独立编写数据集加载脚本的能力，尽量去提升每一项任务的效率。
* 能够将度量学习等其他领域的相关算法迁移到其他方向，验证是否能提升性能。
* 能通过查阅相关技术文档快速入门一门新的框架和技术。



# 在实验室期间完成的开源代码

## 1. GradCam-Auto
[GradCam-Auto](https://github.com/Mahiro2211/GradCAM_Auto)  
在处理实验结果数据时，通过将训练好的模型的权重导入模型后，使用自动化脚本批量处理图像，可以快速得到结果。
<br>代码运行示例见:https://mahiro2211.github.io/2023/12/08/grad-cam-auto/
- **自动化脚本**：支持批量处理图像。
- **可视化结果**：通过Grad-CAM技术生成可视化结果，便于分析模型的关注区域。
- **易用接口**：提供易于使用的接口，适合快速实验和结果验证。

---

## 2. CrossHash
[CrossHash](https://github.com/Mahiro2211/CrossHash)  
这是一个使用CrossFormer作为骨干网络，改进交叉熵作为损失函数的大规模图像哈希检索方法。

- **增强特征提取**：使用CrossFormer作为骨干网络，增强特征提取能力。
- **优化训练过程**：改进的交叉熵损失函数，优化模型训练过程。
- **高效检索**：适用于大规模图像数据集的高效检索。

---

## 3. Auto-Retrieval-Util
[Auto-Retrieval-Util](https://github.com/Mahiro2211/Eval_Image_Retrieval_and_Cross_Modal_Retrieval)  
这是一个自动脚本，通过保存的哈希码自动计算所有需要的性能指标，输出格式为CSV，方便快速粘贴到Visio进行实验数据可视化。

- **自动计算指标**：自动计算图像检索和跨模态检索的性能指标。
- **便捷输出格式**：输出格式为CSV，便于数据处理和可视化。
- **提高效率**：提高实验效率，简化数据分析过程。

---

## 联系
- **Github**: [Mahiro2211](https://github.com/Mahiro2211)  
- **Mail**: dj4569103@gmail.com  
- **WeChat**: YUTT6678  

## 技能关键词
{% for skill in site.data.skills %}
### {{ skill.name }}
<div class="btn-inline">
{% for keyword in skill.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}





<!-- ## 个人简介
在山东的一名计算机专业的本科生，我喜欢使用python和javascript构建项目
<br>
我在大一和大二两个学年中学习了Python(了解Python的高级特性)并且做了一些深度学习相关的科研训练(主要是计算机视觉中的图像检索相关的任务)
## 我可以做
* 可以独立编写数据集加载脚本的能力，尽量去提升每一项任务的效率。
* 能够将度量学习等其他领域的相关算法迁移到其他方向，验证是否能提升性能。
* 能通过查阅相关技术文档快速入门一门新的框架和技术


## 实验室期间完成的开源代码

# 项目介绍

## 1. GradCam-Auto
[GradCam-Auto](https://github.com/Mahiro2211/GradCAM_Auto)  
在处理实验结果数据时，通过将训练好的模型的权重导入模型后，使用自动化脚本批量处理图像，可以快速得到结果。

- **自动化脚本**：支持批量处理图像。
- **可视化结果**：通过Grad-CAM技术生成可视化结果，便于分析模型的关注区域。
- **易用接口**：提供易于使用的接口，适合快速实验和结果验证。

---

## 2. CrossHash
[CrossHash](https://github.com/Mahiro2211/CrossHash)  
这是一个使用CrossFormer作为骨干网络，改进交叉熵作为损失函数的大规模图像哈希检索方法。

### 主要特点：
- **增强特征提取**：使用CrossFormer作为骨干网络，增强特征提取能力。
- **优化训练过程**：改进的交叉熵损失函数，优化模型训练过程。
- **高效检索**：适用于大规模图像数据集的高效检索。

---

## 3. Auto-Retrieval-Util
[Auto-Retrieval-Util](https://github.com/Mahiro2211/Eval_Image_Retrieval_and_Cross_Modal_Retrieval)  
这是一个自动脚本，通过保存的哈希码自动计算所有需要的性能指标，输出格式为CSV，方便快速粘贴到Visio进行实验数据可视化。

### 主要特点：
- **自动计算指标**：自动计算图像检索和跨模态检索的性能指标。
- **便捷输出格式**：输出格式为CSV，便于数据处理和可视化。
- **提高效率**：提高实验效率，简化数据分析过程。

--- -->

<!-- 这些项目展示了我在计算机视觉领域的研究兴趣和技术能力，期待与更多的研究者和开发者合作，共同推动这一领域的发展。
GradCam-Auto:[GithubRepoLink](https://github.com/Mahiro2211/GradCAM_Auto)<br>
在处理实验结果数据时通过将训练好的模型的权重导入模型后通过自动化脚本批量处理图片可以快速得到结果
自动化脚本，支持批量处理图像。
通过Grad-CAM技术生成可视化结果，便于分析模型的关注区域。
提供易于使用的接口，适合快速实验和结果验证。
CrossHash:[GithubRepoLink](https://github.com/Mahiro2211/CrossHash)
这是一个使用CrossFormer作为Backbone,改进交叉熵作为损失函数的一个大规模图像哈希检索方法
使用CrossFormer作为骨干网络，增强特征提取能力。
改进的交叉熵损失函数，优化模型训练过程。
适用于大规模图像数据集的高效检索。  
Auto-Retrieval-Util[GithubRepoLink](https://github.com/Mahiro2211/Eval_Image_Retrieval_and_Cross_Modal_Retrieval)
这是一个自动脚本，通过保存的哈希码自动算出所有需要的性能指标，输出格式为csv方便快速粘贴到visio进行实验数据可视化
自动计算图像检索和跨模态检索的性能指标。
输出格式为CSV，便于数据处理和可视化。
提高实验效率，简化数据分析过程。 -->
<!-- 
## 联系

Github:[Mahiro2211](https://github.com/Mahiro2211)<br>
Mail:dj4569103@gmail.com<br>
WeChat:YUTT6678<br>

## Skill Keywords

{% for skill in site.data.skills %}
### {{ skill.name }}
<div class="btn-inline">
{% for keyword in skill.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %} -->
