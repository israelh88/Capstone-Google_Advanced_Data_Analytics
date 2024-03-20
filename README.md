# Capstone Project for Google Advanced Data Analytics Professional Certificate
# Salifort Employee Retention Project

## Project Overview
The intended audience for this project is the Salifort Human Resources & Leadership Team.  In this scenario, Human Resources has requested that a data analyst review the results from an employee satisfaction survey to develop a model that predicts employees will leave the company.  They have also requested an executive summary.

For this task, I used XGBoost binary classification as an excellent choice in turnover prediction for improved employee retention and reduced cost of re-training new hires.  

Follow this link for my [Executive Summary](https://github.com/israelh88/Capstone-Google_Advanced_Data_Analytics/blob/e54706bb2a0d4bb5366cadcbd4b7f4a514523b81/Salifort%20Capstone%20Project%20-%20Executive%20Summary%20ih.pdf) for this project.

## Table of Contents
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning](#data-cleaning) 
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Preparation for Modeling](#data-preparation-for-modeling)
- [Modeling: XGBoost Binomial Classification](#modeling-xgboost-binomial-classification)
  - [Model Results on Training Data](#model-results-on-training-data)
  - [XGBoost Predict on Validation Data](#xgboost-predict-on-validation-data)
  - [XGBoost Predict on Test Data](#xgboost-predict-on-test-data)
- [Conclusion and Insights](#conclusion-and-insights)
- [Recommendation and Next Steps](#recommendation-and-next-steps)
  
### Data Sources
- A CSV file was provided in a Jupyter Notebook for the capstone project by Google/Coursera.
- Coursera obtained the data from [Kaggle.](https://www.kaggle.com/datasets/mfaisalqureshi/hr-analytics-and-job-prediction?select=HR_comma_sep.csv)
  
### Tools
- **Python**: For scripting and running the data analysis.
- **Pandas**: Used for data manipulation and analysis.
- **NumPy**: Employed for numerical computing.
- **Matplotlib/Seaborn**: For creating visualizations.
- **Scikit-learn**: Utilized for machine learning model building.
- **XGBoost**: For XGBClassifier and Feature Importance Plot

### Data Overview
- The data file has 15000 rows (including a header) and 10 columns.
- Column 'Left' is the target, while the other 9 columns are features.

![columns_description](https://github.com/israelh88/Capstone-Google_Advanced_Data_Analytics/blob/852294b8c9eb1ffc406ab781423ea0beb2029259/images/Screenshot%202024-03-03%20155159.png)

### Data Cleaning
- Executed **.info()** to gather basic information about the data
- Executed **.describe()** for descriptive statistics
- Renamed columns and corrected spelling, changed names to snake_case, and made them more concise as needed.
  e.g., 'average_montly_hours' [sic] was changed to 'monthly_hours.'
- Checked for missing values with .isna().sum. No missing values were found.
- Removed duplicates with the method, **.drop_duplicates().**

### Exploratory Data Analysis
- Examined counts of employees who left vs those who stayed.
  - In this data set, 16.6% of employees left, 83.4% stayed.
  - This ratio is approximately the same when grouped by Department.
  
### Data Preparation for Modeling
- Converted categorical columns into numeric columns.
  - Changed Salary from low, medium, high to 0, 1, 2.
  - Converted Department names into numeric values with pd.get_dummies.

### Modeling
- Set column 'left' as the Target, y.  "Left" means the employee left the company.
- All other columns are features and were set as X.
- Split the data into training/validate/test splits of 60/20/20
  - X_tr, X_test, y_tr, y_test and set test_size = 0.20.
  - Followed by X_train, X_val, y_train, y_val with test_size = 0.25.
- Set stratify = y due to the imbalanced data.
- Used a validation set to evaluate the model’s performance.
  
### Modeling: XGBoost Binomial Classification
- Instantiated the XGBoost classifier and set objective='binary:logistic'. 
- Tuned the following hyperparameters (cv_params). 
  - 'max_depth': [8,None], 
  - 'min_child_weight': [1,5],
  - 'learning_rate': [0.2,0.3,0.4],
  - 'n_estimators': [50, 100]
- Instantiated the GridSearchCV object xgb_cv. 
- Used refit = 'recall' to minimize false negatives--predicting an employee stayed when they left.

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
  - Recall Score = 0.9171

### XGBoost Predict on Test Data
  - xgb_test_preds = xgb_cv.best_estimator_.predict(X_test)
  - Recall Score = 0.9271
 
## Conclusion and Insights
XGBoost classifier is an excellent model for predicting employee turnover, with the champion **recall score** of **0.9271.** This is illustrated in the Confusion Matrix--the model predicted incorrectly that 29 employees stayed, while 369 were correctly labeled as leaving. 
#### Confusion Matrix
!["SalifortCapstoneProjectConfusionMatrix"](https://github.com/israelh88/Capstone-Google_Advanced_Data_Analytics/blob/main/images/Screenshot%202024-02-28%20020321.png?raw=true)

#### Feature Importance Plot
The Feature Importance plot ranks the features based on how much they contribute to the model’s predictions, however, the plot does not show the direction of their impact. For example, given that **monthly_hours worked** is a top 5 feature, how is turnover impacted across the range of hours worked in the data set? My curiosity led me to a Google search digging deeper into Feature Importance, which led me to **"Partial Dependence Plot"** Although partial dependence was beyond the course’s scope, I gained insight into it independently, inspired by the course’s advice to seek knowledge outside the curriculum.

!["SalifortCapstoneProjectFeatureImportance](https://github.com/israelh88/Capstone-Google_Advanced_Data_Analytics/blob/main/images/Screenshot%202024-02-28%20154746.png?raw=true)

#### Partial Dependence Plot of monthly_hours
Partial dependence is a way of measuring how the predicted outcome changes when the value of a feature is varied. It was a big revelation how turnover varies by monthly_hours worked--overworked and underworked employees tend to leave the company. 

Given that a standard workweek consists of 40 hours, this is approximately 173 hrs per month.  
  - Salifort employees are most likely to leave when averaging 250 hours per month, which equates to approximately 63 hours per week.
  - There is a tendency for employees to leave when working 135 hours per month--around 30 hours per week.”

![SalifortCapstoneProjectFeatureImportance](https://github.com/israelh88/Capstone-Google_Advanced_Data_Analytics/blob/09a7e78d86ae38d8573fd90931922327a468ca21/images/Screenshot%202024-03-03%20222559.png)

- It is not unexpected that the number of projects affects employee turnover. At 6 or more projects, there is a tendency for employees to leave the company.  More projects lead to longer hours worked.

![PDPnumber_projects](https://github.com/israelh88/Capstone-Google_Advanced_Data_Analytics/blob/545c11af34cbd8524cc832d6217774171718022c/images/Screenshot%202024-03-03%20222234.png)  

- As for employee satisfaction, it is no surprise that less employee satisfaction predicts turnover well. Refer to the partial dependence chart. Satisfaction of 0.1 or less is a definite walk.

![PDPsatisfaction](https://github.com/israelh88/Capstone-Google_Advanced_Data_Analytics/blob/5d8fbb66b05f6ef3268e43f62b82052c94af51dd/images/Screenshot%202024-03-03%20224048.png)

- As for tenure, there's something unusual about the higher tendency to leave the company after 5 years. Maybe employees choose to wait until stock options have vested before they quit.
![PDP_tenure](https://github.com/israelh88/Capstone-Google_Advanced_Data_Analytics/blob/628f775d64330ed3f478868f35b97d44795abfab/images/Screenshot%202024-03-20%20111823.png)

### Recommendation and Next Steps
- Alleviate the load on those with 6 or more projects--possibly dividing the work with those who tend to have much less than 160 monthly hours. 
- Ascertain why employees leave after 5 years of tenure.
- For employees with excellent evaluations, discuss what incentives would help retain these talented individuals.
- As for last_evaluation, find out why better-performing employees leave.  Do they find a better opportunity elsewhere? Consider offering incentives to longer-term employees.
