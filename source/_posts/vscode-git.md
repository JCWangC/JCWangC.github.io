---
title: vscode-git
date: 2020-04-8 13:00:00
tags:
    - 驱动 
    - 英伟达
categories: 
    - 深度学习


---
# 背景
原来安装`tensorflow2.0`时，对应安装的`cuda`版本为`10.0`。今天使用 `nvidia-smi`命令查看显卡状态，发现cuda版本却是`10.1`。
从 `nvcc`命令来看，却是`CUDA 10.0`。

![nvidia](https://img-blog.csdnimg.cn/2020040812055676.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzQyOTM2MA==,size_16,color_FFFFFF,t_70#pic_center)

# 分析
CUDA 目前有两种不同的 API：Runtime API 和 Driver API，两种 API 各有其适用的范围。
高级API（cuda_runtime.h）是一种C++风格的接口，构建于低级API之上。
`nvidia-smi` 的结果除了有 GPU 驱动版本型号，还有 CUDA Driver API的型号，这里是 `10.1`。
而nvcc的结果是对应 CUDA Runtime API[<sup>[1]</sup> ](#refer-anchor-1)。

在安装CUDA 时候会安装3大组件，分别是 `NVIDIA 驱动`、`toolkit` 和 `samples`。NVIDIA 驱动是用来控制 GPU 硬件，`toolkit `里面包括`nvcc编译器`等，`samples`或者说`SDK` 里面包括很多样例程序包括查询设备、带宽测试等等。上面说的 `CUDA Driver API`是依赖于` NVIDIA 驱动`安装的，而`CUDA Runtime API `是通过`CUDA toolkit `安装的。


![nvidia2](https://img-blog.csdnimg.cn/20200408125144710.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzQyOTM2MA==,size_16,color_FFFFFF,t_70#pic_center)

图[<sup>[2]</sup> ](#refer-anchor-2)






# 参考
<div id="refer-anchor-1"></div>
[1] 

<div id="refer-anchor-2"></div>
[2] 
