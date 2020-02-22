## MLN -- 机器学习笔记

#### 1. Data Understanding

!["Attributes_calsses.png"](https://raw.githubusercontent.com/Joey-Hu/markdown-noteook/master/machine_learning/images/MLN_机器学习笔记/Attributes_calsses.png)

**属性类型**

1. 标称属性（nominal attribute）：性别（male, female）、头发颜色（red, yellow, black ...）、职业（docter, engineer, ...）
2. 二元属性（binary attribute）：标称属性的一种，只有两个状态，0 or 1，true or false
	对称的二元属性：两种状态具有同等价值，并且携带相同权重，如性别（male, female）
	非对称的二元属性：两种状态的结果不是同等重要的，如 HIV 患者和不是 HIV 患者
3. 序数属性（ordinal attribute）：属性对应的值有先后顺序，但相继值之间的差是未知的。如，0-很不满意、1-不满意、2-中性、3-满意、4-很满意

标称、二元和序数属性都是**定性的**。

4. 数值属性（numeric attribute）：是可定量的可度量的量，用整数或实数表示。可以是区间标度的（interval-scaled）或比率度量的（ratio-scaled）。
5. 离散属性和连续属性

#### 2. Data Preparation

> The data preparation phase covers all activities to construct the final dataset (data that
will be fed into the modeling tool(s)) from the initial raw data. Data preparation tasks are
likely to be performed multiple times, and not in any prescribed order. Tasks include
table, record, and attribute selection as well as transformation and cleaning of data for
modeling tools..

**数据清理**
!["preprocess_data.png"](https://raw.githubusercontent.com/Joey-Hu/markdown-noteook/master/machine_learning/images/MLN_%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/preprocess_data.png)

!["data_cleaning.png"](https://raw.githubusercontent.com/Joey-Hu/markdown-noteook/master/machine_learning/images/MLN_%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/data_cleaning.png)

1. 缺失值（missing value）：忽略元组、手动填值、全局填充（全填unknown或无穷）、用中心值填充（mean、median等）、用同一类中的mean或median填充、用概率最大的值填充（最流行的方法)、回归替换
2. 噪声（noise）
2. 无效值（invalid value）
2. 主键唯一性（uniqueness）
3. 属性格式（formats）
4. 属性依赖（attribute dependencies）

**数据归一化** -- Rescale
1. Min-Max Normalization
$$
\begin{equation}
X_{norm} = \frac{X - X_{min}}{X_{max} - X_{min}}. \tag{1}
\end{equation}
$$

2. Z-score Normalization
$$
\begin{equation}
z = \frac{X - \mu}{\sigma}. \tag{2}
\end{equation}
$$
其中，$\mu$表示均值，$\sigma$表示方差

3. Decimal scaling
Scale the data by moving the decimal point of the attribute value.

**离散化数据**
分箱

**Feature Engineering**

!["feature_engineering.png"](https://raw.githubusercontent.com/Joey-Hu/markdown-noteook/master/machine_learning/images/MLN_%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/feature_engineering.png)

> **Feature Engineering is a sort of art**
> Feature engineers requires a creative combination of domain expertise and insights obtained from the data exploration step.

> This is a balancing act of finding and including informative variables while avoiding too many unrelated variables.

> Informative variables improve our result; unrelated variables introduce unnecessary noise into the model .

This process(Feature Engineering) attempts to **create additional relevant features from the existing raw features** in the data, and to increase the predictive power of the learning algorithm.

Although many of the raw data fields can be directly included in the selected feature set used to train a model, it is often the case that additional (engineered) features need to be constructed from the features in the raw data to generate an enhanced training dataset.

!["feature_engineering_example.png"](https://raw.githubusercontent.com/Joey-Hu/markdown-noteook/master/machine_learning/images/MLN_%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/feature_engineering_example.png)

**Feature Selection**
原始数据中一些特征是冗余的（redundant）或是不相关的（irrelevant）。Feature Selection 的任务就是从原始数据集中选择特征的子集，通过使用最小的特征集来表示数据中的最大方差来减少数据的维数。

Curse of dimensionality
!["curse_of_dimensionality.png"](https://raw.githubusercontent.com/Joey-Hu/markdown-noteook/master/machine_learning/images/MLN_%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/curse_of_dimensionality.png)

Approaches for Feature Selecion

1. Filter
By evaluating the correlation between each feature and the target attribute, these methods apply a statistical measure to assign a score to each feature. The features are then ranked by the score, which may be used to help set the threshold for keeping or eliminating a specific feature.

用于衡量相关性的方法：
* Pearson correlation
* Mutual information

缺陷：
However, filter methods tend to select redundant variables because they
do not consider the relationships between variables. Therefore, they are
mainly used as a pre-process method.

2. Wrapper
Wrapper methods consider the selection of a set of features as a search problem, where **different combinations** are prepared, evaluated and compared to other combinations. A predictive model is used to evaluate a combination of features and assign a score based on model accuracy.

缺陷：
虽然wrapper method为特定的模型提供了最优的特征子集，但是计算量大、耗时。

3. Embedded method
最常见的是正则化方法

!["feature_selection_approaches_summarize.png"](https://raw.githubusercontent.com/Joey-Hu/markdown-noteook/master/machine_learning/images/MLN_%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/feature_selection_approaches_summarize.png)

**Feature Selection vs Dimension Reduction**

1. Feature Selection: Feature selection methods extract a subset of original features in the data **without changing them.**

2. Feature Reduction: Dimensionality reduction methods employ engineered features that can **transform the original features and thus modify them.**

#### 3. Modeling
**A model defines the relationship between features and label (i.e. target)**

Machine learning uses a model to capture the relationship between feature vectors and some target variables within a training data set.

> “All models are wrong, but some are useful.” - George Box

!["the_process_of_ML.png"](https://raw.githubusercontent.com/Joey-Hu/markdown-noteook/master/machine_learning/images/MLN_%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/the_process_of_ML.png)

!["how_to_develop_a_model.png"](https://raw.githubusercontent.com/Joey-Hu/markdown-noteook/master/machine_learning/images/MLN_%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/how_to_develop_a_model.png)

#### 4. Some of Machine Learning Algorithms
!["some_of_ML_algo.png"](https://raw.githubusercontent.com/Joey-Hu/markdown-noteook/master/machine_learning/images/MLN_%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/some_of_ML_algo.png)

#### 5. Evaluate the model
* Accurate
* Intepretable
* Fast
* Scalable

**About the model fitting**
!["model_fitting.png"](https://raw.githubusercontent.com/Joey-Hu/markdown-noteook/master/machine_learning/images/MLN_%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/model_fitting.png)

**Model validtion strategy**
* 留出法（hold-out validation）（70~80% for train, 20~30% for test）
* k-折交叉验证（K-fold cross validation）
* Leave-one-out cross validation

**criteria or metrics to evaluate the performance of model**
1. Accuracy
2. Precision
3. Recall

!["accuracy_precision_recall.png"](https://raw.githubusercontent.com/Joey-Hu/markdown-noteook/master/machine_learning/images/MLN_%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/accuracy_precision_recall.png)

4. F Score
5. ROC
6. AUC
7. Log Loss



------------------p169