## 论文笔记 - LPANNI: Overlapping Community Detection Using Label Propagation in Large-Scale Complex Networks

#### 主要工作
标签传播算法是一种线性时间复杂度的社团检测算法，人们提出了一种用于大规模网络中社团检测的标签传播算法。但大多数算法还不能用于检测重叠社团，要么就是检测结果不准确。针对缺陷，本文提出一种改进的重叠社团检测算法：LPANNI(标签传播算法与邻居节点的影响)

#### 关键字
Label propagation algorithm; large-scale network; neighbor node influence; overlapping community detection

#### 理论方法
**Introduction**
Label Propagatio Algorithm(LPA)标签传播算法是由Raghavan等人于2007年提出的一种接近线性时间复杂度的社团检测算法，优点在于能够快速有效地检测出大规模复杂网络中的社团结构，缺点在于1)只能检测出非重叠社团；2)在许多网络中，由于考虑到传播标签时所有邻居节点的影响是相同的，所以它的精度较低；3)标签传播的随机性决定了群体检测的不稳定性；4)标签传播的随机性也产生了“标签扩散”问题。

针对LPA不能检测重叠社团的缺点，一些改进算法开始提出：COPRA，SLPA，DLPA。这些算法通过采用多重标签和隶属系数来判断一个节点属于多个社团的可能性。NIBLPA，LPA_NI，WLPA和CK-LPA算法考虑到节点重要性和标签影响能够提升社团检测的精度，但只有WLPA能够检测重叠社团。

本文提出的LPANNI算法采用基于节点重要性的固定标签传播序列和基于邻居节点影响和历史标签优先策略的标签更新策略来检测重叠的社区。本文主要工作：
1. 三个新的衡量指标，NI(node importance)、Sim(similarity between nodes)、NNI(neighbor node influenc)来反映邻居节点对标签传播中的不同影响。
2. 一个基于节点重要性升序排列的固定标签传播顺序，一个基于邻居节点影响的标签更新策略和历史标签优先策略。

以上两者有效避免便签传播的随机性和加速标签传播的收敛。




#### 数据来源

#### 实验验证及分析

#### 讨论与展望

#### 心得体会

#### 参考
