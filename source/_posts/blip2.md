---
title: 使用 BLIP 2 来增强多模态数据集
tags:
- Python
- Machine Learning
- Multimodal
- CLIP
date: 2023/07/21
---

论文原文: [Improving Multimodal Datasets with Image Captioning](https://arxiv.org/abs/2307.10350)

## 选择 BLIP 2 的原因
<img src="https://imagedelivery.net/5O09_o54BtxkkrL59wq3ZQ/5b0cc3b0-61af-4d07-81ed-d05dc22cbb00/public" alt="BLIP 2" width="50%" />

BLIP 2 的核心组件包括 Frozen Image Encoder、Frozen LLM 以及 Q-Former。它在 Image Captioning 方面表现出了强大的性能，特别是在生成样本的 Diversity 方面，显著超过了 OpenCLIP-CoCa。因此，我们选择 BLIP 2 来提升多模态数据集的效能。

## 多模态数据集中的挑战
<div style="display: flex; justify-content: space-between;">
<img src="https://imagedelivery.net/5O09_o54BtxkkrL59wq3ZQ/71a64ee9-195e-4425-1da1-1ef843e6dc00/public" alt="CLIP Score" style="width: 50%; object-fit: contain;" />
<img src="https://imagedelivery.net/5O09_o54BtxkkrL59wq3ZQ/d583c80d-f6b0-4323-1278-e6a1b7a3b300/public" alt="Diversity & Noise" width="40%" />
</div>

我们常常面临的问题是数据集的规模只有 100-200M，远未达到 Billion 级别。在这种情况下，数据质量的重要性不言而喻。然而，像 DataComp 128M 这样的数据集的 CLIP Score 平均只有 0.2，其中存在大量与文字不相关的图片和噪声。一个直接的解决方法是过滤掉与文字不相关的图片，仅保留 top 30%，但这会导致数据集的规模大幅度缩减，且一定程度上降低了数据的 Diversity。

## 解决策略
作者在试验和数据分析后，选择了如下策略：

1. 计算已有数据集的 CLIP Score，保留 top 30% 的图片。
2. 对剩余的 70% 的图片使用 BLIP 2 生成 caption。
3. 对生成的 Caption 进行过滤，过滤标准与 Step 1 相同。

这个策略实际上可以看作是对 BLIP 2 进行了一种形式的 knowledge distillation，使模型学习到了 ViT-G 和 BLIP2 数据集 LAION400M 的分布。实际上，当数据集的规模扩大到 400M 以上时，这种策略的效果几乎与直接保留原始数据的 Top 30% 相当。

![Comparison](https://imagedelivery.net/5O09_o54BtxkkrL59wq3ZQ/7ddf3050-eb3e-4832-5616-dc00e77d4300/public)  

## 参考文献
- [BLIP-2: Bootstrapping Language-Image Pre-training with Frozen Image Encoders and Large Language Models](https://arxiv.org/abs/2301.12597)
- [Improving Multimodal Datasets with Image Captioning](https://arxiv.org/abs/2307.10350)
