---
title: 深度学习常用框架
tags:
- OpenCV
- Python
- Machine Learning
---

以下框架均来源于个人 PyTorch 开发经历, 仅供参考. (以下内容不分先后)

- [Albumentations](https://github.com/albumentations-team/albumentations): 一个基于 OpenCV 的图像增强/解析库, 速度爆杀 torchvision (Pillow-SIMD).
- [Einops](https://einops.rocks/): 一个 PyTorch 的 Tensor 操作库, 可以在大幅简化 transpose, reshape, squeeze, unsqueeze 等操作.
- [PyTorch Lightning](https://pytorch-lightning.readthedocs.io/): 一个 PyTorch 的高级封装, 可以大幅简化训练过程的代码, 如 DDP, Logging, Checkpointing, 等等.
- [PyTorch Image Models](https://github.com/rwightman/pytorch-image-models): (TIMM): 一个耳熟能详的 CV 模型库.
- [HuggingFace Transformers](https://huggingface.co/transformers/): 一个耳熟能详的 NLP 模型库.
- [ONNX Runtime](https://onnxruntime.ai/): 一个 ONNX 的运行时, 可以用来大幅加速 PyTorch 模型的推理, 支持 CPU, CUDA, CoreML, TensorRT 等后端.
- [WandB](https://wandb.ai/): 一个可视化工具 (类似 tensorboard), 可以用来记录训练过程中的各种指标, 以及可视化训练过程中的图片, 模型, 等等. 

如果你有其他推荐, 欢迎在评论区留言.
