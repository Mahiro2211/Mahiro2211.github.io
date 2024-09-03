---
layout: fragment
title: 指定python解释器创建jupyter内核
tags: [Python]
descriptiotn: python -m ipykernel install --user --name <environment_name>
keywords: Python    
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---
```shell
conda install ipykernel
python -m ipykernel install --user --name <environment_name>
conda activate <environment_name>
conda install matplotlib
```

将`<environment_name>`替换为Conda环境的名称。



``` shell
python -m ipykernel install --user --name <environment_name> --display-name "Python (<environment_name>)"
```

将`<environment_name>`替换为要使用的Conda环境的名称。




​