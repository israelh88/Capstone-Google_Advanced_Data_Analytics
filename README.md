# Capstone Project for Google Advanced Data Analytics Professional Certificate
## Salifort Employee Retention Project. Prepared for Salifort HR & Leadership Team

### Project Overview
HR requested that a data analyst review the results from an employee satisfaction survey to develop a model that  
can predict employees will leave the company. For this task, we used XGBoost binary classification as an excellent   
choice in turnover prediction for improved employee retention and reduced cost of re-training new hires.
### Data Sources
- A CSV file for the capstone project was provided by Google/Coursera in a Jupyter Notebook.
  
### Tools
- I used a Jupyter Notebook for the entire project including the Data Cleaning, Data Analysis, Modeling, and Visualization.

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
- Split the data into training/validate/test splits of 80/20/20
  - X_tr, X_test, y_tr, y_test and set test_size = 0.20.
  - Followed by X_train, X_val, y_train, y_val with test_size = 0.25.
- Set stratify = y due to imbalanced data
- Used the "validate" set to evaluate the model’s performance during training and tune hyperparameters.
  
### XGBoost Classifier
- Instantiated the XGBoost classifier and set objective='binary:logistic'. 
- Tuned the following hyperparameters (cv_params). 
  - 'max_depth': [8,None], 
  - 'min_child_weight': [1,5],
  - 'learning_rate': [0.2,0.3,0.4],
  - 'n_estimators': [50, 100]
- Instantiated the GridSearchCV object xgb_cv. 
- Used refit = 'recall' to minimize false negatives (predicting an employee will stay when they, in fact, leave)

### Model Fit Results on Training data
- xgb_cv.fit(X_train, y_train)
- Best score with xgb_cv.best_score_ = 0.9121
- Best parameter settings with xgb_cv.best_params_
  - 'learning_rate': 0.3,
  - 'max_depth': None,
  - 'min_child_weight': 5,
  - 'n_estimators': 50
 
### Applied the XGBoost model to predict on the validation data
  - XGB_val_preds = xgb_cv.best_estimator_.predict(X_val)
  - Recall = 0.9171

### Applied the XGBoost model to predict on the test data
  - xgb_test_preds = xgb_cv.best_estimator_.predict(X_test)
  - Recall = 0.9271
 
### Summary of Model Results
It is no surprise that less employee satisfaction is a predictor of turnover. See the partial dependence chart below. Satisfaction of 0.1 or less is a definite walk.
Given that 40 hours per week is about the same as 173 hrs per month, when employees work 190 monthly hours (43-hr workweek, which is closet to a normal 40-hr work week), they are least likely to leave according to the chart above for partial dependence of feature = average monthly hours. At 250 average monthly hrs, which is ~63 hrs per week, employees are most likely to leave. Also, for lighter weeks at < 40 hrs, the employees tend to leave as well.

As for last_evaluation, better performance leads to an employee leaving, perhaps given their confidence and competence to find a better opportunity elsewhere.

Also, it is not unexpected that the number of projects affects employee turnover. At 6 or more projects, there is a tendency for employees to leave the company.

As for tenure, there's something unusual about the higher tendency to leave the company after 5 years. Perhaps, after stock options have vested.

### Feature Importance
!["SalifortCapstoneProjectFeatureImportance](https://github.com/israelh88/Capstone-Google_Advanced_Data_Analytics/blob/main/images/Screenshot%202024-02-28%20154746.png?raw=true)

### Confusion Matrix
!["SalifortCapstoneProjectConfusionMatrix"](https://github.com/israelh88/Capstone-Google_Advanced_Data_Analytics/blob/main/images/Screenshot%202024-02-28%20020321.png?raw=true)

### Partial Dependence
![SalifortCapstoneProjectFeatureImportance](https://github.com/israelh88/Capstone-Google_Advanced_Data_Analytics/blob/main/images/Screenshot%202024-02-28%20155004.png?raw=true)


### Conclusion
XGBoost model is an excellent model for predicting whether an employee will leave the company. The stat scores are as follows: model recall score is 0.927136

Top 5 features in order of greatest importance. 1. satisfaction_level ('satisfaction') 2. average_monthly_hours 3. last_evaluation 4. number_project 5. tenure

Given that 40 hours per week is about the same as 173 hrs per month, when employees work 190 monthly hours (43-hr workweek, which is closet to a normal 40-hr work week), they are least likely to leave according to the chart above for partial dependence of feature = average monthly hours. At 250 ave monthly hrs, which is ~63 hrs per week, employees are most likely to leave. Also, at < 40 hrs per week, the employees tend to leave as well.

As for employee satisfaction, it is no surprise that less employee satisfaction is a predictor of turnover. See partial dependence chart below. Satisfaction of 0.1 or less is a definite walk.

As for last_evaluation, better performance leads to an employee leaving, perhaps given their confidence and competence to find a better opportunity else where.

As for tenure, there's something unusual about the higher tendency to leave the company after 5 years. Perhaps, after stock options have vested.

Also it is not unexpected that number of projects affects employee turnover. At 6 or more projects, there is a tendency for employees to leave the company.

###Recommendation/Next Steps:
Find ways to reduce long workweeks (< 45 hrs per week), perhaps by looking into employees workload, or number of projects.
Look into ways to alleviate the load on those with 6 or more projects--possible hiring more help? This in turn could help with the monthly hours worked.
Ascertain why employees leave at 5 years of enture.
For employees with excellent evaluations, discuss what incentives would help keep these talented individuals.
