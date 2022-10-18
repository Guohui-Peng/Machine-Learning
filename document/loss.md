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
\large f(s_i) = \frac{1}{1+e^{-s_i}}
$$  

它的函数图形如下：

![图2](../images/sigmoid.png)  

### Softmax  

Softmax也是将数据转换为0~1的数据，不同的是softmax转换FC输出后的所有数据和为1。因此，一般取概率最大的值作为最终的预测输出。一般用于多分类损失。它的计算公式如下：  

$$\large f(s_i) = \frac{e^{s_i}}{\sum_{j=1}^{C}e^{s_j} }$$  

C为分类数量

## 交叉熵损失（Corss-Entropy loss）  

一般用于计算分类问题的损失。公式如下：  

$$\large CE=-\sum_{i=1}^{C}t_{i}log(s_i)$$  

$t_i$ 和 $s_i$ 分别为真实和预测值在C类中出现的概率。通常在计算前使用sigmoid或softmax函数将数据转换为0~1之间的数，因为one-hot后的标签正类为1，负类为0，所以可以将sigmoid或softmax函数处理过的数据近似的看为其在C类中的概率。  

对于二分类问题，就是 $C=2$ 时，交叉熵损失函数可以被定义为：  

$$\large CE=-\sum_{i=1}^{C=2}t_{i}log(s_i)=-t_1log(s_1)-(1-t_1)log(1-s_1)$$  

### 分类交叉熵损失（Categorical Cross-Entropy loss）  

也叫做softmax损失。它是softmax激活层后加上一个交叉熵损失。通过softmax将模型的输出结果输出为和为1的小数，可以似作计算出的各类可能性的概率。分类交叉熵经常用于多类问题的分类。

真实结果通常转换为one-hot标签，所以对每一个样本来说，标签只有正类 $C_p$ 是1，其它均为0。由于其它的类均为0，也就是除了 $t_i=t_p$ 外的其它 $t_i = 0$ 。公式可变化为：

$$\large CE = - log \left ( \frac{e^{s_p}}{\sum_{j=1}^{C}e^{s_j}} \right ) $$  

其中 $s_p$ 为模型输出的正类的softmax后的值。  

### 二元交叉熵损失（Binary Cross-Entropy Loss）  

也叫sigmoid损失。它是在sigmoid激活层上加一个交叉熵损失组成。与softmax损失不同在于，它对每个向量的分量是独立的，没有关联性。因此可以让多个网络输出的分类值都为1，从而对应多标签的多个值为1的情形。由于它每个类的元素不受其它类的影响，就如对每个类进行二分类操作，所以被称为二元交叉熵损失。

$$\large CE=-\sum_{i=1}^{C=2}t_{i}log(s_i)=-t_1log(s_1)-(1-t_1)log(1-s_1)$$  

也可表示为：

$$\large CE =  
    \begin{cases}  
        -log(f(s_1)) & if \ t_1 =1 \\  
        -log(1-f(s_1)) & if \ t_1 = 0  
    \end{cases}  
$$  

在此 $t_1 = 1$ 意味着 $C_1$ 为正类。因此对于 $s$ 中的 $s_i$ 的梯度计算仅与其对应的二元交叉熵损失有关。  

对于其 $s_1$ 的项的偏导为：

$$\large \frac{\partial}{\partial{s_1}}(CE(f(s_i))) = t_1(f(s_1)-1) + (1-t_1)f(s_1) $$  

也可表示为：

$$ \large \frac{\partial}{\partial{s_1}}(CE(f(s_i))) =  
\begin{cases}  
        f(s_1)-1 & if \ t_1 =1 \\  
        f(s_1) & if \ t_1 = 0  
    \end{cases}  
$$  

$f()$ 代表sigmoid函数。注：如果在网络的输出时未加sigmoid函数，则需在计算损失时选择带logit功能的损失函数，如pytorch中选择`BCEWithLogitsLoss`函数，而tensorflow则需在`tf.keras.losses.BinaryCrossentropy`函数的参数中设置`from_logits=true`。

## 均方差（MSE）损失  

均方差损失一般用来计算线性回归的损失，它通过计算预测值与真实值的距离的平方来衡量预估与真实之间的差距。  

$$ MSE = \frac{1}{2m} \left ( \hat{y} - y \right )^2$$  
