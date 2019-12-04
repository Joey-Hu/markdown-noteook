## 论文笔记

**题目**
Community Detection inMulti-Layer Networks Using Joint Nonnegative Matrix Factorization

**关键字**
Multi-layer networks;  Community Structure; nonnegative matrix factorization(加权非负矩阵分解); semi-supervised clustering

**理论方法**
实现单层网络的社团检测算法已经很多了，很自然的就对单层网络的算法进行扩展，目前扩展有两种策略：
1. 将多层网络分解成单层网络，再用单层网络的算法检测社团。
2. 采用单层网络算法检测获取每一层的社团，然后利用共识聚类将各层社团进行连接

以上方法被认为准确率低，忽略了各层之间的连接。

本文主要工作：
1. 提出了一个适合多层网络中的社团量化函数，该函数既适用于未加权的多层网络，也适用于加权多层网络。
2. 证明了核k均值(kernel K-means)、NMF、谱聚类(spectral clustering)、多视点聚类(multi-view clustering)、多层模块密度(multi-layer modularity density)等目标函数之间的等价性，为多层社区检测算法提供了理论基础。
3. 提出了局部监督(partial supervision)与NMF相结合的S2-jNMF算法。该算法为多层网络中社区检测的半监督聚类提供了一个通用框架。该算法在不增加时间复杂度的前提下，提高了算法的精度。实验结果表明，该算法具有较好的性能。

!["主要符号"]()

多层模块密度
作者首先定义一个单层网络的模块密度，计算公式如下：
$$
\begin{aligned}
Q_D(\{ V_c\}_{c=1}^k)=\sum_{c=1}^{k}\frac{L(V_c, V_c)-L(V_c, \bar{V_c})}{|V_c|}
\end{aligned}
$$

其中，$\{V_c\}_{c=1}^k$是图G的硬分区，$V_c$是$cth$层的社团的结点，$k$是社团个数，$L(V_1,V_2)=\sum_{i \in V_1, j \in V_2} \omega_{ij}$，$\omega_{ij}$是结点i和节点j之间的权重，$\bar{V_c}=V \setminus V_c$

!["硬分区"]()

理想情况下，作者要通过最大化每层模块密度，总而获得最优划分。这样，就把问题从多层模块检测转化为一个多目标优化问题(multi-objective optimization problem)。

$$
\begin{equation} \left\lbrace \begin{array}{ll}\max Q_{D}^{[1]}(\lbrace V_{c}\rbrace _{c=1}^{k}), & \\ \max Q_{D}^{[2]}(\lbrace V_{c}\rbrace _{c=1}^{k}), & \\ \qquad \qquad \cdots & \\ \max Q_{D}^{[m]}(\lbrace V_{c}\rbrace _{c=1}^{k}), & \end{array} \right. \end{equation}
$$

然而对每一层求最大化模块密度是不可能的，所以作者放宽条件，最大化多层平均模块密度。多层模块密度定义(multi-layer modular- ity density)如下:
$$
\begin{aligned}
Q_{D}^{\mathcal {G}}(\lbrace V_{c}\rbrace _{c=1}^{k})=\frac{1}{m}\sum _{l=1}^{m}Q_{D}^{[l]}(\lbrace V_{c}\rbrace _{c=1}^{k}).
\end{aligned}
$$

这样把一个多目标优化问题转化为一个单目标优化问题。

**数据来源**

**实验验证及分析**

**讨论与展望**


**心得体会**


