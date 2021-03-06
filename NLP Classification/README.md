# 零基础入门NLP - 新闻文本分类
此文件夹存放7月份datawhale与天池合作的[零基础入门NLP - 新闻文本分类](https://tianchi.aliyun.com/competition/entrance/531810/introduction/)整个竞赛过程中个人的方案总结思考与代码。

#### 赛题数据
赛题数据为新闻文本，并按照字符级别进行匿名处理。整合划分出14个候选分类类别：财经、彩票、房产、股票、家居、教育、科技、社会、时尚、时政、体育、星座、游戏、娱乐的文本数据。
赛题数据由以下几个部分构成：训练集20w条样本，测试集A包括5w条样本，测试集B包括5w条样本。为了预防选手人工标注测试集的情况，我们将比赛数据的文本按照字符级别进行了匿名处理。

#### 评价标准
评价标准为类别f1_score的均值，选手提交结果与实际测试集的类别进行对比，结果越大越好。

### task1.赛题理解
了解本次赛题的数据集组成，包括训练集和测试集内容与大小。

### task2.数据分析
分析新闻文本数据集的句子长度，新闻类别，字符分布统计等等，找出标点符号，在进行tf-idf时候可以跳过以提高精度。

### task3.机器学习 
利用bag of words或者tf-idf对文本数据集进行预处理，然后利用机器学习方法进行分类，datawhale给出的机器学习方法是RidgeClassifier，在训练集上划分验证集进行f1得分可以达到0.93。
我们也可以尝试GBDT,xgb与lgb结合等等方法。

### task4.深度学习之FastText
转换得到的向量维度很高，需要较长的训练实践；没有考虑单词与单词之间的关系，只是进行了统计。与这些表示方法不同，深度学习也可以用于文本表示，还可以将其映射到一个低纬空间。其中比较典型的例子有：FastText、Word2Vec和Bert。

通过Embedding层将单词映射到稠密空间，然后将句子中所有的单词在Embedding空间中进行平均，进而完成分类操作。所以FastText是一个三层的神经网络，输入层、隐含层和输出层。

FastText在文本分类任务上，是优于TF-IDF的：
- FastText用单词的Embedding叠加获得的文档向量，将相似的句子分为一类
- FastText学习到的Embedding空间维度比较低，可以快速进行训练

**如何使用验证集调参**
- 通过阅读文档，要弄清楚这些参数的大致含义，那些参数会增加模型的复杂度
- 通过在验证集上进行验证模型精度，找到模型在是否过拟合还是欠拟合

### task5.深度学习之Word2Vec
Embedding能够用低维向量对物体进行编码还能保留其含义的特点非常适合深度学习。

在训练word2vec的过程中还有很多工程技巧，用negative sampling或Hierarchical Softmax减少词汇空间过大带来的计算量。

### task6.深度学习之BERT
BERT，全称是Pre-training of Deep Bidirectional Transformers for Language Understanding。注意其中的每一个词都说明了BERT的一个特征。

Pre-training说明BERT是一个预训练模型，通过前期的大量语料的无监督训练，为下游任务学习大量的先验的语言、句法、词义等信息。

Bidirectional 说明BERT采用的是双向语言模型的方式，能够更好的融合前后文的知识。

Transformers说明BERT采用Transformers作为特征抽取器。

Deep说明模型很深，base版本有12层，large版本有24层。

总的来说，BERT是一个用Transformers作为特征抽取器的深度双向预训练语言理解模型。
