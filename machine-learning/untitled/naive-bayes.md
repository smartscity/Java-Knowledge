---
description: >-
  朴素贝叶斯法是基于贝叶斯定理与特征条件独立假设的分类方法。最为广泛的两种分类模型是决策树模型(Decision Tree
  Model)和朴素贝叶斯模型（Naive Bayesian Model，NBM）。
---

# Naive Bayes

## 介绍

* **条件概率贝叶斯法则：**

  \*\*\*\*

  ![](../../.gitbook/assets/image-36.png)![](../../.gitbook/assets/image-39.png)

* **公式**

  Vnb =arg max P\( Vj \) Π i P \( ai \| Vj \)

* **特点**
  * **朴素贝叶斯分类器基于一个简单的假定：给定目标值时属性之间相互条件独立**
  * 基于概率的预测
  * 所需参数较少
  * 对缺失数据不敏感
  * 理论误差小，实际误差大，因为NBC模型假设属性之间相互独立，这个假设在实际应用中往往是不成立的，这给NBC模型的正确分类带来了一定影响。

> 参考文献
>
> [https://blog.csdn.net/amds123/article/details/70173402](https://blog.csdn.net/amds123/article/details/70173402)

