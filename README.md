# WIDS-Datathon
## Women in Data Science Datathon 2020

Collaborators: 
- Aarthe Jayaprakash
- Chaitanya Chaphalkar

In this data science challenge, we are focusing on predicting mortality rate for patients visiting Intensive Care Units.
Data has been provided by Harvard Privacy Lab, which included data from 130,000 visits to the ICU and it spans over a one year timeframe. The dataset has 91,713 rows and 186 columns.

Dataset link: https://www.kaggle.com/c/17807/download-all

The dataset is imbalanced with the target column having 8.63% mortality vs 91.37% survival rate.

After upsampling, we needed to handle the missing values. We did it in the following manner:

1. If the column has a single unique value it can be removed as it carries no value to our predictions. 
2. If the column has binary values, missing cells can be replaced with most frequent value(mode).
3. If the column is a category(less than 30 unique values), missing values can be replaced with most frequent value(mode).
4. If the columns are IDs they need to be excluded from training models.
5. All the remaining features were numeric(float64) and are replaced with average(mean) value.

After handling the missing values, we needed to define what columns were to be used for training the models.
We did this by calculating feature importances from a LightGBM model using goss boosting and 10,000 estimators and we did this twice and divided by two to get the mean feature importance value for each column.

We used these feature importances to delete 10 columns which had zero importance towards the target column.

Models used in predicitng mortality rate:
1. XGBoost:
  - XGBoost with multiple parameters provided us with a 0.61 final AUC and it took over 18 mins to run.
  - As a result, we decided to run LightGBM with hyper parameter tuning(using RandomizedSearchCV) for a blend of performance and   accuracy.
2. LightGBM:
  - After multiple tuning cycles, LightGBM provided us with 0.90721 final AUC and runtime was just over 9 mins.
