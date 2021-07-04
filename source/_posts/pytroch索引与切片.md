---
title: pytroch索引与切片
date: 2020-08-24 07:33:55
tags: [pytorch, 大创]
categories: [深度学习,pytorch]
---

### 1.pytorch风格的索引－维度选择　　　　　　　　　　　　　　　　　　　　

跟据Tensor的shape,从前往后索引，依次在每个维度上进行索引



```python
import torch

a = torch.rand([4,3,28,28])
print( a[0].shape)#和Ｃ＋＋等高级问题类似，a[0]表示选择第一章图片
print(a[0,0].shape)#选择第一章图片的第一个通道
print(a[0,0,2,4])#选择某个像素点，

'''
torch.Size([3, 28, 28])
torch.Size([28, 28])
tensor(0.1076) #是维度为０的元素
'''
```



### 2.python风格的索引-特定维度上

```python
'''
基本结构：
eg : a[0,0,0:28:2,0]
#0:28:2－－在某个维度上，start: end[:offset]  其中start:end:offset 表示从start开始，到end结束，其中不包括end，每次步长为offset;当省略offset时，表示offset=1
# 0:28其实等价与 0:28:1这种结构

-1:表示倒数第１个元素，-2表示倒数第２个元素
'''

import torch
 
# 譬如：4张图片，每张三个通道，每个通道28行28列的像素
a = torch.rand(4, 3, 28, 28)
 
# 在第一个维度上取后0和1，等同于取第一、第二张图片
print(a[:2].shape)  
 
# 在第一个维度上取0和1,在第二个维度上取0，
# 等同于取第一、第二张图片中的第一个通道
print(a[:2, :1, :, :].shape)  
 
# 在第一个维度上取0和1,在第二个维度上取1,2，
# 等同于取第一、第二张图片中的第二个通道与第三个通道
print(a[:2, 1:, :, :].shape) 
 
# 在第一个维度上取0和1,在第二个维度上取1,2，
# 等同于取第一、第二张图片中的第二个通道与第三个通道
print(a[:2, -2:, :, :].shape)  
 
# 使用step隔行采样
# 在第一、第二维度取所有元素，在第三、第四维度步长为２采样
# 等同于所有图片所有通道的行列每个一行或者一列采样
# 注意：下面的代码不包括28
print(a[:, :, 0:28:2, 0:28:2].shape) 
print(a[:, :, ::2, ::2].shape)  # 等同于上面语句
```



### 3. 选择特定的元素

#### 3.1 index_select

```python
'''
torch.index_select(input, dim, index, out=None) → Tensor

input (Tensor) – the input tensor.
dim (int) – the dimension in which we index
index (LongTensor) – the 1-D tensor containing the indices to index；是list转换成的一维tensor
out (Tensor, optional) – the output tensor.
'''

a = torch.rand(4,3,28,28)
print(a.shape)
print((a.index_select(0,torch.tensor([0,2]))).shape)
#选取第一个维度上的　索引为0,2的tensor
#第二个参数是将list [0,2]转换成 tensor
print((a.index_select(2,torch.arange(8))).shape)

#选择第二维度上的前８个元素

'''
torch.Size([4, 3, 28, 28])
torch.Size([2, 3, 28, 28])
torch.Size([4, 3, 8, 28])
'''
```

索引效果

```python
a = torch.rand(3,3)
print(a)
print((a.index_select(0,torch.tensor([0,2]))))
print((a.index_select(1,torch.arange(2))))

'''
tensor([[0.9071, 0.5350, 0.4018],
        [0.9565, 0.9739, 0.7234],
        [0.1984, 0.3562, 0.8078]])
        
tensor([[0.9071, 0.5350, 0.4018],
        [0.1984, 0.3562, 0.8078]])
        
tensor([[0.9071, 0.5350],
        [0.9565, 0.9739],
        [0.1984, 0.3562]])
'''
```



#### 3.2 masked_select()

###### 方式一

```python
'''

函数说明
torch.masked_select( input, mask ,out = None ) -> 张量


根据掩码张量mask中的二元值(0,1)，取输入张量中的指定项( mask为一个 ByteTensor)，将取值返回到一个新的1D张量－－－是打平的张量; 张量 mask须跟input张量有相同数量的元素数目，但形状或维度不需要相同。
注意： 返回的张量不与原始张量共享内存空间。
'''
a = torch.randn(3,4)
print(a)
mask2 = a.ge(0.3)
mask3 = a.le(0.7)
print(mask2)
print(a[mask2])
print(a[mask3])
print(torch.masked_select(a,mask2))

'''
output:
tensor([[-0.7769, -0.0803, -0.4235, -0.3562],
        [-0.4744,  1.2078,  0.6371, -0.6981],
        [-1.1653, -0.3432, -2.3189,  0.1708]])
         
tensor([[ True, False,  True,  True],
        [False, False, False, False],
        [ True, False, False, False]])
        
tensor([1.2078, 0.6371])

tensor([-0.7769, -0.0803, -0.4235, -0.3562, -0.4744,  0.6371, -0.6981, -1.1653,
        -0.3432, -2.3189,  0.1708])
        
tensor([1.2078, 0.6371])

'''
```

###### 方式二　不使用a.ge() \ a.le()方法

```python

a = torch.randn(3,4)
print(a)
mask1 = torch.ByteTensor((a>0.5).byte())　
print(mask1)

print(a[mask1])

'''
输出：
tensor([[-1.8973, -0.2158,  1.0196, -0.2119],
        [-0.2365,  0.4743, -1.2473, -0.6554],
        [ 0.3040, -0.0906, -0.8517, -0.2679]])
tensor([[0, 0, 1, 0],
        [0, 0, 0, 0],
        [0, 0, 0, 0]], dtype=torch.uint8)
tensor([1.0196])


提醒一下：不是特别建议使用这种方式
 UserWarning: indexing with dtype torch.uint8 is now deprecated, please use a dtype torch.bool instead.
 
 感觉自己对python和pytorch基本类型的转换有些模糊　－－－torch.bool　与　torch.uint8
'''
```



#### 3.2 torch.take()

```python
'''
使用打平的index进行索引
torch.take(input, index) ->Tensor

index(Long Tensor) 把input Tensor看作一维Tensor对每个元素的索引
输出:一个一维Tensor
'''
a = torch.randn(3,4)
print(a)
b = torch.take(a, torch.tensor([1,5,7]))
print(b)

'''
输出：
tensor([[-0.3353,  1.2220,  0.7055,  0.4678],
        [-0.4759,  0.5173,  1.1912, -0.8545],
        [-1.2037,  0.5052,  0.0388, -0.3160]])
        
tensor([ 1.2220,  0.5173, -0.8545])

'''

```

