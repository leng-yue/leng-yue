---
title: 构建 OpenCV Contrib Python
tags:
- OpenCV
- Python
---
很多时候我们需要使用 OpenCV 的一些功能 (如 viz), 但是 OpenCV 官方的 Python Contrib 版本并没有提供这些功能, 这时候我们就需要使用自己构建.  
然而, 手动从官方下载源码然后构建相当繁琐, 下面的代码将使用 pip 来构建 OpenCV Contrib Python.

```sh
export ENABLE_CONTRIB=1  # 启用 contrib
export CMAKE_ARGS="-DWITH_VTK=ON -DOPENCV_ENABLE_NONFREE=ON"  # 启用 VTK 和非免费功能
export MAKEFLAGS="-j$(nproc)"  # 使用多核编译
pip install -v --no-binary=opencv-contrib-python opencv-contrib-python
```

在这里, 我使用 `CMAKE_ARGS` 来指定 CMAKE 参数, 你们可以根据自己的需求调整.
