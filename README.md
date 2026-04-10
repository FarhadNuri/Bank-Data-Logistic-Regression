# Bank Data Logistic Regression

## Procedure

### Initial Phase

Firstly, I loaded the dataset and checked its shape and first few rows to understand how it looks like. I also checked column names, data types, and some basic statistics to understand the data better. Then I checked for problems like missing values, duplicate rows, and also the balance of “yes” and “no” in the target column.

Then I checked for problems like missing values, duplicate rows, and also the balance of yes and no in the target column. In preprocessing, I removed missing values, converted the target columns, yes and no into 1 and 0, and changed categorical columns into numbers using one-hot encoding so model can understand it. 

One hot encoding works like, if a column has text values like job = admin, student or teacher, then it creates separate columns for each like job_admin, job_student, job_teacher. If the row has admin then job_admin = 1 and others 0. So it convert text into numbers in a simple way.

For separating data, I put all columns except the target into X, and only the target column into y. So X is input data and y is output which model will try to predict.


### Dataset Splitting & Scaling

In this phase, I split the dataset into train, validation, and test sets. First, I separated 15% of the data as test set, so I can use it later for final evaluation. Then from the remaining data, I split again into training and validation sets. Training data will be used for learning and validation will be used for performance and model adjustment.

After splitting, I checked the balance of target values in train, validation, and test sets to make sure they are similar. Next, I did feature scaling using StandardScaler. 

As different features have very different values, like balance is very big but age is small. So I scaled them to similar range so model can learn properly. I fitted the scaler only on training data and then applied it on validation and test data, so model does not get any future information.

### Initial Model Training & Validation

I trained the logistic regression model using the training data. I set max iterations high so it can converge properly, and used class_weight balanced because data has more "no" than "yes". After training, I used the model to predict on validation set.

I got around 84% accuracy, but I understood that accuracy is not very reliable for imbalanced data. My precision was around 41%, which means when model says someone will subscribe, it is correct less than half time.

The F1 score was around 54%, and ROC-AUC was around 0.91. It shows the model is able to separate "yes" and "no"  quite well. From classification report, I saw that model is very strong in predicting "no" but weaker in predicting "yes" correctly, because it gives many false positives.

Overall, I understood that my model is good at finding potential subscribers but it also predicts many wrong yes cases. This happens because I used class_weight balanced, so model tries more to not miss actual yes cases.


### Threshold Tuning and Model Analysis

In this phase, I tried to improve my model by adjusting the decision threshold instead of using default 0.50. I tested different threshold values like 0.55, 0.60, up to 0.85 and observed how precision, recall and F1 score changes. I saw that when I increase threshold, precision increases but recall decreases. Then I created a full table of thresholds to find the best value instead of guessing.

From the results, I found that F1 score was slowly increasing and reached highest around 0.76 threshold, but the improvement was very small compared to original model. Even after trying many thresholds, F1 was stuck around 0.55 range. This showed that threshold tuning alone is not enough to improve the model much.

So I understood that my logistic regression model has reached its limit on this dataset. It is already doing good in terms of ROC-AUC, but cannot improve much more in F1 score. This means the problem is not with threshold or parameters, but with the model itself.

### Output Analysis

### Before Threshold Tuning — Base Model

Validation Set Results (Default Threshold 0.50)

Before applying threshold tuning, the baseline logistic regression model gave good performance in terms of ROC-AUC with around 0.90, which means it can separate yes and no quite well. But precision for Yes class was low around 41%, so when the model predicts someone will subscribe, it is correct less than half of the time. On the other side, recall was high around 81%, so it is able to catch most of the real subscribers.


| Class | Precision | Recall | F1-Score | Support |
|-------|-----------|--------|----------|---------|
| No | 0.97 | 0.84 | 0.90 | 5973 |
| Yes | 0.41 | 0.81 | 0.54 | 791 |
| **Accuracy** | | | **0.84** | **6764** |
| Macro Avg | 0.69 | 0.82 | 0.72 | 6764 |
| Weighted Avg | 0.90 | 0.84 | 0.86 | 6764 |

> **ROC-AUC Score: 0.9068**

The overall accuracy was around 84%, which looks good but actually misleading because of class imbalance. Even a simple model that always predicts No can get around 88% accuracy, so accuracy alone is not reliable here. The main problem of the model was low precision for Yes class. This happens because I used class_weight balanced, so model tries more to not miss real subscribers, which increases recall but also increases false positive predictions.

### After Threshold Tuning

To improve precision without retraining, I applied threshold tuning on logistic regression model. By default it predicts Yes when probability is 0.50 or more, but I increased this threshold so the model only predicts Yes when it is more confident. This helped reduce false alarms and improved precision.

I tested different threshold values and found that F1 score was highest at around 0.76 with value near 0.56. Below this threshold, the model was predicting too many false Yes, and above this it started missing real subscribers. So 0.76 was the best balance between precision and recall.

### Confusion Matrix Analysis

For validation set with threshold 0.76, the model correctly predicted most of the No cases and a good number of Yes cases. It predicted around 5537 No correctly and 473 Yes correctly. But there were still some errors, around 436 No cases were wrongly predicted as Yes and 318 real Yes were missed. 

For the test set, the results were very similar. The model correctly predicted around 5610 No and 467 Yes cases. Around 379 No were wrongly predicted as Yes and 326 real subscribers were missed. The overall pattern is almost same as validation set, with only small differences. 


### Summary

The logistic regression model was able to predict term deposit subscription quite well with final test accuracy around 89.6% and ROC-AUC around 0.90. It means most of the time the model ranks real subscriber higher than non-subscriber.


| Metric | Value | 
|--------|-------|
| Final Accuracy | 89.60% | 
| Precision (Yes) | 0.5523 | 
| Recall (Yes) | 0.5890 | 
| F1 Score (Yes) | 0.5565 | 
| ROC-AUC | 0.9068 | 
| Optimal Threshold | 0.76 | 


After applying threshold tuning from 0.50 to around 0.76, precision improved a lot and overall accuracy also increased. Recall dropped a bit but it is still acceptable for this problem. Also the validation and test results are very similar, which shows the model is stable and not overfitting. Overall, the model is performing well and ready to use for this task.