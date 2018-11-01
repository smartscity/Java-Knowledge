# Basic Knowledge

* **Dataset&lt;Row&gt;**
  * **数据集，用于存储训练集和测试集**
* **Dataset\(\).stat\(\).corr\("Survived", "Pclass"\)**
  * 相关性分析
* **Double ageMean = data.select\( mean\( "Age" \) \).head\(\).getDouble\( 0 \);**
  * **用mean函数分析，获取大多数加权值**
* **data = data.na\(\).fill\( ageMean, new String\[\]{"Age"} \);**
  * **将Value填充某一列中，所有空值**
* **data = data.withColumn\( "Family", data.col\( "Parch" \).$plus\( data.col\( "SibSp" \)\).$plus\( 1 \)\);**
  * **新增Family列，值=Parch+SibSp+1**
* **Normalizer**
  * Normalizer normalizer = new Normalizer\(\).setInputCol\("norm\_features"\).setOutputCol\("features"\) .setP\(1.0\);
  * **L1正则化处理**
* **Pipeline**
  * 连接多个转换器和预测器在一起，形成一个机器学习工作流
* **BinaryClassificationEvaluator**
  * **二分问题结果评估**
  * Double score = evaluator.evaluate\(bestModel\);
  * **评估模型得分**
* **CrossValidator**
  * 交叉验证





* 评估指标
  * **回归评估指标**
  * **多元分类**
  * **聚类评估指标**
  * \*\*\*\*
* 回归评估指数
  * **MSE**
    * 均方差（MSE），就是对各个实际存在评分的项，pow（预测评分-实际评分，2）的值进行累加，在除以项数。
  * **RMSE**
    * 均方根差（RMSE）就是MSE开根号
  * **MAPK/MAP**
    * K值平均准确率（MAPK）
  * R2（拟合优度检验）
  * MAE\(平均绝对误差\)

