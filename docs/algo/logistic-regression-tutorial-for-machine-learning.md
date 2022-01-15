# 机器学习中的逻辑回归教程

> 原文： [https://machinelearningmastery.com/logistic-regression-tutorial-for-machine-learning/](https://machinelearningmastery.com/logistic-regression-tutorial-for-machine-learning/)

逻辑回归是用于二分类的最流行的机器学习算法之一。这是因为它是一种简单的算法，可以很好地解决各种各样的问题。

在这篇文章中，您将逐步发现二分类的逻辑回归算法。阅读这篇文章后你会知道：

*   如何计算物流函数。
*   如何使用随机梯度下降来学习逻辑回归模型的系数。
*   如何使用逻辑回归模型做出预测。

这篇文章是为开发人员编写的，不承担统计或概率的背景。打开电子表格并按照说明进行操作。如果您对逻辑回归有任何疑问，请在评论中提出，我会尽力回答。

让我们开始吧。

**2016 年 11 月更新**：修正了 b0 更新方程中的一个小错字。

![Logistic Regression Tutorial for Machine Learning](img/703ec066a22749619c753bfcc77f4213.jpg)

机器学习的逻辑回归教程
照片由 [Brian Gratwicke](https://www.flickr.com/photos/briangratwicke/6118345858) 拍摄，保留一些权利。

## 教程数据集

在本教程中，我们将使用一个人为的数据集。

该数据集有两个输入变量（X1 和 X2）和一个输出变量（Y）。输入变量是从高斯分布中提取的实值随机数。输出变量有两个值，使问题成为二分类问题。

原始数据如下所示。

```py
X1		X2		Y
2.7810836	2.550537003	0
1.465489372	2.362125076	0
3.396561688	4.400293529	0
1.38807019	1.850220317	0
3.06407232	3.005305973	0
7.627531214	2.759262235	1
5.332441248	2.088626775	1
6.922596716	1.77106367	1
8.675418651	-0.2420686549	1
7.673756466	3.508563011	1
```

下面是数据集的图表。您可以看到它完全是人为的，我们可以轻松地绘制一条线来分隔类。

这正是我们要采用逻辑回归模型的方法。

![Logistic Regression Tutorial Dataset](img/1b9a5032b58df3ac16965ac5efe3582d.jpg)

逻辑回归教程数据集

## 物流功能

在我们深入研究逻辑回归之前，让我们来看一下逻辑函数，它是逻辑回归技术的核心。

逻辑函数定义为：

变换= 1 /（1 + e ^ -x）

其中 e 是数值常数 Euler 的数字，x 是输入，我们插入函数。

让我们插入从-5 到+5 的一系列数字，看看逻辑函数如何转换它们：

```py
X	Transformed
-5	0.006692850924
-4	0.01798620996
-3	0.04742587318
-2	0.119202922
-1	0.2689414214
0	0.5
1	0.7310585786
2	0.880797078
3	0.9525741268
4	0.98201379
5	0.9933071491
```

您可以看到所有输入都已转换为范围[0,1]，并且最小的负数导致值接近零，而较大的正数导致值接近 1。您还可以看到 0 转换为 0.5 或新范围的中点。

由此我们可以看出，只要我们的平均值为零，我们就可以将正值和负值插入到函数中，并始终对新范围进行一致的转换。

![Logistic Function](img/b27eff2d941d0d8e50b42686be5aaca9.jpg)

物流功能

## 获取免费算法思维导图

![Machine Learning Algorithms Mind Map](img/2ce1275c2a1cac30a9f4eea6edd42d61.jpg)

方便的机器学习算法思维导图的样本。

我已经创建了一个由类型组织的 60 多种算法的方便思维导图。

下载，打印并使用它。

## 逻辑回归模型

逻辑回归模型采用实值输入并对输入属于默认类（类 0）的概率做出预测。

如果概率&gt; 0.5 我们可以将输出作为默认类（类 0）的预测，否则预测是针对另一个类（类 1）。

对于此数据集，逻辑回归有三个系数，就像线性回归一样，例如：

output = b0 + b1 * x1 + b2 * x2

学习算法的工作是基于训练数据发现系数（b0，b1 和 b2）的最佳值。

与线性回归不同，使用逻辑函数将输出转换为概率：

p（class = 0）= 1 /（1 + e ^（ - 输出））

在您的电子表格中，这将写为：

p（class = 0）= 1 /（1 + EXP（-output））

## 随机梯度下降的逻辑回归

我们可以使用随机梯度下降来估计系数的值。

这是一个简单的过程，可以被机器学习中的许多算法使用。它通过使用模型来计算训练集中每个实例的预测并计算每个预测的误差。

我们可以将随机梯度下降应用于寻找逻辑回归模型系数的问题如下：

给出每个训练实例：

1.  使用系数的当前值计算预测。
2.  根据预测误差计算新系数值。

重复该过程，直到模型足够准确（例如，误差下降到某个期望的水平）或者固定次数的迭代。您继续更新训练实例的模型并更正错误，直到模型足够准确或者无法使其更准确。将示出的训练实例的顺序随机化到模型中以混合所做的校正通常是个好主意。

通过更新每种训练模式的模型，我们称之为在线学习。还可以在所有训练实例上收集模型的所有更改，并进行一次大型更新。这种变化称为批量学习，如果您喜欢冒险，可能会对本教程做出很好的扩展。

### 计算预测

让我们从为每个系数分配 0.0 并计算属于 0 级的第一个训练实例的概率开始。

B0 = 0.0

B1 = 0.0

B2 = 0.0

第一个训练实例是：x1 = 2.7810836，x2 = 2.550537003，Y = 0

使用上面的等式，我们可以插入所有这些数字并计算预测：

预测= 1 /（1 + e ^（ - （b0 + b1 * x1 + b2 * x2）））

预测= 1 /（1 + e ^（ - （0.0 + 0.0 * 2.7810836 + 0.0 * 2.550537003）））

预测= 0.5

### 计算新系数

我们可以使用简单的更新方程计算新的系数值。

b = b + alpha *（y - 预测）*预测*（1 - 预测）* x

其中 b 是我们正在更新的系数，预测是使用模型做出预测的输出。

Alpha 是您必须在训练开始时指定的参数。这是学习率并控制系数（以及模型）每次更新时的变化或学习量。在线学习中使用较大的学习率（当我们更新每个训练实例的模型时）。良好的值可能在 0.1 到 0.3 的范围内。我们使用 0.3 的值。

您会注意到等式中的最后一项是 x，这是系数的输入值。您会注意到 B0 没有输入。该系数通常称为偏差或截距，我们可以假设它的输入值始终为 1.0。当使用向量或数组实现算法时，此假设可能有所帮助。

让我们使用前一节中的预测（0.5）和系数值（0.0）来更新系数。

b0 = b0 + 0.3 *（0-0.5）* 0.5 *（1-0.5）* 1.0

b1 = b1 + 0.3 *（0-0.5）* 0.5 *（1-0.5）* 2.7810836

b2 = b2 + 0.3 *（0-0.5）* 0.5 *（1-0.5）* 2.550537003

要么

b0 = -0.0375

b1 = -0.104290635

b2 = -0.09564513761

### 重复这个过程

我们可以重复此过程并更新数据集中每个训练实例的模型。

通过训练数据集的单次迭代称为迭代。对于固定数量的迭代重复随机梯度下降过程是常见的。

在迭代的末尾，您可以计算模型的误差值。因为这是一个分类问题，所以很容易了解模型在每次迭代时的准确度。

下图显示了模型在 10 个时期内的准确度图。

![Logistic Regression with Gradient Descent Accuracy versus Iteration](img/a7031b0865ce5bdb6a761d9cf76a625a.jpg)

具有梯度下降精度与迭代的逻辑回归

您可以看到模型很快就能在训练数据集上实现 100％的准确率。

在随机梯度下降的 10 个时期之后计算的系数是：

b0 = -0.4066054641

b1 = 0.8525733164

b2 = -1.104746259

### 作出预测

现在我们已经训练了模型，我们可以用它来做出预测。

我们可以对训练数据集做出预测，但这可以很容易地成为新数据。

使用 10 个时期之后学习的上述系数，我们可以计算每个训练实例的输出值：

```py
0.2987569857
0.145951056
0.08533326531
0.2197373144
0.2470590002
0.9547021348
0.8620341908
0.9717729051
0.9992954521
0.905489323
```

这些是属于 class = 0 的每个实例的概率。我们可以使用以下方法将它们转换为清晰的类值：

预测= IF（输出＆lt; 0.5）然后 0 Else 1

通过这个简单的过程，我们可以将所有输出转换为类值：

```py
0
0
0
0
0
1
1
1
1
1
```

最后，我们可以在训练数据集上计算模型的准确度：

准确度=（正确的预测/数字预测）* 100

准确度=（10/10）* 100

准确度= 100％

## 摘要

在这篇文章中，您了解了如何从零开始逐步实现逻辑回归。你了解到：

*   如何计算物流函数。
*   如何使用随机梯度下降来学习逻辑回归模型的系数。
*   如何使用逻辑回归模型做出预测。

您对此帖子或逻辑回归有任何疑问吗？
发表评论并提出问题，我会尽力回答。