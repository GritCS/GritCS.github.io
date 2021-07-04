---
title: 在GPU上加速
date: 2020-09-03 09:36:14
tags: [pytorch,大创]
categories: [深度学习, pytorch]
---

## 回顾基本训练过程(MINIST)

### 数据准备

#### 导包、设计训练参数

```python
import  torch
import  torch.nn as nn
import  torch.nn.functional as F
import  torch.optim as optim
from    torchvision import datasets, transforms

batch_size=200
learning_rate=0.01
epochs=10
```

下载数据

```python
train_loader = torch.utils.data.DataLoader(
    datasets.MNIST('../data', train=True, download=True,
                   transform=transforms.Compose([
                       transforms.ToTensor(),
                       transforms.Normalize((0.1307,), (0.3081,))对数据进行正则化
                   ])),
    batch_size=batch_size, shuffle=True)
    
    
test_loader = torch.utils.data.DataLoader(
    datasets.MNIST('../data', train=False, transform=transforms.Compose([
        transforms.ToTensor(),
        transforms.Normalize((0.1307,), (0.3081,))
    ])),
    batch_size=batch_size, shuffle=True)
```

### 前向传播

#### 参数初始化

```python
w1, b1 = torch.randn(200, 784, requires_grad=True),\
         torch.zeros(200, requires_grad=True)
w2, b2 = torch.randn(200, 200, requires_grad=True),\
         torch.zeros(200, requires_grad=True)
w3, b3 = torch.randn(10, 200, requires_grad=True),\
         torch.zeros(10, requires_grad=True)#class_size = 10，最后分为10类
    
#利用何恺明大佬的初始化方法
torch.nn.init.kaiming_normal_(w1)
torch.nn.init.kaiming_normal_(w2)
torch.nn.init.kaiming_normal_(w3)
```



#### 搭建网络

```python
def fforward():
    x = x@w1 + b1
    x =F. relu(x)
    x = x@w2 + b2
    x = F.relu(x)
    x = x@w3+ b3
    return x
```

#### 定义加速器和loss函数

```python
optimizer = optim.SGD([w1,b1,w2,b2,w3,b3],lr = learning_rate)#传入需要计算梯度的参数和learning_rate
criteon = nn.CrossEntropyLoss()
```

### 反向传播

```python
for epoch in len(epochs):
	for batch_idx , (data,target) in enumerate(train_loader):
        data= data.view(-1,28*28)#view(-1,28*28)中-1表示不确定/不关心的数，只要求dim=0的size是28×28就行
        logits = fforward(data)
        
        loss = criteon(logits,target)
        
        optimizer.zero_grad()#所有参数上一轮的梯度信息都清除
        loss.backward()
        optimizer.step()#用梯度更新参数
        if( batch_idx %100 ==0):
            print("train epoch :{}, batch:{} ,loss{:.6f}".format(epoch,batch_idx,loss.item() ))
            #torch.item() 表示如果多维的torch中只包含一个元素，取出来
```

### 测试

```
test_loss = 0
correct =0
for data,target in test_loader :
	data = data.view(-1,28*28)
	logits = fforward(data)#得到预测值
	test_loss += criteon(logits,target).item()
	
	pre = logits.argmax(dim=1)
	correct = pred.eq(target.data)
	
```

## GPU加速

比较好的博客：

http://www.feiguyunai.com/index.php/2019/04/30/python-ml-25-pytorch-gpu/

[自己的实验环境应该是单GPU环境]

### 创建一个GPU实例

```python
device = torch.device('cuda: 0' if torch.cuda.is_available() else'cpu')#首先要先判断是否有可用的GPU
#'0'表示GPU的编号

```

### 把需要计算的量加载到设备上

```python
net = MLP().to(deivce) 
optimizer = optim.SGD(net.parameters() , lr = learning_rate)
criteon = nn.CrossEntropyLoss().to(device)#to(device) ,如果数据类型是model，则返回类型是 cpu上数据的引用

for epoch in range(epochs):
	for batch_idx , (data,target) in enumerate(train_loader):
		data = data.view(-1,28*28)
		data, target = data.to(device),target.cuda()#如果数据类型是 tensor，返回类型是cpu上数据的copy()
```

