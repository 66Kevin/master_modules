求导，矩阵计算：https://www.symbolab.com/solver/derivative-calculator

科学计算器：https://www.geogebra.org/scientific?lang=zh_CN

---

## Week1 Introduction

Confusion Matrix and Metric

## Week2 Discriminant Functions

### Discriminant Functions

### Linear Discriminant Functions

**Dichotimizer**:

$g(x) = g_1(x) - g_2(x) = (w_1^tx + w_{10}) - (w_2^tx+w_{20}) = (w_1-w_2)^tx +(w_{10}-w_{20}) = w^tx + w_0$

$g(x) = w^tx + w_0$

- $g(x)>0$属于class1，$g(x)<= 0$属于class2
- $g(x)$ provides a measures of how far x is from the decision boundary. The actual distance: $\frac{\abs{g(x)}}{||w||}$

**Augmented Vectors**:

$g(x) = a^ty$ Where: $a = [w_0w^t]t$ and $y=[1 x^t]^t$

**Sample Normalisation**：

- 如果用了normalisation，预测结果为负则预测错误；预测结果为正，预测正确
- 如果有两个class，1和2，class2的sample的y的第一个要变成-1，不是-2

### Perceptron Learning

#### **Batch Learning**

- 全部样本进行了一次完整的学习，即一个epoch后才对网络的参数进行更新
- if y is misclassified, then $a \larr a + \eta* \sum \omega' y$（其中$\omega$为class）

#### **Sequential Learning**

- 每输入一个样本就进行一次迭代
- if $y_k$ is misclassified, then $a \larr a + \eta* \omega'_k y_k$（其中$\omega$为class）
- 如果$g(x) = a^t y < 0$，说明**分类错误，则需要更新梯度**，否则无需更新梯度。

注意⚠️：Batch和Sequential Learning都是因为用了augmented vector梯度下降变为了➕，因为只更新错误的分类的sample，求导之后就变成了➕

- Sequential Multiclass Learning
  - 当预测正确时$j = \omega_k$，不更新权重
  - 当预测不正确时$j \neq \omega_k$
    - true class更新策略：$a_{w_k} \larr a_{w_k} + \eta y_k$
    - 预测class j的更新策略：$a_j \larr a_j - \eta y_k$

### **MSE最小二乘**：

**MSE via Pseudo-inverse：**

通过伪逆矩阵的方法进行求解

**Widrow-Hoff Rule**：

通过 Widrow-Hoff Learning Algorithm 对向量a进行求解

使用**MSE**作为WH Rule的损失函数，所以

- WH下的batch learning：$a \larr a - \eta* Y^t(Ya - b)$
- WH下的Sequential learning：$a \larr a - \eta*(a^ty_k - b_k)y_k$或者$a \larr a + \eta*(b_k - a^ty_k)y_k$

### 总结：

感知器学习：

1. 只针对于misclassified的样本学习参数

MSE与感知机相比：

1.本质上解决的是线性等式 $a^ty_k = b_k $的问题

2.即使样本线性不可分，也会给出结果

3.对于所有的样本进行学习

4.使用伪逆矩阵的方式来进行计算

## Week3 Intro to NN

### Heaviside Funciton

H(0) = 0.5

### **Linear Threshold Unit**

$y = H (\sum_i w_ix_i - \theta)$

线性阈值神经网路与其他神经网络的差异在于

- activation function 是 heaviside step function
- 在进行heaviside step function 计算之前需要先用netj - θ

**写成Augmented形式则为，$y = H(wx)$，注意⚠️，此时的$w_0 = -\theta$**

![img](https://pic4.zhimg.com/80/v2-85f6deb583c7bf1658efd33b050a9fff_1440w.jpg)

#### Delta Learning

注意⚠️，Augmented形式的$w_0 = -\theta$

**Sequential**:

- $w \larr w + \eta(t-y)x^t$

**Batch** :

- $w \larr w + \eta\sum_p (t_p - H(wx_k))x_p^t$，label - pred，**计算所有sample，没有misclassified的sample也要计算梯度**

### Competitive Learning Networks

competition是指某些神经元被其他的压制

competition如何实现：

- **Winner-takes-all**: 通过Softmax，选取得分最大的那个神经元为winner，其他都设为0
- **inhibitory lateral weights**：输出神经元进行竞争谁可以接受input，而不是向上面的竞争谁可以输出；即negative feedback networks

### Negative Feedback Networks

- Activation
  - 初始化y为0
  - iterations
    - update input units: $e = x - W^T y$
    - Update output units: $y \larr y + \alpha We$
- Learning
  - $W \larr W + \beta y e^T$



## Week2+3 总结

- **widrow-Hoff和delta learning都是计算each sample来更新参数**；**只有perceptron learning才只用missclassified的sample来更新参数**

- **Widrow-Hoff必须要sample normalization，delta learning无需sample normalization**
- **delta learning里的theta要变成相反数，所以算出来的结果也要取相反数**
- 公式总结：
  - Perceptron learning：只missclassified的sample参与更新
    - Sequential：$a \larr a + \eta* \omega'_k y_k$
    - Batch：$a \larr a + \eta* \sum \omega' y$
    - Multiclass Sequential：
      - 当预测正确时$j = \omega_k$，不更新权重
      - 当预测不正确时$j \neq \omega_k$
        - true class更新策略：$a_{w_k} \larr a_{w_k} + \eta y_k$
        - 预测class j的更新策略：$a_j \larr a_j - \eta y_k$
  - MSE：所有sample参与更新
    - Psedo-Inverse：
    - Widrow-Hoff：
      - Batch：$a \larr a - \eta* Y^t(Ya - b)$
      - Sequential：$a \larr a + \eta*(b_k - a^ty_k)y_k$
  - Delta Learning：所有sample参与更新
    - Sequential：$w \larr w + \eta(t-y)x^t$
    - Batch：$w \larr w + \eta\sum_p (t_p - H(wx_k))x_p^t$
  - Negative Feedback NN：$e = x - W^T y$ /  $y \larr y + \alpha We$

## Week4 Multilayer Perceptrons and Backpropagation

XOR问题：相同为0，不同为1

### Radial Basis Function NN

径向基函数：空间任意点X到中心c之间的欧式距离函数，我们可以看作`d = (x-c)`

- 只有在输出层才有bias
- 注意计算：$c_1 = (x_1, x_2), c_2 = (x_3,x_4)$
  - $||c_1 - c_2|| = \sqrt{(x_1-x_3)^2+(x_2-x_4)^2}$
  - $||c_1 - c_2||^2 = (x_1-x_3)^2+(x_2-x_4)^2$

## Week5 Deep Discriminative Neural Networks (Spratling)

- ReLu
- LReLu
  - a是固定的，且所有neuron都相等
- PReLu
  - a是学习的，每个neuron都要学习

### Batch Norm

### CNN

**Conv按照correlation算**

- Dilation:
  - dilation = 2：两行pixel间隔一行为0；两列pixel间隔一列为0
- Size Computation
  - output Height & Width:$1+\frac{input Dim - mask Dim + 2*padding}{stride}$

## Week6 GAN

https://www.bilibili.com/video/BV1uR4y1K74x?p=15

编码器AE：编码出每张图片在z空间里的一个点

变分编码器VAE：编码出的z空间是个分布

#### GAN

**minmax方式训练：**

- 判别器($\theta_d$)希望**最大化目标函数**使得D(x)接近于1（真实样本），而D(G(z))解决于0（生成样本）
- 生成器($\theta_g$)希望**最小化目标函数**使得D(G(z))尽量接近1，即希望判别器认为生成器生成的图片G(z)为真实图片

**如何训练：**

1. **梯度提升**训练判别器($\theta_d$) ->因为希望越大越好

2. **梯度下降**训练生成器($\theta_g$) -> 越小越好

**问题：**

- 在训练初期，生成器生成的样本与真实样本差距很大，判别器很容易就把生成的样本判断为0，$log(1-D_{\theta_d}(G_{\theta_g(z)}))$变化很小，所以梯度会非常的小，因此训练生成器($\theta_g$)的过程会很慢；相反，当生成样本较好时，判别器输出值都会很大，生成器损失函数梯度大，更新较大，容易过拟合。

## Week7 Feature Engineering

#### PCA for NN

Unsupervised Learning 

- Karhunen-Loeve Transforme
  - x减去同一行与其他点算出的平均值，不能减去自身算出的平均值
  - $X = [x1, x2 ,x3], x1 = [1, 2, 3]^T, x2 = [1, 2, 3]^T, x3 = [1, 2, 3]^T$,x1,x2,x3都为列向量。计算平均值时要计算一行的均值，再每一行减去该均值

- Hebbian Learning

- Oja's Leaning

  $w \larr w + \mu\sum_iy(x_i^t - y w)$, where y = wx

- Sanger's Rule



#### Whitening白化

#### LDA

Supervised Learning

LDA seeks a projection that is maximally discriminative

类内距离最小，类间距离最大 -> 需要判断不同的类所以需要label因此是Supervised Learning

- Fisher's LDA
  - Maximise the cost function $J(w) = \frac{sb}{sw}$

#### ICA 独立成分分析

- Oja's subspace rule

  $\Delta W = \mu g(y) (x - W^tg(y))^t$, where g is a nonlinear function

#### Random Projections

PCA, LDA和LCA都是降维的，RP是把data按照随机directions映射到高纬可分空间

- Extreme Learning Machine 极限学习机

  单隐含层前馈NN

#### Sparse Coding

https://blog.csdn.net/walilk/article/details/78175912

跟RP相同，但不是随机找directions映射

稀疏编码算法的目的就是找到一组基向量Φϕ，使得我们能将输入向量 x 表示为这些基向量的线性组合

不断迭代，直至收敛，这就可以得到一组能够良好表示训练样本x的基向量，也就是**字典**

## Week8 SVM

### Linear SVMs

只有当$x_i$对应的$\lambda_i\neq0$时，才是支持向量

- $w = \sum_{i=1}^N \lambda_i y_i x_i$
- $\sum_{i=1}^N\lambda_iy_i = 0$

Hard Classifier:$f(x) = sgn(w^Tx + w_0)$

### Soft SVMs

- $w = \sum_{i=1}^N \lambda_i y_i x_i$
- $\sum_{i=1}^N\lambda_iy_i = 0$
- $C - \mu_i - \lambda_i = 0$

Soft Classifier: $f(x) = h(w^Tx + w_0)$

求解：

1. assign class label and identify support vectors
2. represent $w$ using $w = \sum_{i=1}^N \lambda_i y_i x_i$
3. Construct linear equations using $y_i(w^Tx + w_0) = 1$ and $\sum_{i=1}^N\lambda_iy_i = 0$
4. 求解$\lambda$ 和$w_0$
5. 用$\lambda$ 来Construct w using $w = \sum_{i=1}^N \lambda_i y_i x_i$
6. construct hyperplan

## Week9 Ensemble Methods

**Bagging:**

算法过程如下：

1. 从原始样本集中抽取训练集。每轮从原始样本集中使用Bootstraping的方法抽取n个训练样本（在训练集中，有些样本可能被多次抽取到，而有些样本可能一次都没有被抽中，有放回抽取）。共进行k轮抽取，得到k个训练集。
2. 每次使用一个训练集得到一个模型，k个训练集共得到k个模型。（注：这里并没有具体的分类算法或回归方法，我们可以根据具体问题采用不同的分类或回归方法，如决策树、感知器等）
3. 对分类问题：将上步得到的k个模型采用投票的方式得到分类结果；对回归问题，计算上述模型的均值作为最后的结果。（所有模型的重要性相同）

**Boosting：**

   AdaBoosting方式每次使用的是全部的样本，每轮训练改变样本的权重。下一轮训练的目标是找到一个函数f 来拟合上一轮的残差。当残差足够小或者达到设置的最大迭代次数则停止。Boosting会减小在上一轮训练正确的样本的权重，增大错误样本的权重。（对的残差小，错的残差大）

   梯度提升的Boosting方式是使用代价函数对上一轮训练出的模型函数f的偏导来拟合残差。

**Stacking：**

![817dbd5bb8803383f3a9033a2ad0c9b5.png](https://img-blog.csdnimg.cn/img_convert/817dbd5bb8803383f3a9033a2ad0c9b5.png)

将训练集分成5份，共计迭代5次，每次迭代都将4份数据作为Train Set对每个Base Model进行训练，然后剩下的一份作为Hold-out Set（可以类比cv中的测试集）进行预测。同时，每个Base Model在Test Set的预测值也要保存下来。经过5-Flod迭代后，我们获得了一个：

#### AdaBoost

#### Random Forest







## Week10 Clustering

- Agglomerative approach：自底向上
- Divisive approach：自上而下

### K-Means

### Fuzzy K-Means

主要思想：每个点可能输入多个clustering

$\mu_{ij}$表示$x_j$多大程度属于第i个clustering（权重）

### Interative Optimisation

### Agglomerative Hierarchical

- Single-link clustering (min)
  - 如何一个cluster中有超过1个的sample，就取该cluster与其他点的最小值最为距离
- Complete-link clustering (max)
- Group-average clustering
- Centroid clustering

### Competitive Learning

- Without Input and Weight Normalisation
  - euclidean距离选取最小的
- With Input and Weight Normalisation
  - cosine相似度选取最大的
  - 需要Input Augmentation

### Clustering for Unknown Num of Clusters

设置阈值，如果距离小于阈值就当成新的cluster

- Without Input and Weight Normalisation
  - Basic Leader-follower clustering 
- With Input and Weight Normalisation
  - Basic Leader-follower clustering 