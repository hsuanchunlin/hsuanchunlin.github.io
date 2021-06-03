---
layout: post
title:  "Model Selection to Prevent Over-fitting"
date:   2021-05-31 00:00:42 +0530
categories: Python scikit-learn
---

## Demo Dataset
```python
import numpy as np
from sklearn import datasets
iris = datasets.load_iris()
iris.data.shape, iris.target.shape
```

## Random split into training and test sets by using train_test_split

```python
# Import the Modules
from sklearn.model_selection import train_test_split
# Here the demo is using SVM
from sklearn import svm

# Split data randomly and pick 40% of data as test. here test_size = 0.4 means 40%
X_train, X_test, y_train, y_test = train_test_split(iris.data, iris.target, test_size=0.4, random_state=0)
#Show the splitted data
print("Train:", X_train.shape, y_train.shape)
print("Test:", X_test.shape, y_test.shape)
```
## Performing the train and test several times by cross_val_score module

```python
from sklearn.model_selection import cross_val_score
clf = svm.SVC(kernel='linear', C=1)
scores = cross_val_score(clf, iris.data, iris.target, cv=5)

for i, count in enumerate(scores):
    print("%1d : %4.2f" % (i,count))
```

## Perform GridSearchCV to find better hyper parameters

```python
cv_result = []
best_estimators = []
for i in range(len(classifier)):
    clf = GridSearchCV(classifier[i], param_grid=classifier_param[i], cv = StratifiedKFold(n_splits = 10), scoring = "accuracy", n_jobs = -1,verbose = 1)
    clf.fit(x_train,y_train)
    cv_result.append(clf.best_score_)
    best_estimators.append(clf.best_estimator_)
    print(cv_result[i])
```

