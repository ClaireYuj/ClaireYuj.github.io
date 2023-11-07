---
title: 识别mnist手写数字数据集
top: false
cover: false
author: DULULU oO
date: 2021-07-11 20:58:58
password:
summary:
tags:
    - Python
    - 机器学习
categories: 机器学习
---

MNIST包含70,000张手写数字图像: 60,000张用于培训，10,000张于测试。图像是灰度的，28x28像素的。  
识别mnist手写数字及可以视作图像识别的入门学习任务  
需要准备pytorch环境


```Python
import torch
import torchvision
from torch.utils.data import DataLoader
```

## 定义参数

```Python
n_epochs = 3 # 整个训练集训练的次数
batch_size_train = 64  # 训练集的批次大小
batch_size_test = 1000  # 测试机batchsize
learning_rate = 0.01
momentum = 0.5
log_interval = 10
# 对于可重复实验设置随机种子，进行重复实验，设置随机种子
random_seed = 1
torch.manual_seed(random_seed)

```

learning_rate和momentum是我们稍后将使用的优化器的超参数。  

## 下载数据集

直接利用torchvision加载minist数据集，我们将使用batch_size=64进行训练，并使用size=1000对这个数据集进行测试。下面的Normalize()转换使用的值0.1307和0.3081是MNIST数据集的全局平均值和标准偏差，此处我们将它们作为给定值。

```Python
 # 下载训练集和测试集, './minist_data'是目录下相对路径
 # 记得要设置download为true，不然可能下载到一般突然中断
 # train=True是训练集，batch_size=batch_size_train
train_loader = dataLoader(
    torchvision.datasets.MNIST('./mnist_data',train=True, download=True,
                          transform= torchvision.transforms.Compose([
                              torchvision.transforms.ToTensor(),
                              torchvision.transforms.Normalize((0.1307,),(0.3081,))
                          ])),
    batch_size=batch_size_train, shuffle=True
)

 # train=False是测试集，batch_size=batch_size_test
test_loader = dataLoader(
    torchvision.datasets.MNIST('./mnist_data',train=False, download=True,
                          transform= torchvision.transforms.Compose([
                              torchvision.transforms.ToTensor(),
                              torchvision.transforms.Normalize((0.1307,),(0.3081,))
                          ])),
    batch_size=batch_size_test, shuffle=True
)

```

下载完成后，在目录./mnist_data/MNIST/raw目录下可以看到这些
![](/img/posts/Programming/mnist0.jpg)

利用train_loader来看训练数据的组成
```Python
examples = enumerate(train_loader)
batch_idx, (example_data, example_targets) = nex(examples)
print(example_targets)  # example_targets是图片实际对应的数字标签
print(example_data.shape)  # 一批训练数据是一个数据张量
```

输出结果
![](/img/posts/Programming/mnist1.jpg)

这意味着我们有64个例子的28x28像素的灰度(即没有rgb通道)。

尝试用mathplotllib绘制部分

```Python
import matplotlib.pyplot as plt
fig = plt.figure()
for i in range(12):
    plt.subplot(3,4,i+1)  # subplot(m,n,i) 说明是m*n的网格表示，i表示每个plot的编号， 这样可以把几张图放在一张图片中表示出来
    plt.tight_layout()
    plt.imshow(example_data[i][0], cmap='gray', interpolation='none')
    plt.title("Ground Truth:{}".format(example_targets[i]))
    plt.xticks([])
    plt.yticks([])
plt.show()
```

![输出结果](/img/posts/Programming/mnist2.jpg)

## 构建网络
