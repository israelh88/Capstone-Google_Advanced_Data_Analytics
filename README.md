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
- (list libraries??)

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
- Split the data into training/test splits of 80/20 by setting test_size = 0.2
- Set stratify = y due to imbalanced data
- Split the data into train and validate sets 

### XGBoost Details
- Instantiate the XGBoost classifier xgb and set objective='binary:logistic'. Also set the random state.
- Tune the following hyperparameters (cv_params). 
  - max_depth
  - min_child_weight
  - learning_rate
  - n_estimators
- Define a set scoring of scoring metrics for grid search to capture (precision, recall, F1 score, and accuracy).
- Instantiate the GridSearchCV object xgb_cv. Pass to it as arguments:
  - estimator=xgb
  - param_grid=cv_params
  - scoring=scoring
  - cv: define the number of cross-validation folds you want (cv=4 for this project)
- Use refit = 'recall' to minimize false negatives (predicting an employee will stay when they leave)

 ### Fit the model to train the data
- xgb_cv.fit(X_train, y_train)
- Get the best score: xgb_cv.best_params_
  - Result = 0.9121
- Get the best parameter settings with xgb_cv.best_params_
  - 'learning_rate': 0.3,
  - 'max_depth': None,
  - 'min_child_weight': 5,
  - 'n_estimators': 50
 
  
