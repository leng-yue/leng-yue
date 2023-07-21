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

## 为什么选择 BLIP 2?
<img src="https://imagedelivery.net/5O09_o54BtxkkrL59wq3ZQ/5b0cc3b0-61af-4d07-81ed-d05dc22cbb00/public" alt="BLIP 2" width="50%" />

BLIP 2 由 Frozen Image Encoder, Frozen LLM, 和 Q-Former 组成. 具有较好的 Image Captioning 性能, 其生成样本的 Diversity 优于 OpenCLIP-CoCa. 因此, 本文使用 BLIP 2 来增强多模态数据集.

## 多模态数据集中的问题
<div style="display: flex; justify-content: space-between;">
<!-- Keep aspect ratio -->
<img src="https://imagedelivery.net/5O09_o54BtxkkrL59wq3ZQ/71a64ee9-195e-4425-1da1-1ef843e6dc00/public" alt="CLIP Score" style="width: 50%; object-fit: contain;" />
<img src="https://imagedelivery.net/5O09_o54BtxkkrL59wq3ZQ/d583c80d-f6b0-4323-1278-e6a1b7a3b300/public" alt="Diversity & Noise" width="40%" />
</div>

很多时候, 我们的数据只有 100-200M, 达不到 Billion 的级别. 这种情况下数据的质量尤为重要. 然而, DataComp 128M 的 CLIP Score 平均只有 0.2 (大量图文不相关, 噪音). 一种粗暴的方案是直接过滤掉不相关的图片, 只保留 top 30%, 这会导致数据严重缩水, 并且在一定程度上失去 Diversity.

## 解决方案
在试验和数据后, 作者选择

1. 计算已有数据集的 CLIP Score, 保留 top 30% 的图片
2. 对剩余 70% 的图片使用 BLIP 2 生成 caption
3. 对生成的 Caption 进行过滤, 阈值和 Step 1 一致

在笔者看来, 这种方案其实是对 BLIP 2 进行了一定程度上的 knowledge distillation, 让模型学习到了 ViT-G 和 BLIP2 数据集 LAION400M 的分布. 事实上, 随着把数据集 scale 到 400M 以上, 这种方案的效果几乎和直接保留原始数据的 Top 30% 一致.

![Comparison](https://imagedelivery.net/5O09_o54BtxkkrL59wq3ZQ/7ddf3050-eb3e-4832-5616-dc00e77d4300/public)  


## 参考
- [BLIP-2: Bootstrapping Language-Image Pre-training with Frozen Image Encoders and Large Language Models](https://arxiv.org/abs/2301.12597)
- [Improving Multimodal Datasets with Image Captioning](https://arxiv.org/abs/2307.10350)
