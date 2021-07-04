---
title: pytorch中的网络结构
date: 2020-09-03 17:32:45
tags: [大创, pytorch]
categories: [深度学习, pytorch]
---

==**首先先给出一些python面向对象的一些基础知识**==

[子类中初始化父类](https://blog.csdn.net/brucewong0516/article/details/79121179)的相关问题

[pytorch中网络构造的深入理解](https://zhuanlan.zhihu.com/p/53927068)

[官方中文文档](https://pytorch-cn.readthedocs.io/zh/latest/package_references/torch-nn/)



# pytorch.nn.module



## current layer

[api:](https://pytorch.org/docs/stable/nn.html)

## Container

### [torch.nn.Sequential](https://pytorch.org/docs/stable/generated/torch.nn.Sequential.html#torch.nn.Sequential)

A sequential container.

Modules will be added to it in the order they are passed in the constructor. Alternatively, an ordered dict of modules can also be passed in.

```python
# Example of using Sequential
model = nn.Sequential(
          nn.Conv2d(1,20,5),
          nn.ReLU(),
          nn.Conv2d(20,64,5),
          nn.ReLU()
        )
```

### parameters

module 保存所有需要计算的参数

**自己在设计网络时，要通过`nn.Parameter()`加入module的优化器管理**

```python
class MyLiner(nn.Module):
    
    def __int__(self, inp, outp):
        super(MyLinear , self).__init__()
        
        #
        self.w = nn.Parameter(torch.randn(outp,inp))#这时生成参数不需要 requires_grad =True
        self.b = nn.Parameter(torch.randn(outp))
        
      def forward(self,x ):
        x = x@self.w.t() +self.b
        return x
	
```

### modules

```
modules

children

```

### GPU

```
device = torch.device('cuda')
net = Net()
net.to(device)
```

### save and load

#### save

```
cn = MyNet()
......
torch.save(cn.state_dict(), "your_model_path.pth")
```

#### load

```python
cn = MyNet()

state_dict = torch.load("your_model_path.pth")

#加载训练好的参数
cn.load_state_dict(state_dict)
```



### train/test

```python
cn = cn.eval()#转换成测试状态
cn = cn.train()#转换成 训练状态
```

了解基本知识之后，以下从构建自己网络的思路开始讲述



# mynet

https://blog.csdn.net/qq_27825451/article/details/90550890

https://blog.csdn.net/qq_27825451/article/details/90705328

**貌似是需要自己实验forward过程**

# 常用的网络模块



## Flatten

```python
class Flatten(nn.Module ):
    
    def __init__(self):
        	super(Flatten, self ).__init__()
     
    def forward(self, input):
        return input.view(input.size(0),-1) #将数据打平作为卷积层和全连接层之间的过渡
```

