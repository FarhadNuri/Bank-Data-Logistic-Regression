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