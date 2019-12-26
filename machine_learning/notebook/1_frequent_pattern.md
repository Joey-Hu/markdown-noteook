## 频繁项集挖掘算法

### Apriori算法
算法目标：找到最多的k项频繁子集

#### 相关定义：

!["数据"](https://raw.githubusercontent.com/Joey-Hu/markdown-noteook/master/machine_learning/images/apriori_algo/data.jpg)
1. 支持度(support)：数据集中包含该项集记录所占的比例，如$I_1$出现3次，项集总数量为5，则：
$$
\begin{equation}
support(A \Rightarrow B) = P(A \cup B) = \frac {\left|A \cup B \right|}{\left|D \right|} -- 表示A和B同时发生发概率
\end{equation}
$$
$$
\begin{equation}
support(I_1) = 3/5 = 0.6
\end{equation}
$$

2. 置信度(confidence)：即条件概率
$$
\begin{equation}
confidence(A \Rightarrow B) = P(A \mid B) = \frac {\left|A \cup B \right|}{\left|A \right|} -- 表示A和B同时发生发概率
\end{equation}
$$

> Apriori定律1：如果一个集合是频繁项集，则它的所有子集都是频繁项集。-- 常用于先验剪枝
Apriori定律2：如果一个集合不是频繁项集，则它的所有超集都不是频繁项集。

#### Apriori算法
1. 扫描数据集，得到候选1项集C1;
2. 挖掘频繁k(k>=2)项集：
		由频繁k-1项集生成频繁项集k项集候选Ck，扫描计算候选Ck项集的支持度。
		剪枝去掉候选k项集中支持度低于最小支持度α的数据集，得到频繁k项集。如果频繁k项集为空，则返回频繁k-1项集的集合作为算法结果，算法结束。如果得到的频繁k项集只有一项，则直接返回频繁k项集的集合作为算法结果，算法结束。
		基于频繁k项集，连接生成候选k+1项集。
3. 利用步骤2，迭代得到k=k+1项集结果。
4. 利用得到的频繁项集进行置信度规则分析，得到强关联关系的项集关系

**举例**
!["举例"](https://raw.githubusercontent.com/Joey-Hu/markdown-noteook/master/machine_learning/images/apriori_algo/example.jpg)

### FP-growth算法

Apriori算法可能受到两种非平凡开销的影响：需要产生大量的候选项集；需要重复地扫描整个数据库，通过模式匹配检查一个很大的候选集合。检查数据库中每个事务来确定候选项集支持度的开销很大。

为避免这种代价昂贵的候选产生过程，设计了一种方法：频繁模式增长(FP-growth)，它采用如下分治策略：首先，将代表频繁项集的数据库压缩到一颗频繁模式树（FP树），该树仍保留项集的关联信息。然后，把这种压缩后的数据库划分成一组条件数据库，每个数据库关联一个频繁项或模式段，并分别挖掘每个条件数据库。对于每个“模式片段”，只需要考察它相关联数据集。因此，随着被考察的模式的“增长”，这种方法可以显著地压缩被搜索的数据集的大小。

FP-growth算法的基本思路如下：
* 扫描一次事务数据库，找出频繁1-项集合，记为L，并把它们按支持度计数的降序进行排列。
* 基于L，再扫描一次事务数据库，构造表示事务数据库中项集关联的FP树。
* 在FP树上递归地找出所有频繁项集。
* 最后在所有频繁项集中产生强关联规则。

FP树的挖掘

**【转】举例**
!["fap_growth"](https://raw.githubusercontent.com/Joey-Hu/markdown-noteook/master/machine_learning/images/apriori_algo/fp-growth_example.jpg)







参考：
https://blog.csdn.net/qq_36187544/article/details/89184879
https://blog.csdn.net/my_learning_road/article/details/79726555
https://www.cnblogs.com/pinard/p/6307064.html


