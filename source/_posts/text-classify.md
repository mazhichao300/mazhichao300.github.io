---
layout: post
title:  "文本分类的调研"
date:   2016-07-16 23:17:03 +0800
categories: [Machine-Learning]
tags: [python,scikitlearn,jieba]
---

>最近拿到一批用户对产品的反馈数据，需要进行文本分类，找出用户的痛点，从而发掘产品的优化方向。

### 本文主要调研了：  
1. LDA主题分类模型（python gensim包）；
2. Naive Bayes;
3. SVM;


#### 首先调研了LDA主题分类模型，使用python gensim包，代码如下：

```python
import jieba
import jieba.analyse
import  os, sys
from gensim import corpora, models, similarities

train_set = []

walk = os.walk('./data') #数据放在了data目录，每条反馈数据放在了单独的一个文件中

jieba.load_userdict("userdict.txt")

stopwords = [line.strip().decode('utf-8') for line in open('./stopwords.txt')]
stopwords.append(' ')


for root, dirs, files in walk:
	for name in files:
		f = open(os.path.join(root, name), 'r')
		raw = f.read()
		word_list = filter(lambda _: _ not in stopwords, jieba.cut(raw, cut_all = False))
		train_set.append(word_list)

num = 20  #提取20个主题

dic = corpora.Dictionary(train_set)
corpus = [dic.doc2bow(text) for text in train_set]
tfidf = models.TfidfModel(corpus)
corpus_tfidf = tfidf[corpus]
lda = models.LdaModel(corpus_tfidf, id2word = dic, num_topics = num)
corpus_lda = lda[corpus_tfidf]

for i in range(0, num):
	print lda.print_topic(i)

```
结果： 提取出来的主题很难解释，效果不怎么好。

----

#### 接下来调研了有监督学习,Naive Bayes和SVM，代码实现如下（使用python sklearn）
```python
import jieba
import jieba.analyse
import  os, sys
import sklearn.feature_extraction
import sklearn.naive_bayes as nb
import sklearn.externals.joblib as jl
from sklearn import metrics
import xlrd

reload(sys)
sys.setdefaultencoding('utf8') 

def calculate_result(actual,pred):  
    m_precision = metrics.precision_score(actual,pred);  
    m_recall = metrics.recall_score(actual,pred);  
    print 'predict info:'  
    print 'precision:{0:.3f}'.format(m_precision)  
    print 'recall:{0:0.3f}'.format(m_recall);  
    print 'f1-score:{0:.3f}'.format(metrics.f1_score(actual,pred));  


train_set = []

jieba.load_userdict("userdict.txt")

stopwords = [line.strip().decode('utf-8') for line in open('./stopwords.txt')]
stopwords.append(' ')

kvlist = []
targetlist = []

gnb = nb.MultinomialNB(alpha = 0.01)
fh = sklearn.feature_extraction.FeatureHasher(n_features=15000,non_negative=True,input_type='string')

# get train data, 标记的训练数据，excel中每行一条数据：第一列为需要分类的文本数据，第二行为人工标记的分类
data = xlrd.open_workbook('./data/train.csv.xls')
table = data.sheets()[0]
nrows = table.nrows
ncols = table.ncols

for r in range(1, nrows):
	line = table.row_values(r)
	if line[1] != '':
		targetlist += [int(line[1])]
		wordlist = filter(lambda _: _ not in stopwords, jieba.cut(line[0], cut_all = False))
		kvlist += [ [ i for i in wordlist ] ]

# get test data，标记的测试数据，格式同训练数据
testlist = []
testtargetlist = []
data = xlrd.open_workbook('./data/test.xls')
table = data.sheets()[0]
nrows = table.nrows
ncols = table.ncols

for r in range(1, nrows):
	line = table.row_values(r)
	if line[1] != '':
		testtargetlist += [int(line[1])]
		wordlist = filter(lambda _: _ not in stopwords, jieba.cut(line[0], cut_all = False))
		testlist += [ [ i for i in wordlist ] ]

#验证模型的准确率，召回率
print '*************************\nNB\n*************************'  
X = fh.fit_transform(kvlist)
testX = fh.fit_transform(testlist)

gnb.fit(X,targetlist)
result = gnb.predict(testX)

calculate_result(testtargetlist, result)

from sklearn.svm import SVC 
print '*************************\nSVM\n*************************'  
svclf = SVC(kernel = 'linear')#default with 'rbf'  
svclf.fit(X,targetlist)  
pred = svclf.predict(testX);  
calculate_result(testtargetlist,pred);

#使用训练出来的SVM模型对原始数据进行预测，原始数据格式为：每条数据单独存放在一个文本文件中
walk = os.walk('./data/2016')
for root, dirs, files in walk:
	for name in files:
		f = open(os.path.join(root, name), 'r')
		raw = f.read()
		word_list = filter(lambda _: _ not in stopwords, jieba.cut(raw, cut_all = False))
		kvlist = [ [ i for i in word_list ] ]
		X = fh.fit_transform(kvlist)
		pred = svclf.predict(X)
		print raw
		print pred
```
结果：SVM的准备率，召回率接近70%，比Naive Bayes稍微好些。

----
参考文档：

1. [scikit-learn官网文档](http://scikit-learn.org/stable/user_guide.html)
2. [应用scikit-learn做文本分类](http://blog.csdn.net/abcjennifer/article/details/23615947)
3. [使用sklearn + jieba中文分词构建文本分类器](http://myg0u.com/%E6%95%B0%E6%8D%AE%E6%8C%96%E6%8E%98/2015/05/06/use-sklearn-jieba.html)
