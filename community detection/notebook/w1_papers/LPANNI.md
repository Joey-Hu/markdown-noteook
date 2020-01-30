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

**相关工作**
作者分别介绍了LPA算法，和用于检测重叠社团的检测算法COPRA、SLPA、DLPA，但是以上算法都没有解决LPA算法的不稳定性问题。

>LPA算法划分结果不稳定，随机强主要表现在以下两个方面<sup>[1]</sup>：
>1. 节点标签更新顺序随机，越重要的节点越早更新会加速收敛的过程；
>2. 随机选择：如果一个节点的出现次数最大的邻居标签不止一个时，随机选择一个标签作为自己标签。这种随机性可能会带来一个雪崩效应，即刚开始一个小小的聚类错误会不断被放大。不过话也说话来，如果相似邻居节点出现多个，可能是weight计算的逻辑有问题，需要回过头去优化weight抽象和计算逻辑；

为提升LPA算法的精度和稳定性，开始考虑到用于更新节点标签的节点重要性和标签影响因素。一些算法被提出，如NIBLPA，LPA_NI，CK-LPA和WLPA算法，其中只有WLPA可以检测重叠社团。

因此，本文主要有两个推动因素来提出LPANNI算法：
1. 引入了邻居节点影响(NNI)这一新的度量，在更新节点标签时更合理地度量不同邻居节点的影响，从而提高了重叠一致性检测的准确性;
2. 该算法结合节点影响序列和历史标签优先策略对节点标签进行更新，显著降低了标签传播算法的不稳定性。

**术语定义**
*节点重要性(Node Importance, NI)*
衡量了一个节点称为潜在社团中心的可能性。
一个社团的中心节点具有以下特征：1) 该节点与更多的邻居节点相连接，即该节点的度越大；2) 这些邻居节点之间连接紧密，具体表现是该节点与其邻居节点形成的三角形越多。
$$
\begin{equation}
NI(u) = \frac{1}{2} + \frac{1}{2}*\frac{{({e_u} + {k_u}) - \mathop {\min }\nolimits_{i \in V} ({e_i} + {k_i})}}{{\mathop {\max }\nolimits_{j \in V} ({e_j} + {k_j}) - \mathop {\min }\nolimits_{i \in V} ({e_i} - {k_i})}}. \tag{1}
\end{equation}
$$
其中$e_u$表示包含节点u的三角形数量，$k_u$表示节点u的度。

针对有权重网络$G=(V, E, w)$，定义如下：
$$
\begin{equation} 
{e_u} = \sum\limits_{x,y \in Ng(u),{e_{xy}} \in E} {\frac{{w(u,x) + w(u,y) + w(x,y)}}{3}}. \tag{2}
\end{equation}
$$
$$
\begin{equation} 
{k_u} = \sum\limits_{v \in Ng(u)} {w(u,v)}. \tag{3}
\end{equation}
$$
其中$Ng(x)$表示节点x的邻居节点集

*节点间相似性(Similarity between nodes, Sim)*
反映两个相邻节点之间的连接强度，用来衡量节点的影响，该函数是由Jaccard相似度函数<sup>[2]</sup>改进得来
$$
\begin{equation} 
Sim(u,v) = \frac{{s(u,v)}}{{\sqrt {\sum\nolimits_{x \in Ng(u)} {s(u,x)} *\sum\nolimits_{y \in Ng(v)} {s(v,y)} } }} \tag{4}
\end{equation}
$$
$$
\begin{equation} 
s(i,j) = \sum\limits_{|p| = 1}^\alpha {\frac{{{{({A^{|p|}})}_{ij}}}}{{|p|}}} . \tag{5}
\end{equation}
$$
其中，p表示直接或间接连接节点i和节点j的路径，$|p|$表示p的长度，在1到$\alpha$之间，对于无权重网络，$A^{|p|}=1$；对于有权重网络，$A^{|p|}=$具体见论文。

*邻居节点影响*
表示邻居节点v对于节点u的影响：
$$
\begin{equation} 
NN{I_v}(u) = \sqrt {NI(v)*\frac{{Sim(v,u)}}{{\mathop {\max }\nolimits_{h \in Ng(u)} Sim(h,u)}}} . \tag{6}
\end{equation}
$$
使用NNI来衡量邻居节点对需要更新标签的节点的不同影响，使节点标签更新更加合理。NI和Sim较大的邻居节点对需要更新标签的节点影响较大。

**标签更新策略**
标签更新策略如下所示：
1. 基于每个节点的NI对网络中的节点进行升序排序，得到节点序列$vQueue$.
2. 对于每个结点$u \in vQueue$，在更新其标签时，从邻居节点收集多个优势标签，得到标签集$L_{Ng}$:
$$
\begin{equation} 
{L_{Ng}} = \{ l({c_1},{b_1}),l({c_2},{b_2}),...l({c_v},{b_v})\}, v \in Ng(u). \tag{7}
\end{equation}
$$
其中$l({c_1},{b_1}$表示邻居节点v的优势标签，$b_v$标识节点v属于社团c的隶属系数。
3. 基于NNI和$L_{Ng}$计算节点u对于社团c新的隶属系数$b'(c,u)$
$$
\begin{equation} 
{b^{\prime}}(c,u) = \frac{{\sum\nolimits_{l({c_v},{b_v}) \in {L_{Ng}},v \in Ng(u),{c_v} = c} {b({c_v},v)*NN{I_v}(u)} }}{{\sum\nolimits_{l({c_v},{b_v}) \in {L_{Ng}},v \in Ng(u)} {b({c_v},v)*NN{I_v}(u)} }}. \tag{8}
\end{equation}
$$
新的标签集L'产生：
$$
\begin{equation} 
{L^{\prime}} = \{ l({c_1},b_1^{\prime}),l({c_2},b_2^{\prime}),...l({c_{|{L^{\prime}}|}},b_{|{L^{\prime}}|}^{\prime})\}, \sum\limits_{l(c,b^{\prime}) \in {L^{\prime}}} {{b^{\prime}}(c,u)} = 1 \tag{9}
\end{equation}
$$
4. 自适应删除满足$b'(c,u)<1/|L'|$条件的无用标签，剩下的标签形成L''
5. 归一化L''的隶属系数得到$L_u$：
$$
\begin{equation} 
b_{(c,u)}^{\prime\prime} = \frac{{b_{(c,u)}^{\prime}}}{{\sum\nolimits_{l(c,{b^{\prime}}) \in {L^{\prime\prime}}} {b_{(c,u)}^{\prime}} }},\,\sum\limits_{l(c,b^{\prime\prime}) \in {L^{\prime\prime}}} {b_{(c,u)}^{\prime\prime}} = 1. \tag{10}
\end{equation}
$$
6. 将$L_u$中最大隶属系数的标签确定为节点u的优势标签。
在此，作者提出了一种历史标签优选策略，进一步降低了标签的随机性，提高了群体检测的稳定性。也就是说，如果有多个具有最大归属系数的标签，其中一个是上一个迭代中的主导标签，则选择这个主导标签作为当前迭代的主导标签，否则随机选择一个作为主导标签。

**LPANNI算法**
LPANNI算法从两个方面增强了LPA算法的精度和稳定性。
1. 它根据节点的NI对升序序列中的节点进行排序，并根据这个序列而不是随机序列更新节点标签。
2. 认为不同的邻居节点对更新节点的影响是不同的，这种影响不仅与邻居节点的重要性有关，还与更新节点与其邻居节点的相似性有关。NI和Sim较大的邻居节点对更新节点的影响较大。

在LPANNI标签传播的迭代过程时，将具有多个标签的节点视为重叠节点，检测出重叠的社团结构。

收敛条件：
一般的收敛条件是所有的节点标签不再改变。然而，在某些网络中，这一条件可能无法满足，即使经过数百次迭代，算法也无法完成。因此，LPANNI使用的收敛条件是标签集的大小和所有节点的优势标签都是稳定的，或者达到了指定的最大迭代数。

!["LPANNI-algo"]()


#### 数据来源

#### 实验验证及分析

#### 讨论与展望

#### 心得体会

#### 参考
[1] https://www.cnblogs.com/LittleHann/p/10699988.html

[2]  https://blog.csdn.net/jianbinzheng/article/details/82892001 