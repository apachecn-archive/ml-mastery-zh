# 为什么用随机权重初始化神经网络？

> 原文： [https://machinelearningmastery.com/why-initialize-a-neural-network-with-random-weights/](https://machinelearningmastery.com/why-initialize-a-neural-network-with-random-weights/)

必须将人工神经网络的权重初始化为小的随机数。

这是因为这是用于训练模型的随机优化算法的期望，称为随机梯度下降。

要理解这种解决问题的方法，首先必须了解非确定性和随机算法的作用，以及随机优化算法在搜索过程中利用随机性的必要性。

在这篇文章中，您将发现为什么必须随机初始化神经网络权重的完整背景。

阅读这篇文章后，你会知道：

*   关于针对具有挑战性的问题的非确定性和随机算法的需求。
*   在随机优化算法中初始化和搜索期间使用随机性。
*   随机梯度下降是随机优化算法，需要随机初始化网络权重。

让我们开始吧。

![Why Initialize a Neural Network with Random Weights?](img/d767349f43cecc391f31806440729f12.png)

为什么用随机权重初始化神经网络？
照 [lwtt93](https://www.flickr.com/photos/37195641@N03/7086827121/) ，保留一些权利。

## 概观

这篇文章分为 4 部分;他们是：

1.  确定性和非确定性算法
2.  随机搜索算法
3.  神经网络中的随机初始化
4.  初始化方法

## 确定性和非确定性算法

经典算法是确定性的。

一个例子是对列表进行排序的算法。

给定一个未排序的列表，排序算法，比如冒泡排序或快速排序，将系统地对列表进行排序，直到您有一个有序的结果。确定性意味着每次给出相同的列表时，它将以完全相同的方式执行。它将在程序的每个步骤中进行相同的移动。

确定性算法很棒，因为它们可以保证最佳，最差和平均运行时间。问题是，它们不适合所有问题。

有些问题对计算机来说很难。也许是因为组合的数量;也许是因为数据的大小。它们非常难，因为确定性算法不能用于有效地解决它们。该算法可能会运行，但会继续运行直到宇宙的热量死亡。

另一种解决方案是使用[非确定性算法](https://en.wikipedia.org/wiki/Nondeterministic_algorithm)。这些是在算法执行期间做出决策时使用[随机性](https://en.wikipedia.org/wiki/Randomized_algorithm)元素的算法。这意味着当在相同数据上重新运行相同的算法时，将遵循不同的步骤顺序。

他们可以迅速加快获得解决方案的过程，但解决方案将是近似的，或“_ 好 _”，但往往不是“_ 最佳 _。”不确定性算法往往不能强保证运行时间或找到的解决方案的质量。

这通常很好，因为问题非常严重，任何好的解决方案通常都会令人满意。

## 随机搜索算法

搜索问题通常非常具有挑战性，需要使用大量使用随机性的非确定性算法。

算法本身并不是随机的;相反，他们会谨慎使用随机性。它们在一个边界内是随机的，被称为[随机算法](https://en.wikipedia.org/wiki/Stochastic_optimization)。

搜索的增量或逐步性质通常意味着过程和算法被称为从初始状态或位置到最终状态或位置的优化。例如，随机优化问题或随机优化算法。

一些例子包括遗传算法，模拟退火和随机梯度下降。

搜索过程是从可能的解决方案空间的起点到一些足够好的解决方案的增量。

它们在使用随机性方面具有共同特征，例如：

*   在初始化期间使用随机性。
*   在搜索过程中使用随机性。

我们对搜索空间的结构一无所知。因此，为了消除搜索过程中的偏差，我们从随机选择的位置开始。

随着搜索过程的展开，我们有可能陷入搜索空间的不利区域。在搜索过程中使用随机性可能会导致失败并找到更好的最终候选解决方案。

陷入困境并返回不太好的解决方案的想法被称为陷入局部最优。

这两个元素在搜索过程中随机初始化和随机性一起工作。

如果我们将搜索找到的任何解决方案视为临时或候选，并且搜索过程可以多次执行，它们可以更好地协同工作。

这为随机搜索过程提供了多个机会来启动和遍历候选解决方案的空间，以寻找更好的候选解决方案 - 即所谓的全局最优解。

候选解决方案空间的导航通常使用山脉和山谷的一个或两个景观的类比来描述（例如像[健身景观](https://en.wikipedia.org/wiki/Fitness_landscape)）。如果我们在搜索过程中最大化得分，我们可以将景观中的小山丘视为当地的最佳山峰，将最大的山丘视为全球最佳山峰。

这是一个迷人的研究领域，我有一些背景。例如，看我的书：

*   [聪明的算法：自然启发的编程食谱](http://cleveralgorithms.com/nature-inspired/index.html)

## 神经网络中的随机初始化

使用称为随机梯度下降的随机优化算法训练人工神经网络。

该算法使用随机性，以便为正在学习的数据中的输入到输出的特定映射函数找到足够好的权重集。这意味着每次运行训练算法时，特定训练数据的特定网络将适合具有不同模型技能的不同网络。

这是一个功能，而不是一个 bug。

我在帖子中更多地写了这个问题：

*   [在机器学习中拥抱随机性](https://machinelearningmastery.com/randomness-in-machine-learning/)

如前一节所述，诸如随机梯度下降的随机优化算法在选择搜索的起始点和搜索的进展中使用随机性。

具体而言，随机梯度下降要求将网络的权重初始化为小的随机值（随机，但接近零，例如[0.0,0.1]）。在搜索过程中，在每个时期之前的训练数据集的混洗中也使用随机性，这反过来导致每个批次的梯度估计的差异。

您可以在这篇文章中了解更多关于随机梯度下降的信息：

*   [微量批量梯度下降的简要介绍以及如何配置批量大小](https://machinelearningmastery.com/gentle-introduction-mini-batch-gradient-descent-configure-batch-size/)

搜索或学习神经网络的进展称为收敛。发现次优解或局部最优被称为早熟收敛。

> 用于深度学习模型的训练算法本质上通常是迭代的，因此需要用户指定开始迭代的一些初始点。此外，训练深度模型是一项非常困难的任务，大多数算法都会受到初始化选择的强烈影响。

- 第 301 页，[深度学习](https://amzn.to/2H5wjfg)，2016 年。

评估神经网络配置技能的最有效方法是多次重复搜索过程，并报告模型在这些重复上的平均表现。这为配置提供了从多个不同初始条件集搜索空间的最佳机会。有时这称为多次重启或多次重启搜索。

您可以在这篇文章中了解有关神经网络有效评估的更多信息：

*   [如何评估深度学习模型的技巧](https://machinelearningmastery.com/evaluate-skill-deep-learning-models/)

### 为什么不将权重设置为零？

每次我们训练网络时，我们都可以使用相同的权重集;例如，您可以对所有权重使用 0.0 的值。

在这种情况下，学习算法的方程将无法对网络权重进行任何更改，并且模型将被卡住。重要的是要注意，每个神经元中的偏差权重默认设置为零，而不是一个小的随机值。

具体地，在连接到相同输入的隐藏层中并排的节点必须具有用于学习算法的不同权重以更新权重。

这通常被称为在训练期间需要打破对称性。

> 也许唯一已知完全确定的属性是初始参数需要在不同单元之间“打破对称性”。如果具有相同激活功能的两个隐藏单元连接到相同的输入，则这些单元必须具有不同的初始参数。如果它们具有相同的初始参数，则应用于确定性成本和模型的确定性学习算法将以相同方式不断更新这两个单元。

- 第 301 页，[深度学习](https://amzn.to/2H5wjfg)，2016 年。

### 何时初始化为相同权重？

每次训练网络时，我们都可以使用相同的随机数。

在评估网络配置时，这没有用。

在生产环境中使用模型的情况下，给定训练数据集训练相同的最终网络权重集可能是有帮助的。

您可以在此文章中了解有关修复使用 Keras 开发的神经网络的随机种子的更多信息：

*   [如何使用 Keras](https://machinelearningmastery.com/reproducible-results-neural-networks-keras/) 获得可重现的结果

## 初始化方法

传统上，神经网络的权重被设置为小的随机数。

神经网络权重的初始化是一个完整的研究领域，因为网络的仔细初始化可以加速学习过程。

现代深度学习库，例如 Keras，提供了许多网络初始化方法，所有这些都是用小随机数初始化权重的变体。

例如，在为所有网络类型编写时，Keras 中提供了当前的方法：

*   **Zeros** ：生成张量初始化为 0 的初始值设定项。
*   **Ones** ：生成张量初始化为 1 的初始值设定项。
*   **常量**：生成张量初始化为常量值的初始值设定项。
*   **RandomNormal** ：生成具有正态分布的张量的初始化器。
*   **RandomUniform** ：生成具有均匀分布的张量的初始化器。
*   **TruncatedNormal** ：生成截断正态分布的初始化程序。
*   **VarianceScaling** ：初始化程序，能够使其比例适应权重的形状。
*   **Orthogonal** ：生成随机正交矩阵的初始化器。
*   **Identity** ：生成单位矩阵的初始化程序。
*   **lecun_uniform** ：LeCun 统一初始化器。
*   **glorot_normal** ：Glorot 正常初始化器，也称为 Xavier 正常初始化器。
*   **glorot_uniform** ：Glorot 统一初始化器，也叫 Xavier 统一初始化器。
*   **he_normal** ：他正常的初始化程序。
*   **lecun_normal** ：LeCun 正常初始化程序。
*   **he_uniform** ：他统一方差缩放初始化器。

有关详细信息，请参阅[文档](https://keras.io/initializers/)。

出于兴趣，Keras 开发人员为不同的层类型选择的默认初始值设定项如下：

*   **致密**（例如 MLP）： _glorot_uniform_
*   **LSTM** ： _glorot_uniform_
*   **CNN** ： _glorot_uniform_

您可以在本文中了解更多关于“ _glorot_uniform_ ”，也称为“ _Xavier normal_ ”，以 Xavier Glorot 方法的开发人员命名：

*   [了解深度前馈神经网络训练的难度](http://proceedings.mlr.press/v9/glorot10a.html)，2010。

没有单一的最佳方法来初始化神经网络的权重。

> 现代初始化策略简单且具有启发性。设计改进的初始化策略是一项艰巨的任务，因为神经网络优化还不是很清楚。 [...]我们对初始点如何影响泛化的理解特别原始，几乎没有为如何选择初始点提供指导。

- 第 301 页，[深度学习](https://amzn.to/2H5wjfg)，2016 年。

这是一个超级参数，供您探索，测试和试验您的特定预测建模问题。

你有一个最喜欢的重量初始化方法吗？
请在下面的评论中告诉我。

## 进一步阅读

如果您希望深入了解，本节将提供有关该主题的更多资源。

### 图书

*   [深度学习](https://amzn.to/2H5wjfg)，2016 年。

### 用品

*   维基百科上的[非确定性算法](https://en.wikipedia.org/wiki/Nondeterministic_algorithm)
*   [维基百科上的随机算法](https://en.wikipedia.org/wiki/Randomized_algorithm)
*   [维基百科上的随机优化](https://en.wikipedia.org/wiki/Stochastic_optimization)
*   [维基百科上的随机梯度下降](https://en.wikipedia.org/wiki/Stochastic_gradient_descent)
*   [维基百科上的健身景观](https://en.wikipedia.org/wiki/Fitness_landscape)
*   [神经网络常见问题](ftp://ftp.sas.com/pub/neural/FAQ.html)
*   [Keras 重量初始化](https://keras.io/initializers/)
*   [了解深度前馈神经网络训练的难度](http://proceedings.mlr.press/v9/glorot10a.html)，2010。

### 讨论

*   [神经网络中有哪些好的初始权重？](https://stats.stackexchange.com/questions/47590/what-are-good-initial-weights-in-a-neural-network)
*   [为什么神经网络的权重应该初始化为随机数？](https://stackoverflow.com/questions/20027598/why-should-weights-of-neural-networks-be-initialized-to-random-numbers)
*   [神经网络中有哪些好的初始权重？](https://www.quora.com/What-are-good-initial-weights-in-a-neural-network)

## 摘要

在这篇文章中，您发现了为什么必须随机初始化神经网络权重。

具体来说，你学到了：

*   关于针对具有挑战性的问题的非确定性和随机算法的需求。
*   在随机优化算法中初始化和搜索期间使用随机性。
*   随机梯度下降是随机优化算法，需要随机初始化网络权重。

你有任何问题吗？
在下面的评论中提出您的问题，我会尽力回答。