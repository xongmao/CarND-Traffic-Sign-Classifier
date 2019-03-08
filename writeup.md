### 数据集汇总与探索
#### 1.数据集的基本概要
训练集的大小是34799  
测试集的大小是12630  
交通标志图像的形状是(32,32,3)  
数据集的类别数量是43
#### 2.包括数据集的探索性可视化
下图为训练集各类别的数量分布图
![avatar](img1.png)  
训练集数据增强后的各类别的数量分布图如下
![avatar](img2.png)
### 模型体系结构的设计与测试
#### 1.描述如何对图像数据进行预处理
首先将原图转换成灰度图，如下例  
![avatar](img3.png)
![avatar](img4.png)  
再将灰度图正则化，如下例  
![avatar](img4.png)
![avatar](img5.png)
#### 2.描述您的最终模型体系结构的外观，包括模型类型、层、层大小、连接性等。
本项目采用DenseNet作为最终模型，大体如下图
![avatar](img6.png)
DenseNet模型结构如下

| Layers   | Output Size  | DenseNet  |
| :------- | -----------: | :-------: |
|Conv      |   28*28      | 33 conv  |
|Dense Block(1) | 28*28 | 33 conv  |
|Transition(1) | 14*14 | 11 conv + 22avg pool,stride 2|
|Dense Block(2) | 14*14 | 33 conv  |
|Transition(2) | 7*7 | 11 conv + 22avg pool,stride 2|
|Dense Block(3) | 7*7 | 33 conv  |
|Classification | 1*1 | 77avg pool,stride 7 + 43Dfully-conn |
Dense Block 模块中共12个3*3conv，每2个conv之间的连接如下  
input --> relu --> 33 conv --> dropout --> output  
input --> output  
Transition模块第一个有16个11conv，后面每个递增12，连接如下  
input --> 11conv --> dropout --> avg pool --> bn --> output  
Classification模块连接如下  
input --> bn --> relu --> avg pool --> full conn --> output
#### 3.描述你是如何训练你的模型的
训练参数如下  
learning rate: 1e-3  
weight_decay: 1e-4  
dropout_rate: 0.5  
epochs: 20 (early stopping for the accuracy rate more than 0.93)  
batch_size: 32
#### 4.描述用于寻找解决方案并使验证集精度至少为0.93的方法
最终结果如下  
Train Accuracy: 0.970  
Validation Accuracy: 0.950  
Test Accuracy: 0.947  
Test Recall: 0.915  
Test Precision: 0.919  
(I input all the data from the test set and got it error   'ResourceExhaustedError: OOM when allocating tensor with shape[12630,12,32,32]', So I input 200 samples were randomly selected from the test set to estimate recall rate and Precision rate.
)
### 在新图像上测试模型
#### 1.选择在网上找到的五个德国交通标志
图像和预测结果如下  
![avatar](img7.png)
![avatar](img8.png)
![avatar](img9.png)
![avatar](img10.png)
![avatar](img11.png)  
(There should be no mistake. accurate is 100%)
#### 2.描述当通过查看每个预测的软最大概率对五个新图像中的每个进行预测时，模型的确定性如何。提供每个图像的前5个概率以及每个概率的符号类型。
例如第1张图片前5个概率以及每个概率的符号类型如下

| Probability   | Prediction  |
| :------- | :-----------: |
|.52 | Stop |
|.42 | Road narrows on the right |
|.25 | Speed limit(120km/h) |
|.18 | Speed limit(100km/h) |
|.07 | Speed limit(20km/h) |
### 参考文献
https://github.com/ikhlestov/vision_networks  
https://arxiv.org/pdf/1608.06993v3.pdf#4
