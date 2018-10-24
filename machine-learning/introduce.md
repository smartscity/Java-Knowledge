# Introduce

## 名词解释

* **机器学习**
  * **实际上就是寻找X 到 Y之间的一种对应关系 F，这种对应关系F 称之为一个模型；然而机器学习就是对很多个X 和 Y，通过对模型的训练，然后去发现这个最优的模型的过程。**
* **拟合**
  * **过拟合**
    * **是指训练好的模型F，对某些特征要求过于苛刻**
  * **欠拟合**
    * **与过拟合相反**
  * **Just Fit**
    * **刚刚好**
* **特征工程**
  * **一维特征：只有一个自变量的关系式**

                       **e.g. f\(衣服厚度\) = 温度      只有一个温度自变量**

  * **二维特征：**

                       **e.g. f\(衣服厚度\) = 温度 + 风速      温度和风速两个自变量**
* **划分训练集和测试集**
* **评价模型**
  * **WOE（Weight of Evidence）证据权重**

    \*\*\*\*

  * **GINI系数**
  * **混淆矩阵**
  * **ROC曲线**
  * **KS曲线**
  * **KS（Kolmogorov-Smirnov）值**
    * 柯尔莫哥洛夫-斯米尔诺夫检验（Колмогоров-Смирнов检验）基于累计分布函数，用以检验两个经验分布是否不同或一个经验分布与另一个理想分布是否不同。
    * KS值越大，表示模型能够将正、负客户区分开的程度越大。
    * 绘制方式与ROC曲线略有相同，都要计算TPR和FPR。但是TPR和FPR都要做纵轴，横轴为把样本分成多少份。
    * KS曲线是两条线，其横轴是阈值，纵轴是TPR与FPR。两条曲线之间之间相距最远的地方对应的阈值，就是最能划分模型的阈值。
    * **KS值即为Max\(TPR-FPR\)**
    * * \*\*\*\*

> 注解
>
> 在实际建模中需要重复特征工程、变量离散化、KS检验等步骤，不断优化以达到更优效果

\*\*\*\*

\*\*\*\*

\*\*\*\*


