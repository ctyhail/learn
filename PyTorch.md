[TOC]

# PyTorch

## 数据类型

python中数据类型有：

- Int
- Float
- Int array
- Float array
- string

pytorch中数据类型有：

- IntTensor of size()
- FloatTensor of size()
- IntTensor of size[d1,d2,...]
- FloatTensor of size[d1,d2,...]

![image-20240125203746383](C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240125203746383.png)

## type check

`a = torch.randn.(2,3)`。用`type`有两种使用方法：

![image-20240125204340778](C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240125204340778.png)

或者用`isinstance()`比较两个数据类型是否相同，相同则返回True，不同则返回False。

注意：Tensor在GPU和CPU上的部署是不一样的![image-20240125204729079](C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240125204729079.png)

## 维度变换

### expand/repeat

两种api最终效果完全一样，都是使tensor在某一个维度上进行复制，但本质操作不同。

`expand`不会复制数据，节省内存（推荐），`repeat`会实实在在地复制数据。

`expand`操作的维度必须是1。`expand`操作：

<img src="C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240130160455074.png" alt="image-20240130160455074" style="zoom:50%;" />

`repeat`操作：

<img src="C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240130160631242.png" alt="image-20240130160631242" style="zoom:50%;" />

### transpose

`transpose()`里面的两个参数是想交换的两个维度

### contiguous

使内存变得连续，维度变换时才不会污染数据

### permute

与`transpose`一样可以进行维度变换，但要这样使用：

<img src="C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240130162338332.png" alt="image-20240130162338332" style="zoom:50%;" />

### view/reshape

PyTorch中的`view`和`reshape`和Numpy中的`reshape`中的作用基本相同。

### unsqueeze

在目标维度插入一个新的维度

![image-20240126172620602](C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240126172620602.png)



## 索引

### index——select

```
a=torch.rand(4,3,28,28)
print(a.index_select(0,[0,2]))
```

`index_select`函数第一个参数为目标维度，第二个参数为目标维度上选取的目标索引

比如上述代码会打印出`a[0]`和`a[2]`，其size大小为[2,3,28,28]。

### ...

简单的说在`a=torch.rand(4,3,28,28)`中，`a[0]`和`a[0,...]`的作用是等效的。

### mask掩码索引

![image-20240126170546225](C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240126170546225.png)

### take

`torch.take`函数会先将原Tensor打平在去索引，例如：

```
a = torch.tensor([[4,3,5],[6,7,8]])
b = torch.take(a,torch.tensor([0,2,5]))
```

打印b的值为4,5,8。

## 合并与分割

### cat

在某个维度下合并tensor

### stack

会创建一个新的维度

### split

在某个维度上拆分tensor

### chunk

`chunk`的第一个参数是每一块有多少个，第二个参数是在什么维度。

## 数学运算

### add(+)

### sub(-)

### mul(*)

### div(/)

### *

矩阵每个相应的位置相乘。

### mm(只使用于二维的tensor)      @

矩阵乘法，只用numpy里的`@`也可以。

### matmul(多维)

矩阵乘法，但可以用于多维。但只是后两维进行矩阵相乘，前面的维度不变。

### **和pow

平方。

### sqrt

开方。

### rsqrt

倒数。

### exp

`e^x`函数。

### log

`log`函数。

### 近似处理

#### floor

向下取整。

#### ceil

向上取整。

#### round

对数字进行四舍五入处理。

#### trunc

去掉一个数字的小数部分。

#### frac

去掉一个数字的整数部分。

### clamp

对梯度进行裁剪。用法为：

<img src="C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240228223048746.png" alt="image-20240228223048746" style="zoom:50%;" />

只有一个参数是(min),有两个参数是(min,max)。小于`min`的数将变为`min`的数，大于`max`的数变为`max`的数。

### prod

将tensor里的值进行垒乘。

### argmin和argmax

求tensor中最小值和最大值的位置，如果不给定维度参数，将会将tensor强制打平再索引最大值。

### topk

其用法为：

```
a.topk(个数,dim,largest=False/True)

```

![image-20240229220042460](C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240229220042460.png)

### kthvalue

```
kthvalue(num,dim)
```

找第几小的，并返回其位置

### eq

比较两个tensor是否相等。如果相等，返回一个相同大小tensor，但都为一。

### equal

与`eq`相似，但返回True。

## 高阶操作

### where

<img src="C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240229224505880.png" alt="image-20240229224505880" style="zoom: 33%;" />

`cond`中对应大于0.5，就选`a`中对应元素，否则就选`b`。

## 感知机

单层感知机的梯度推导（其中激活函数为sigmod函数）

<img src="C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240310171751056.png" alt="image-20240310171751056" style="zoom: 25%;" />

多层感知机的梯度推导（其中激活函数为sigmod函数）

<img src="C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240310180302396.png" alt="image-20240310180302396" style="zoom:25%;" />

如果中间隐藏层不止一层，可以通过上述的推导一步一步得将梯度反向传播。

## 不常见的激活函数

### Leaky ReLU

<img src="C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240310211307514.png" alt="image-20240310211307514" style="zoom: 25%;" />

其中`a`的值通常很小，一般设定值为0.02，为了避免`ReLu`函数在x<0时梯度为0导致网络中的参数不变化。

### SELU

<img src="C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240310212047303.png" alt="image-20240310212047303" style="zoom:50%;" />

其图像为：

<img src="C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240310211833065.png" alt="image-20240310211833065" style="zoom: 50%;" />

使得当x<0时能有更光滑的曲线。、

### Softpuls

<img src="C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240310212500200.png" alt="image-20240310212500200" style="zoom:50%;" />

相比与`ReLu`激活函数，当x=0时有更光滑的处理。

## cuda加速

在新版`pytorch`中可以用如图所示的方法使用GPU加速或CPU加速，能够合理地分配算力

![image-20240311195916771](C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240311195916771.png)

先创建一个设备`device`，需要用此设备的算力可以在后面添加`.to(device)`。其中创建`device`时中`cuda:0`表示0号GPU，如果有多块GPU后面的数字以次增加。
