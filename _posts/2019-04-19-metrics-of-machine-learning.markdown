---
layout: post
title:  "Note: Summary of scores about evaluation of machine learning models"
date:   2019-04-19 08:43:12 +0530
categories: Note
---
This is the note of the scores which is applied to evaluate the performance of machine learning models. In this study note, it includes **Accuracy**, **Confusion Matrix**, **Precision**, **Recall**, and **F1-score**. For visualize the performance, we can use Receiver Operating Characteristic Curve (ROC curve) and Area under curve (AUC)

# Metrics applied to evaluate the performance of models

## Accuracy
- Correct: number of correct result (predicted/classified) 
- Total: number of total prediction
  
$$ Accuracy = \frac{Correct}{Total}$$

## Confusion Matrix

| |  Predicted Positive  |  Predicted Negative  |
|-------------------|-----------------|-----------------|
| Actual Positive | True Positives | False Positives |
| Actual Negative | False Negatives | True Negatives |


## Precision
Precision is the frequency of the predicted positive values which are actually correct in all positively predicted results. This metric is used when the objective is to reduce the number of false positives in the confusion matrix.

$$
Precision = \frac{ \textrm{true  positives}}{\textrm{true positives} + \textrm{false positives}}
$$

## Recall / Sensitivity

Recall is a metric that tells the frequency of the correct predictions that are positive values. This metric is used when the objective is to reduce the number of false negatives in the confusion matrix. It is also known as "sensitivity" or the "true positive rate" (TPR).

$$
recall = \frac{\textrm{true positives}}{\textrm{true positives + false negatives}}
= \frac{\textrm{false positives}}{\textrm{Number of real positive  samples}}
$$

## False positive rate (FPR) / Specificity
Usage: y axis in ROC curve.

$$
\textrm{false positive rate} = \frac{\textrm{false positives}}{\textrm{false positives + true negatives}}
= \frac{\textrm{false positives}}{\textrm{Number of real negative  samples}}

$$

## F1-score / harmonic mean of recall and precision
Usage: When precision and recall are both used as metrics in analysing a model's performance. There should be a careful balance between precision and recall.
To make sure there is a balance between precision and recall.

$$
\textrm{F1 Score} = 2 * \frac{precision * recall}{ precision + recall} 
$$

# Visualize the performance of models

## Receiver Operating Characteristic Curve (ROC curve)
- Usage: Measuring the performance of a binary classifier
- x axis: false positive rate (FPR)
- y axis: Recall or TPR

## Area under curve (AUC) from ROC curve / average precision
- Usage: to find the area under the ROC curve. This value is usually between 0 and 1, wherein a value closer to 1 or 1 itself means that the model provides a very good classification performance.

## Precision-recall curve
- Usage: Balancing the precision recall value can be a tricky task. This trade-off can be represented using the precision-recall curve.
# Reference
https://www.studytonight.com/post/how-good-is-my-machine-learning-model-how-do-i-improve-its-performance
