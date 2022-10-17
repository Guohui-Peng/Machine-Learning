# 损失函数  

参考：<https://gombru.github.io/2018/05/23/cross_entropy_loss/>  

## 分类问题  

### 多分类问题（Multi-Class Classification）  

多分类问题是指每个样本属于几个类中的一种。如：猫、狗、牛等，只能属于其中一种。

### 多标签问题（Multi-Label Classification）  

多标签问题是指样本中含有待分类中的一种或几种。如：一个图片即有猫又有狗，并且要在一次分类中都识别出来，就属于多标签问题。  

下图显示了这两种分类的差别：

![图1](../images/multiclass_multilabel.png)

## 激活函数  

这里讲到激活函数，是因为对于多分类与多标签需要乃至不同的激活函数来处理。  

### Sigmoid  

Sigmoid可以将数据转换为0~1之间的值。通过将FC输出的每个数据都转换为0到1之间的数据，转换后的各个值之间无必然联系。通过将转换后的数据与one-hot后的值进行比较，就可以进行多标签的分类损失计算。Sigmoid计算公式如下：

$$  
\Large f(s_i) = \frac{1}{1+e^{-s_i}}
$$  

它的函数图形如下：

![图2](../images/sigmoid.png)  

### Softmax  

Softmax也是将数据转换为0~1的数据，不同的是softmax转换FC输出后的所有数据和为1。因此，一般取概率最大的值作为最终的预测输出。一般用于多分类损失。它的计算公式如下：  

$$\Large f(s_i) = \frac{e^{s_i}}{\sum_{j}^{C}e^{s_j} }$$  

C为分类数量

## 交叉熵损失（Corss-Entropy loss）  

一般用于计算分类问题的损失。公式如下：  

$$CE=-\sum_{i}^{C}t_{i}log(s_i)$$  

$t_i$ 和 $s_i$ 分别为真实和预测值在C类中出现的概率。通常在计算前使用sigmoid或softmax函数将数据转换为0~1之间的数，因为one-hot后的标签正类为1，负类为0，所以可以将sigmoid或softmax函数处理过的数据近似的看为其在C类中的概率。  

对于二分类问题，就是 $C=2$ 时，交叉熵损失函数可以被定义为：  

$$CE=$$  

## 均方差（MSE）损失  

均方差损失一般用来计算线性回归的损失，它通过计算预测值与真实值的距离的平方来衡量预估与真实之间的差距。  

$$ MSE = \frac{1}{2m} \left ( \hat{y} - y \right )^2$$  
