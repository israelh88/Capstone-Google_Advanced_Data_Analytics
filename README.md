# Capstone Project for Google Advanced Data Analytics Professional Certificate
## Salifort Employee Retention Project. Prepared for Salifort HR & Leadership Team

### Project Overview
HR requested that a data analyst review the results from an employee satisfaction survey to develop a model that  
can predict employees will leave the company. For this task, we used XGBoost binary classification as an excellent   
choice in turnover prediction for improved employee retention and reduced cost of re-training new hires.
### Data Sources
- csv file provided by Google for the capstone project.

### Tools
- Jupyter Notebook: Data Cleaning, Data Analysis, Modeling, and Visualizations

### Data Cleaning
- Run **.info()** to gather basic information about the data
- Run **.describe()** for descriptive statistics
- Rename columns and correct spelling, change names to snake_case, and make them more concise as needed.
  e.g., 'average_montly_hours' [sic] becomes 'monthly_hours.'
- Check for missing values
- Remove duplicates with the method, drop_duplicates()

### Exploratory Data Analysis
- Examine counts of employees who left vs those who stayed.
  - In this data set, 16.6% left, 83.4 stayed.
  - Ratio is approximately the same when grouped by Department
  
### Data Preparation
- Convert categorical columns into numeric columns.
  - Salary from low, medium, high to 0, 1, 2.
  - Department converted to numeric values with pd.get_dummies.

### Modeling
- Set column 'left' as the Target, y.
  - Set all other columns as X
- Split the data into training/test splits of 80/20 by setting test_size = 0.20.
  - X_tr, X_test, y_tr, y_test
- Set stratify = y due to imbalanced data
- Split the data into train and validate sets
  - X_train, X_val, y_train, y_val with test_size = 0.25.

### XGBoost Classifier
- Instantiated the XGBoost classifier xgb and set objective='binary:logistic'. 
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
 
### Used the XGBoost model to predict on the validation data
  - XGB_val_preds = xgb_cv.best_estimator_.predict(X_val)
  - Recall = 0.9171

### Used the XGBoost model to predict on test data
  - xgb_test_preds = xgb_cv.best_estimator_.predict(X_test)
  - Recall = 0.9271
 
### Summary of Model Results
### Confusion Matrix
!"SalifortCapstoneProjectConfusionMatrix"https://github.com/israelh88/Capstone-Google_Advanced_Data_Analytics/blob/main/images/Screenshot%202024-02-28%20014809.png?raw=true

