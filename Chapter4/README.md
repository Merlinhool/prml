前一章中，我们研究了一类有相当简单的解析和计算性质的回归模型。现在，我们讨论一类与此相似的用于解决分类问题的模型。分类的目的是是将输入变量$$ x $$分到$$ K $$个离散的类别$$ C_k,k=1,...K $$中的一类。一般情况下，类别是互不相交的，因此每个输入被分到唯一的一个类别中。输入空间被决策边界（decision boundary）或决策面（decision surface）划分成不同的决策区域（decision region）。本章，我们考虑线性分类模 型。所谓线性分类模型是指决策面是输入向量$$ x $$的线性函数，因此它定义了$$ D $$维输入空间中的$$ (D − 1) $$维超平面。如果数据集可以被线性决策面精确地分类，那么我们说这个数据集是线性可分的（linearly separable）。    

对于回归问题，我们希望预测的变量$$ t $$是一个实数向量。在分类问题中，有很多不同的方式来表达目标变量的标签。对于概率模型，二分类问题最方便的表达方式是二元表示方法。这种方法中，目标变量$$ t \belong \{0, 1\} $$，其中$$ t = 1 $$表示类别$$ C_1 $$，而$$ t = 0 $$表示类别$$ C_2 $$。我们可以把$$ t $$的值解释为分类结果为$$ C_1 $$的概率，其中概率只取极限值$$ 0, 1 $$。对于$$ K > 2 $$的情况，比较方便的方法是使用“1-of-K”编码规则。这种方法是：$$ t $$是一个长度为$$ K $$的向量。如果类别为$$ C_j $$，那么$$ t $$的所有元素$$ t_k $$，除了$$ t_j $$等于1，其余的都等于0。例如，如果我们有5个类别，那么来自第2个类别的模式给出的目标向量为    

$$
t = (0,1,0,0,0)^T \tag{4.1}
$$

同样的，我们也可以把$$ t_k $$的值解释为分类为$$ C_k $$的概率。对于非概率模型，目标变量使用其它表示方法有时候会更方便。    

在第1章，我们提出了三种不同的解决分类问题的方法。最简单的方法是构造一个直接把向量$$ x $$分到具体的类别中判别函数（discriminant function）。但是，一个更强大的方法是在推断阶段对条件概率分布$$ p(C_k|x) $$进行建模，然后使用这个概率分布进行最优决策。正如1.5.4节讨论的那样，分离推断和决策两个阶段，可以获得了很多有益的东西。有两种不同的方法来确定条件概率$$ p(C_k|x) $$。一种是直接对它建模，例如把条件概率分布表示为参数模型，然后使用训练集来最优化参数。另一种是生成式的方法。在这种方法中，我们对类条件概率密度$$ p(x|C_k) $$以及先验概率分布$$ p(C_k) $$建模，然后使用贝叶斯定理来计算需要的后验概率分布

$$
p(C_k|x) = \frac{p(x|C_k)p(C_k)}{p(x)} \tag{4.2}
$$

我们将在本章中讨论这三种方法的例子。    

在第3章讨论的线性回归模型中，模型的预测$$ y(x,w) $$是由参数$$ w $$的线性函数给出的。在最简单的情况下，模型对输入变量也是线性的，因此形式为$$ y(x) = w^Tx + w_0 $$，即$$ y $$是一个实数。然而对于分类问题，我们想预测的是离散的类别标签，或更一般地，预测位于区间$$ (0, 1) $$的后验概 率。为了达到这个目的，我们考虑使用非线性函数$$ f(\dot) $$对$$ w $$的线性函数进行变换，来推广这个模型，即

$$
y(x) = f(w^Tx + w_0) \tag{4.3}
$$

在机器学习的文献中$$ f(\dot) $$被称为激活函数（activation function），而它的反函数在统计文献中被称为链接函数（link function）。决策面对应于$$ y(x) = constant $$，即$$ w^T x + w_0 = constant $$，因此决策面是$$ x $$的线性函数，即使函数$$ f(\dot) $$为非线性函数也是如此。因此，由式（4.3）描述的这一类模型被称为广义线性模型（generalized linear model）（McCullagh and Nelder, 1989）。但是，需要注意，与回归中使用的模型相反，引入了非线性函数$$ f(\dot) $$后，它们不再是关于参数线性的。这会导致比线性回归模型更加复杂的分析及计算。尽管这样，这些模型仍然比后续章节中讨论的更加一般的非线性模型来得简单。     

本章中讨论的算法，在使用一个基函数$$ \phi(x) $$向量（就像第3章的回归模型中那样），对输入变量进行固定的非线性变换的情况下同样适用。我们以直接在原始输入空间$$ x $$进行分类作为本章的开始。而第4.3章中，为了与后续章节的一致性，我们发现引入基函数会比较方便。