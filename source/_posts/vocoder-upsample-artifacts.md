---
title: 神经音频合成中的上采样瑕疵
tags:
- Python
- Machine Learning
- Vocoder
- Audio
date: 2023/07/28
---

论文原文: [Upsampling artifacts in neural audio synthesis](https://arxiv.org/abs/2010.14356)

## 导言
在实践声码器时，我们常选择使用生成对抗网络(GAN)或自回归模型。由于自回归模型生成过程较慢，我们更倾向于使用GAN。目前，大部分广泛应用的GAN都源于HiFiGAN的改进，然而，无论是HiFiGAN还是其改进版，都存在上采样瑕疵的问题，如下图：

<img src="https://imagedelivery.net/5O09_o54BtxkkrL59wq3ZQ/9ce1b81d-8d43-40ad-0988-803db385a400/public" alt="Viz" width="50%" />

尽管本文未能找到完美的解决方案，但更充足的训练和SubPixel CNN能够减轻这类瑕疵。（不过，SubPixel Convolution会引入新的瑕疵。）

## HiFiGAN
HiFiGAN的问题在于使用了Transposed Convolution（转置卷积）进行上采样，而这种方法会带来一些瑕疵。众所周知，转置卷积会导致Checkerboard瑕疵，这是因为其输出存在重叠部分。在HiFiGAN的默认配置中，我们的卷积核大小是步长的两倍。根据相关论文，这被称为Full Overlap，如下图：

<div style="display: flex; justify-content: space-between;">
<img src="https://imagedelivery.net/5O09_o54BtxkkrL59wq3ZQ/d407e96b-43dd-4964-e196-0d5e82a01200/public" alt="Overlap" style="width: 47%; object-fit: contain;" />
<img src="https://imagedelivery.net/5O09_o54BtxkkrL59wq3ZQ/09936ce3-1ef9-44d5-ef14-8039672a9000/public" alt="Transposed Conv Artifact" width="47%" />
</div>

经过多次上采样后，这种瑕疵在生成的样本中以明显的横线出现。

## 其他尝试
作者在此基础上，尝试了不同的上采样方案，包括：
- 最近邻插值
- 线性插值
- 转置卷积
- SubPixel卷积

<img src="https://imagedelivery.net/5O09_o54BtxkkrL59wq3ZQ/e2d4a19c-265a-4dd6-1bc2-cbfaf7595800/public" alt="Table" width="50%" />

结果显示，转置卷积和SubPixel卷积的效果相当，且在客观评价指标上优于其他方案。然而，基于插值的方案在主观听感上与另外两种方案相差不大。

## 总结
转置卷积依然是目前最优的解决方案，它被广泛应用在众多模型中，并且其速度远超插值。幸运的是，通过增加网络层数和充足的训练（例如：Demucs），这类瑕疵可以被大幅降低。

在附录中，作者提到在STFT中使用`center=False`可以避免产生和混合Boundary瑕疵。

## 避免上采样的方案
基于频谱的生成方案如Vocos和iSTFT Net可以避免上采样瑕疵，但它们也会引入一些新的瑕疵，这一方向还需要进一步的研究。

## 参考文献
- [Upsampling artifacts in neural audio synthesis](https://arxiv.org/abs/2010.14356)
  