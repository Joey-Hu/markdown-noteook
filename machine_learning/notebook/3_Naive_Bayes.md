## 朴素贝叶斯

朴素贝叶斯是一种基于概率的预测，其核心是贝叶斯法则：
$$
\begin{equation}
P(H|X) = \frac{P(X|H)P(H)}{P(X)}
\end{equation}. \tag{1}
$$

具体思想如下所示：

1. 设D是训练元组和它们相关联的类标号的集合。每个元组用一个n维属性向量X={$x_1, x_2, ..., x_n$}表示，描述n个属性$A_1, A_2, ..., A_n$对元组的n个测量。
2. 假定有m个类$C_1, C_2, ..., C_m$。给定元组X，分类法将预测X属于具有最高后验概率的类(在条件X下)。即，朴素贝叶斯分类法预测X属于$C_i$，当且仅当
$$
\begin{equation}
P(C_i|X)>P(C_j|X)  \qquad  i\leq j \leq m, j \neq i
\end{equation}. \tag{2}
$$
最大化$P(C_i|X)$。$P(C_i|X)$最大的类$C_i$称为最大后验假设。根据贝叶斯定理，
$$
\begin{equation}
P(C_i|X)=\frac{P(X|C_i)P(C_i)}{P(X)}
\end{equation}. \tag{3}
$$

3. 由于对于所有类P(X)为常数，所以**只要$P(C_i|X)P(C_i)$最大**即可，又因为各特征属性是条件独立的，所以有：
$$
\begin{equation}
P(X|C_i)P(C_i)=P(C_i)\prod_{j=1}^mP(a_j|C_i)
\end{equation}. \tag{4}
$$

为避免零概率值，可以使用拉普拉斯校准(假设训练数据库很大，对每个计数加一造成的估计概率的变化可以忽略不计)。

#### 举例
!["example"](https://raw.githubusercontent.com/Joey-Hu/markdown-noteook/master/machine_learning/images/naive_bayes/naive_bayes_example.png)
!["example_2"](https://raw.githubusercontent.com/Joey-Hu/markdown-noteook/master/machine_learning/images/naive_bayes/naive_bayes_example_2.png)


