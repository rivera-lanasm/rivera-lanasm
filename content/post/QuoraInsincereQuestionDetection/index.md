---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Quora Insincere Question Detection"
subtitle: "I always lie."
summary: "hello"
authors: []
tags: [tensorflow2, anomoly detection]
categories: []
date: 2020-05-17T18:16:15-04:00
lastmod: 2020-05-17T18:16:15-04:00
featured: false
draft: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---


```python
# set working directory to parent
import os
os.chdir("..")

# import packages
import pandas as pd
pd.options.display.max_colwidth = 100


import boto3
import numpy as np
from sklearn.model_selection import train_test_split

# tensorflow
import tensorflow as tf
import tensorflow_hub as hub
from tensorboard import notebook
%load_ext tensorboard

from src.data import process_data 

# check tf version
print("TF Version: ", tf.__version__)

```

    TF Version:  2.0.0


## Quora Insinceere Question Classification 

[Kaggle Competition](https://www.kaggle.com/c/quora-insincere-questions-classification/notebooks)

This project is meant to help me explore some of the theory behind neural network models, as well as the methodologies behind designing their architectures and training them. 

Here is an excerpt from the problem statement from the competition site:

"An existential problem for any major website today is how to handle toxic and divisive content. Quora wants to tackle this problem head-on to keep their platform a place where users can feel safe sharing their knowledge with the world...**A key challenge is to weed out insincere questions -- those founded upon false premises, or that intend to make a statement rather than look for helpful answers**... Help Quora uphold their policy of “Be Nice, Be Respectful” and continue to be a place for sharing and growing the world’s knowledge."



## Topics

All of the approaches in this notebook will involve neural networks written in the Tensorflow 2 framework. In the table of contents is a list of approaches and methodologies that will be tested across the different models I implement, roughly in the order in which they appear in the pipeline. 


## Approaching Imbalanced Data:

The first thing you notice about this data set is the imbalance in class proportions. The size of the data set is about 1.3 million examples, of which only 6.2% are instances of "insincere" questions. Whether this proportion represents a good estimate of the 'true' distribution of classes or an anomolous sample is not so relevant as simply understanding how to model a classifier given class imbalance. 


Below I split the full data set into a training (80%) and test (20%) set. I use Sklearn's stratified sampling method to preserve the class proportions observed in the original data set in both train and test sets.  


```python
# load data from S3
data = process_data.retrieve_training(bucket = "quora-questions", file_name = "data/train.csv")

# Use a utility from sklearn to split and shuffle our dataset
train_df, test_df = train_test_split(data, test_size=.2, random_state=42, stratify = data['target'].values)

# Measure data imbalance in training and test set 
for k,v in {"training set":train_df, "test set":test_df}.items():
    neg, pos = np.bincount(v['target'].values)
    total = neg + pos
    print('{}:\n    Total: {}\n    Positive: {} ({:.2f}% of total)\n'.format(
        k,total, pos, 100 * pos / total))
```

## Data Preprocessing

### Examples of sincere questions


```python
for sent in train_df[train_df['target'] == 0]['question_text'][1:10]:
    print(sent)
    print("=====  Next  =======")
```

### Examples of insincere questions


```python
for sent in train_df[train_df['target'] == 1]['question_text'][1:10]:
    print(sent)
    print("=====  Next  =======")
```

Thinking about data gathering processes in general, it's quite possible that phenomenon that 'naturally' display class imbalance empirically display balance, and vice versa. Knowing whether the imbalance is intrinsic or extrinsic is outside the scope of the problem here. Further, the generating process for processes, like sincere vs. insincere questions, can evolve over time as the user base or judgement standards of a platform like Quora evolves. Therefore, I try to read little into the fact that the classes display imbalance and focus on how to account for it in the context of a model.   

Given highly imbalanced data, most learners will exhibit bias towards the majority class, and in more extreme cases even ignore the minority class altogether. From a probabalistic point of view, for the learner, this often proves logical because the prior probability of the majority group often outweighs the evidence. 

In [Survey on Deep Learning with Class Imbalance](https://link.springer.com/article/10.1186/s40537-019-0192-5) from Journal of Big Data, authors Johnson and Khoshgoftaar group methods for handling class imbalance into three categories. The first, data-level techniques, attempt to reduce imbalance through resampling methods. The second, algorithm-level methods, implement a cost or weight schema on the underlying learner. Hybrid approaches combine both sampling and weighting methods. 


https://towardsdatascience.com/handling-imbalanced-datasets-in-deep-learning-f48407a0e758
https://www.tensorflow.org/tutorials/structured_data/imbalanced_data
http://203.170.84.89/~idawis33/DataScienceLab/publication/IJCNN15.wang.final.pdf
https://towardsdatascience.com/handling-imbalanced-data-4fb691e23fe9
http://di.ulb.ac.be/map/adalpozz/pdf/Racing_unbalanced_IDEAL.pdf
https://rikunert.com/SMOTE_explained


**Resampling methods to test:**
1. no resampling
2. smote

## Model Cost functions & Evaluation


notes on binary cross entropy 
Notes from [Michael Nielson's online NN guide](http://neuralnetworksanddeeplearning.com/chap3.html)

http://www.jussihuotari.com/2018/01/17/why-loss-and-accuracy-metrics-conflict/






```python

```

## Model Architecture


```python

```

## Model Hyperparameters


```python

```

## Training Strategies


```python

```

## Overfitting


```python

```

## Model Descriptions


```python

```

## Model Results


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```
