# Capstone Project for Google Advanced Data Analytics Professional Certificate
## Salifort Employee Retention Project. Prepared for Salifort HR & Leadership Team

## Project Overview
In the scenario, Human Resources has requested that a data analyst review the results from an employee satisfaction survey to develop a model that can predict employees will leave the company. For this task, we used XGBoost binary classification as an excellent choice in turnover prediction for improved employee retention and reduced cost of re-training new hires.

Follow this link for an [Executive Summary](https://github.com/israelh88/Capstone-Google_Advanced_Data_Analytics/blob/e54706bb2a0d4bb5366cadcbd4b7f4a514523b81/Salifort%20Capstone%20Project%20-%20Executive%20Summary%20ih.pdf) of this project.

## Table of Contents
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning](#data-cleaning) 
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Preparation for Modeling](#data-preparation-for-modeling)
- [XGBoost Binomial Classification](#xgboost-binomial-classification)
- [Model Results on Training Data](#model-results-on-training-data)
- [XGBoost Predict on Validation Data](#xgboost-predict-on-validation-data)
- [XGBoost Predict on Test Data](#xgboost-predict-on-test-data)
- [Conclusion](#conclusion)
    - [Insights](#insights)
- [Recommendation and Next Steps](#recommendation-and-next-steps)
  
### Data Sources
- A CSV file for the capstone project was provided by Google/Coursera in a Jupyter Notebook.
  
### Tools
- Jupyter Notebook for the entire project including Data Cleaning, Data Analysis, Modeling, and Visualization.

### Data Cleaning
- Executed **.info()** to gather basic information about the data
- Executed **.describe()** for descriptive statistics
- Renamed columns and corrected spelling, changed names to snake_case, and made them more concise as needed.
  e.g., 'average_montly_hours' [sic] becomes 'monthly_hours.'
- Checked for missing values
- Removed duplicates with the method, .drop_duplicates()

### Exploratory Data Analysis
- Examined counts of employees who left vs those who stayed.
  - In this data set, 16.6% of employees left, 83.4% stayed.
  - This ratio is approximately the same when grouped by Department.
  
### Data Preparation for Modeling
- Converted categorical columns into numeric columns.
  - Salary from low, medium, high to 0, 1, 2.
  - Department converted to numeric values with pd.get_dummies.

### Modeling
- Set column 'left' as the Target, y.  "Left" means the employee left the company.
- All other columns are features and were set as X.
- Split the data into training/validate/test splits of 60/20/20
  - X_tr, X_test, y_tr, y_test and set test_size = 0.20.
  - Followed by X_train, X_val, y_train, y_val with test_size = 0.25.
- Set stratify = y due to imbalanced data
- Used the "validate" set to evaluate the model’s performance during training and tune hyperparameters.
  
### XGBoost Binomial Classification
- Instantiated the XGBoost classifier and set objective='binary:logistic'. 
- Tuned the following hyperparameters (cv_params). 
  - 'max_depth': [8,None], 
  - 'min_child_weight': [1,5],
  - 'learning_rate': [0.2,0.3,0.4],
  - 'n_estimators': [50, 100]
- Instantiated the GridSearchCV object xgb_cv. 
- Used refit = 'recall' to minimize false negatives (predicting an employee will stay when they, in fact, leave)

### Model Results on Training Data
- xgb_cv.fit(X_train, y_train)
- Best score with xgb_cv.best_score_ = 0.9121
- Best parameter settings with xgb_cv.best_params_
  - 'learning_rate': 0.3,
  - 'max_depth': None,
  - 'min_child_weight': 5,
  - 'n_estimators': 50
 
### XGBoost Predict on Validation Data
  - XGB_val_preds = xgb_cv.best_estimator_.predict(X_val)
  - Recall = 0.9171

### XGBoost Predict on Test Data
  - xgb_test_preds = xgb_cv.best_estimator_.predict(X_test)
  - Recall = 0.9271
 


## Conclusion
XGBoost model is an excellent model for predicting whether an employee will leave the company. The best recall score is 0.927136. There are only 29 employees identified incorrectly as staying that actually left as shown in the Confusion Matrix below.
#### Confusion Matrix
!["SalifortCapstoneProjectConfusionMatrix"](https://github.com/israelh88/Capstone-Google_Advanced_Data_Analytics/blob/main/images/Screenshot%202024-02-28%20020321.png?raw=true)

Focus on the Top 5 features in order of greatest importance shown in the plot below. 
#### Feature Importance
!["SalifortCapstoneProjectFeatureImportance](https://github.com/israelh88/Capstone-Google_Advanced_Data_Analytics/blob/main/images/Screenshot%202024-02-28%20154746.png?raw=true)

- Given that 40 hours per week is about the same as 173 hrs per month, when employees work 190 monthly hours (43-hr workweek, which is closet to a normal 40-hr work week), they are least likely to leave according to the chart above for partial dependence of feature = average monthly hours. Employees are most likely to leave when averaging 250 hours per month, which is approximately 63 hours per week on average. Similarly, there is a tendency for employees to leave when working less than 40 hours per week.”
#### Partial Dependence
![SalifortCapstoneProjectFeatureImportance](https://github.com/israelh88/Capstone-Google_Advanced_Data_Analytics/blob/main/images/Screenshot%202024-02-28%20155004.png?raw=true)

- As for employee satisfaction, it is no surprise that less employee satisfaction predicts turnover well. Refer to the partial dependence chart. Satisfaction of 0.1 or less is a definite walk.
- As for last_evaluation, better performance leads to an employee leaving, perhaps given their confidence and competence to find a better opportunity elsewhere.
- As for tenure, there's something unusual about the higher tendency to leave the company after 5 years. Perhaps, after stock options have vested.
- Also, it is not unexpected that the number of projects affects employee turnover. At 6 or more projects, there is a tendency for employees to leave the company.

### Insights

### Recommendation/Next Steps:
Find ways to reduce long workweeks (< 45 hrs per week), perhaps by looking into employees' workload, or number of projects. 
Alleviate the load on those with 6 or more projects--possibly dividing the work with those who tend to have much less than 160 monthly hours. 
Ascertain why employees leave after 5 years of tenure.
For employees with excellent evaluations, discuss what incentives would help retain these talented individuals.
