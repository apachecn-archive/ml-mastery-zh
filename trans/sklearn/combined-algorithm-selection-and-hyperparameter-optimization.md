# 组合算法选择和超参数优化(CASH 优化)

> 原文：<https://machinelearningmastery.com/combined-algorithm-selection-and-hyperparameter-optimization/>

机器学习模型的选择和配置可能是应用机器学习中最大的挑战。

必须进行对照实验，以发现什么最适合给定的分类或回归预测建模任务。考虑到可以考虑的大量数据准备方案、学习算法和模型超参数，这可能会让人感到难以承受。

常见的方法是使用快捷方式，例如使用流行的算法或使用默认超参数测试少量算法。

现代的替代方案是考虑数据准备、学习算法和算法超参数的选择一个大的全局优化问题。这种表征通常被称为组合算法选择和超参数优化，简称为“ **CASH 优化**”。

在这篇文章中，你将发现机器学习模型选择的挑战和被称为 CASH Optimization 的现代解决方案。

看完这篇文章，你会知道:

*   机器学习模型和超参数选择的挑战。
*   使用流行模型或做出一系列连续决策的捷径。
*   作为现代自动驾驶基础的组合算法选择和超参数优化的特征。

我们开始吧。

![Combined Algorithm Selection and Hyperparameter Optimization (CASH Optimization)](img/25e0af1ec24d3c4e31badf80848a102d.png)

组合算法选择和超参数优化(CASH 优化)
图片由[伯纳德·斯拉格提供。新西兰](https://flickr.com/photos/volvob12b/48255064616/)，保留部分权利。

## 概观

本教程分为三个部分；它们是:

1.  模型和超参数选择的挑战
2.  模型和超参数选择的解决方案
3.  组合算法选择和超参数优化

## 模型和超参数选择的挑战

机器学习算法与预测建模任务之间没有明确的映射。

我们无法查看数据集并知道要使用的最佳算法，更不用说为给定模型准备数据或最佳配置要使用的最佳数据转换了。

相反，我们必须使用受控实验来发现什么最适合给定的数据集。

因此，应用机器学习是一门经验学科。与其说是科学，不如说是工程和艺术。

问题是，有几十种(如果不是几百种)机器学习算法可供选择。每个算法最多可以配置几十个超参数。

对于初学者来说，这个问题的范围是巨大的。

*   你从哪里开始？
*   你从什么开始？
*   你什么时候丢弃一个模型？
*   你什么时候加倍关注一个模型？

对于这个问题，大多数从业者都采用了一些标准的解决方案，无论是有经验的还是其他的。

## 模型和超参数选择的解决方案

让我们看看选择数据转换、机器学习模型和模型超参数这一问题的两个最常见的捷径。

### 使用流行的算法

一种方法是使用流行的机器学习算法。

> 当面对这些自由度时，做出正确的选择可能会很有挑战性，让许多用户根据声誉或直觉选择算法，和/或让超参数设置为默认值。当然，这种方法产生的性能可能比最佳方法和超参数设置的性能差得多。

——[Auto-WEKA:分类算法的组合选择和超参数优化](https://arxiv.org/abs/1208.3719)，2012。

例如，如果看起来每个人都在谈论“*随机森林*”，那么随机森林就成了你遇到的所有分类和回归问题的正确算法，你把实验限制在随机森林算法的超参数上。

*   **捷径#1** :使用一个流行的算法，比如*随机森林*或者 *xgboost* 。

随机森林确实在广泛的预测任务中表现良好。但是我们无法知道它对于给定的数据集是好的还是最好的。风险在于，我们可能能够用更简单的线性模型获得更好的结果。

一种解决方法可能是测试一系列流行的算法，从而找到下一条捷径。

### 顺序测试变换、模型和超参数

另一种方法是将问题作为一系列连续的决策来处理。

例如，检查数据并选择使数据更具高斯性的数据变换，移除异常值等。然后用默认超参数测试一套算法，并选择一个或几个表现良好的算法。然后调整那些表现最好的模型的超参数。

*   **捷径#2** :依次选择数据变换、模型和模型超参数。

这是我推荐的快速获得好结果的方法；例如:

*   [应用机器学习过程](https://machinelearningmastery.com/start-here/#process)

这种捷径也可能是有效的，并降低了丢失在数据集上表现良好的算法的可能性。如果你正在寻求伟大或优秀的结果，而不仅仅是快速的好结果，那么这里的缺点会更加微妙并影响你。

风险在于，在选择模型之前选择数据转换可能意味着您错过了从算法中获得最大收益的数据准备序列。

同样，在选择模型超参数之前选择模型或模型子集意味着您可能会丢失一个模型，该模型的超参数不同于默认值，其性能优于任何选定的模型子集及其后续配置。

> AutoML 中的两个重要问题是(1)没有单一的机器学习方法在所有数据集上表现最佳，以及(2)一些机器学习方法(例如非线性支持向量机)关键依赖于超参数优化。

—第 115 页，[自动化机器学习:方法、系统、挑战](https://amzn.to/2w2gVf4s)，2019。

解决方法可能是抽查每个算法的良好或性能良好的配置，作为算法抽查的一部分。这只是部分解决方案。

有更好的方法。

## 组合算法选择和超参数优化

选择数据准备管道、机器学习模型和模型超参数是一个搜索问题。

每个步骤中可能的选择定义了一个搜索空间，单个组合代表该空间中可以用数据集评估的一个点。

高效地导航搜索空间被称为全局优化。

这在机器学习领域已经被很好地理解了很长一段时间，尽管可能是默认的，通常集中在问题的一个元素上，例如超参数优化。

重要的见解是，每个步骤之间都有依赖性，这会影响搜索空间的大小和结构。

> ……[问题]可以看作是一个单一的分层超参数优化问题，其中甚至算法本身的选择也被认为是一个超参数。

—第 82 页，[自动化机器学习:方法、系统、挑战](https://amzn.to/2w2gVf4s)，2019。

这要求数据准备和机器学习模型以及模型超参数必须形成优化问题的范围，并且优化算法必须知道它们之间的依赖关系。

这是一个具有挑战性的全局优化问题，特别是因为依赖关系，但也因为在数据集上估计机器学习模型的性能是随机的，导致性能分数的噪声分布(例如通过[重复的 k 倍交叉验证](https://machinelearningmastery.com/k-fold-cross-validation/))。

> ……学习算法及其超参数的组合空间对搜索来说非常具有挑战性:响应函数是有噪声的，空间是高维的，涉及分类和连续选择，并且包含层次依赖(例如，学习算法的超参数只有在选择了该算法的情况下才有意义；只有选择了集成方法，集成方法中算法选择才有意义；等等)。

——[Auto-WEKA:分类算法的组合选择和超参数优化](https://arxiv.org/abs/1208.3719)，2012。

克里斯·桑顿等人在 2013 年的论文《自动 WEKA:分类算法的组合选择和超参数优化》中对这一挑战进行了最好的描述在论文中，他们将这个问题简称为“*组合算法选择和超参数优化*”或“*现金优化*”。

> …机器学习的一个自然挑战:给定一个数据集，自动同时选择一个学习算法并设置其超参数，以优化经验性能。我们称之为组合算法选择和超参数优化问题(简称:CASH)。

——[Auto-WEKA:分类算法的组合选择和超参数优化](https://arxiv.org/abs/1208.3719)，2012。

这种特性有时也被称为“全模式选择”，简称 FMS。

> FMS 问题由以下内容组成:给定一组预处理方法、特征选择和学习算法，选择这些方法的组合，以获得给定数据集的最低分类误差。该任务还包括为所考虑的方法选择超参数，从而产生非常适合随机优化技术的巨大搜索空间。

——[粒子群模型选择](http://www.jmlr.org/papers/v10/escalante09a.html)，2009。

Thornton 等人继续使用知道依赖性的全局优化算法，即所谓的顺序全局优化算法，例如[贝叶斯优化](https://machinelearningmastery.com/what-is-bayesian-optimization/)的特定版本。然后，他们开始为名为[自动威卡项目](https://www.cs.ubc.ca/labs/beta/Projects/autoweka/)的威卡机器学习工作台实施他们的方法。

> 一个有前途的方法是贝叶斯优化，特别是基于序列模型的优化(SMBO)，这是一个通用的随机优化框架，可以处理分类和连续超参数，并可以利用源于条件参数的层次结构。

—第 85 页，[自动化机器学习:方法、系统、挑战](https://amzn.to/2w2gVf4s)，2019。

这为一个被称为“自动机器学习”的研究领域提供了主导范式。AutoML 关心的是提供工具，允许具有中等技术技能的从业者快速找到机器学习任务的有效解决方案，例如分类和回归预测建模。

> AutoML 旨在提供有效的现成学习系统，将专家和非专家从为手头数据集选择正确算法的繁琐而耗时的任务中解放出来，同时提供正确的预处理方法和所有相关组件的各种超参数。

—第 136 页，[自动化机器学习:方法、系统、挑战](https://amzn.to/2w2gVf4s)，2019。

AutoML 技术由机器学习库提供，并越来越多地作为服务提供，即所谓的机器学习即服务，简称 MLaaS。

## 进一步阅读

如果您想更深入地了解这个主题，本节将提供更多资源。

### 报纸

*   [Auto-WEKA:分类算法的组合选择和超参数优化](https://arxiv.org/abs/1208.3719)，2012。
    [Auto-WEKA 2.0:WEKA](http://jmlr.org/papers/v18/16-261.html)中的自动选型和超参数优化，2016。
*   [做模型搜索的科学:视觉架构的数百维超参数优化](http://proceedings.mlr.press/v28/bergstra13.pdf)，2013。
*   [粒子群模型选择](http://www.jmlr.org/papers/v10/escalante09a.html)，2009。

### 书

*   [自动化机器学习:方法、系统、挑战](https://amzn.to/2w2gVf4s)，2019。

### 文章

*   [自动机器学习，维基百科](https://en.wikipedia.org/wiki/Automated_machine_learning)。
*   [AutoWEKA 项目](https://www.cs.ubc.ca/labs/beta/Projects/autoweka/)。

## 摘要

在这篇文章中，你发现了机器学习模型选择的挑战和被称为 CASH Optimization 的现代解决方案。

具体来说，您了解到:

*   机器学习模型和超参数选择的挑战。
*   使用流行模型或做出一系列连续决策的捷径。
*   作为现代自动驾驶基础的组合算法选择和超参数优化的特征。

**你有什么问题吗？**
在下面的评论中提问，我会尽力回答。