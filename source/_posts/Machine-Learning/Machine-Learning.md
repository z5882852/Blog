---
title: 使用卷积神经网络模型来识别验证码
date: 2023-07-18 01:02:47
tags: [Python,爬虫,机器学习,卷积神经网络,验证码]
---

## 前言

当使用Python爬虫爬取数据或者脚本登录时可能会遇到人机验证，常见的有图片验证码、滑动拼图验证、文字点选验证、空间推理验证等，为了通过这些验证我们可以使用模型来识别这些验证图片的特征。

**以下是使用`Python` `TensorFlow`构建和训练卷积神经网络(CNN)模型的示例**

## 准备工作

### 一、安装TensorFlow库

> 请阅读[tensorflow官网](https://tensorflow.google.cn/install/pip?hl=zh-cn)

#### 系统要求
- Python 3.6–3.9
  - 若要支持 Python 3.9，需要使用 TensorFlow 2.5 或更高版本。
  - 若要支持 Python 3.8，需要使用 TensorFlow 2.2 或更高版本。
- pip 19.0 或更高版本(需要 manylinux2010 支持)
- Ubuntu 16.04 或更高版本(64 位)
- macOS 10.12.6 (Sierra) 或更高版本(64 位)(不支持 GPU)
- macOS 要求使用 pip 20.3 或更高版本
- Windows 7 或更高版本(64 位)
  - [适用于 Visual Studio 2015、2017 和 2019 的 Microsoft Visual C++ 可再发行软件包](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)
- [GPU 支持](https://tensorflow.google.cn/install/gpu?hl=zh-cn)需要使用支持 CUDA® 的卡(适用于 Ubuntu 和 Windows)

这里我使用的系统是`Windows11-64位`，并且使用CPU训练模型，所以不需要安装CUDA

#### 创建虚拟环境

在项目根目录下创建一个`.\venv`目录的虚拟环境
```PowerShell
python -m venv --system-site-packages .\venv
```
激活虚拟环境
```PowerShell
.\venv\Scripts\activate
```
升级pip
```PowerShell
pip install --upgrade pip
```
#### 安装 TensorFlow pip 软件包

未完待续...
