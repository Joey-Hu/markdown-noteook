## 决策树

用决策树对需要测试的实例进行分类：从根节点开始，对实例的某一特征进行测试，根据测试结果，将实例分配到其子结点；这时，每一个子结点对应着该特征的一个取值。如此递归地对实例进行测试并分配，直至达到叶结点。最后将实例分配到叶结点的类中。

#### 决策树基本算法
!["决策树基本算法"](https://raw.githubusercontent.com/Joey-Hu/markdown-noteook/master/machine_learning/images/decision_tree/generate_decision_tree.png)

#### 相关概念

1. 熵(entropy)：体系的混乱的程度

2. 信息熵(information entropy)：度量样本集合纯度的一种指标
D表示标记类元组的训练集，假设类标号属性有m个不同值，定义了m个不同的类$C_i(i=1, ..., m)$，设$C_{i,D}$表示D中$C_{i}$类元组的集合，$|D|$和$|C_{i,D}|$分别表示D和$C_{i,D}$中元组的数量
$$
\begin{equation}
Info(D) = -\sum_{i=1}^{m}p_ilog_2(p_i)
\end{equation}. \tag{1}
$$
其中，$P_i = \frac {|C_{i,D}|}{|D|}$

3. 信息增益(Infomation Gain) -- ID3
假设我们按照某属性A来划分D中的元组，根据观测，属性A在数据集D中有v个不同取值{$a_1, a_2, ..., a_v$}。假设A是离散的，用属性A将D划分成v个子集{$D_1, D_2, ..., D_v$}，我们用以下公式来表示划分后的数据集的熵：
$$
\begin{equation}
Info_A(D) = \sum_{j=1}^{v}\frac {|D_j|}{|D|}xInfo(D_j)
\end{equation}. \tag{2}
$$

则信息增益定义为原来的信息熵与划分后的信息熵之间的差：
$$
\begin{equation}
Gain(A) = Info(D) - Info_A(D)
\end{equation}. \tag{3}
$$

当A是连续值？？

4. 增益率(Gain Ratio) -- C4.5

为克服使用唯一标识符来划分数据集，导致Info_{唯一标识符}(D)=0，采用分类信息将信息增益规范化。分裂信息定义如下：
$$
\begin{equation}
SplitInfo_A(D) = -\sum_{j=1}^{v}\frac{|D_j|}{|D|}xlog_2(\frac{|D_j|}{|D|})
\end{equation}. \tag{4}
$$
该值代表由训练数据集D划分成对应于属性A测试的v个输出的v个分区产生的信息。

增益率定义如下所示：
$$
\begin{equation}
GainRate(A) = \frac{Gain(A)}{SplitInfo_A(D)}
\end{equation}. \tag{5}
$$

5. 基尼指数(Gini index) -- CART
基尼指数度量数据分区或训练元组集D的不纯度，定义如下：
$$
\begin{equation}
Gini(D) = 1-\sum_{i=1}^{m}p_i^2
\end{equation}. \tag{6}
$$

基尼指数考虑每个属性的二元划分。
$$
\begin{equation}
Gini_A(D) = \frac{|D_1|}{|D|}Gini(D_1) + \frac{|D_2|}{|D|}Gini(D_2)
\end{equation}. \tag{7}
$$

#### 剪枝处理
剪枝处理主要是减少过拟合的处理手段，主要策略有以下两种：
* 预剪枝(prepruning)：在构造的过程中先评估，再考虑是否分支
	评估：
	性能度量，即决策树的泛化性能，在构造数的过程中，对一个节点考虑是否分支时，首先计算决策树不分支时在测试集上的性能，再计算分支之后的性能，若分支对性能没有提升，则选择不分支（即剪枝）。
	优缺点：
		优点：表示在构造好一颗完整的决策树后，从最下面的节点开始，考虑该节点分支对模型的性能是否有提升，若无则剪枝，即将该节点标记为叶子节点，类别标记为其包含样本最多的类别。 
		缺点：由于剪枝同时剪掉了当前节点后续子节点的分支，因此预剪枝“贪心”的本质阻止了分支的展开，在一定程度上带来了欠拟合的风险

* 后剪枝（post-pruning）：在构造好一颗完整的决策树后，自底向上，评估分支的必要性。
	表示在构造好一颗完整的决策树后，从最下面的节点开始，考虑该节点分支对模型的性能是否有提升，若无则剪枝，即将该节点标记为叶子节点，类别标记为其包含样本最多的类别。
	优缺点：
		优点：后剪枝则通常保留了更多的分支，因此采用后剪枝策略的决策树性能往往优于预剪枝
		缺点：但其自底向上遍历了所有节点，并计算性能，训练时间开销相比预剪枝大大提升。

```python
#!/usr/bin/env python
# -*- encoding: utf-8 -*-
'''
@File    :   CART.py
@Time    :   2019/11/25 09:38:59
@Author  :   Hu Hao
@Version :   1.0
@Desc    :   决策树（yCART），分类回归树
'''


import numpy as np


class Tree:
    def __init__(self, value=None, trueBranch=None,
                 falseBranch=None, results=None, col=1, summary=None,
                 data=None):
        self.value = value
        self.trueBranch = trueBranch
        self.falseBranch = falseBranch
        self.results = results
        self.col = col
        self.summary = summary
        self.data = data

    def __str__(self):
        print(self.col, self.value)
        print(self.results)
        print(self.summary)
        return ""


def calcuDiffCount(dataset):
    '''
    将输入的数据汇总(input, dataset)
    该函数是计算gini值的辅助函数，假设输入的dataSet为为['A', 'B', 'C', 'A', 'A', \'D']，
    则输出为['A':3,' B':1, 'C':1, 'D':1]，这样分类统计dataSet中每个类别的数量

    Args:
        dataset: 输入数据集

    Returns:
        result set{type1:type1Count,
                        type2:type2Count,
                        ...,
                        typeN:typeNCount}
    '''
    results = {}
    for data in dataset:
        # data[-1]表示数据类型
        if data[-1] not in results:
            results.setdefault(data[-1], 1)
        else:
            results[data[-1]] += 1
    return results


def gini(rows):
    """ 计算gini值 """
    length = len(rows)
    results = calcuDiffCount(rows)
    imp = 0.0
    for i in results:
        imp += results[i] / length * results[i] / length
    return 1 - imp


def splitDataset(rows, value, column):
    """
    split dataset by value, column
    return 2 part(list1, list2)
    """
    list1 = []
    list2 = []

    if isinstance(value, int) or isinstance(value, float):
        for row in rows:
            if row[column] >= value:
                list1.append(row)
            else:
                list2.append(row)
    else:
        for row in rows:
            if row[column] == value:
                list1.append(row)
            else:
                list2.append(row)
    return list1, list2


def loadCSV():
    '''
    加载CSV格式数据文件
    '''
    def convertDatatype(s):
        # string类型数据转换成float和int数据类型
        s = s.strip()
        try:
            if '.' in s:
                return float(s)
            else:
                return int(s)
        except ValueError:
            return s

    data = np.loadtxt("datas.csv", dtype="str", delimiter=",")
    data = data[1:, :]
    for row in data:
        for item in row:
            convertDatatype(item)
    return data


def buildDecisionTree(rows, evalutionFuction=gini):
    '''
    建立决策树，停止条件:gini=0
    '''
    column_length = len(rows[0])
    rows_length = len(rows)
    currentGain = evalutionFuction(rows)

    best_gain = 0.0
    best_value = None
    best_set = None

    # choose the best gain
    for col in range(column_length - 1):
        col_value_set = set([x[col] for x in rows])    # 获取每个属性中离散值的取值
        for value in col_value_set:
            list1, list2 = splitDataset(rows, value, col)
            p = len(list1) / rows_length
            gain = currentGain - p * evalutionFuction(list1) - (1 - p) * evalutionFuction(list2)
            # 获得gini指数最小的属性和值作为最优特征和最优分割点
            if gain > best_gain:
                best_gain = gain
                best_value = (col, value)
                best_set = (list1, list2)

    dcY = {'impurity': '%.3f' % currentGain, 'sample': '%d' % rows_length}

    # stop or not stop
    if best_gain > 0:
        trueBranch = buildDecisionTree(best_set[0], evalutionFuction)
        falseBranch = buildDecisionTree(best_set[1], evalutionFuction)
        return Tree(value=best_value[1], col=best_value[0], trueBranch=trueBranch, falseBranch=falseBranch, summary=dcY)
    else:
        return Tree(results=calcuDiffCount(rows), summary=dcY, data=rows)


if __name__ == "__main__":
    data = loadCSV()
    decisionTree = buildDecisionTree(data, evalutionFuction=gini)
    print(decisionTree)

```


