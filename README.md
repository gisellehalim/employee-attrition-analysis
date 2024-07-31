# Final Project: Solving Human Resources Problem of an Edutech Company

## Business Understanding
Jaya Jaya Maju is a multinational company established in 2000. The company has more than 1000 employees spread across the country. Large companies like Jaya Jaya Maju need data to solve various problems within the company. Data helps companies to make data-driven decisions.

### Business Problems
Jaya Jaya Maju is experiencing difficulties in managing employees. This resulted in a high attrition rate (the ratio of the number of employees who left to the total number of employees) of more than 10%. This number is quite high and will be dangerous if left unchecked. This is why Jaya Jaya Maju company needs data analysis to see what influences cause the high attrition rate. 

### Project Scope
In this project, 2 tasks will be carried out, namely:
- Thorough data analysis to see the factors that determine employee departure.
- Create a machine learning model with XGBoost algorithm to help companies predict the potential exit of an employee. The model will be created in the same file as the exploratory data analysis.

### Preparation

Data source: Data provided by Dicoding for the final project (under the company name Jaya Jaya Maju). However, the original data comes from IBM HR Analytics.

Setup environment: No environment setup was done because the work was done in Google Colab and the dashboard was created with Microsoft PowerBI.

## Business Dashboard

The business dashboard created shows the factors of employee departure from the company. The dashboard discusses several factors, such as demographics, work experience, experience in the office, salary, distance from home, and others. The content of this dashboard only shows a general comparison and is not too detailed like in Python code. This dashboard was created with Microsoft PowerBI which is a similar tool to Tableau and Looker Studio. The data used is 'employee_data_clean' (attached to the data folder) which has been cleaned so that there are no null values and the rating numbers are made to be more neat to see.

Dashboard Link:
https://app.powerbi.com/links/jTjsmmyjnf?ctid=0127957a-1dfd-4219-ba67-9031b9d3857e&pbi_source=linkShare (this dashboard link can only be accessed if you have a Microsoft PowerBI Premium account)

## Modeling
In this project, a model was built to predict the potential exit of employees from the company. This is done by training the model using employee data with their attrition status being the target variable. The model used is XGBoost without any hyperparameter settings. The XGBoost algorithm was chosen because it is efficient in handling large data, improving accuracy, and optimizing computation time.

**Stage of dividing training and testing data**
In the dataset, there is empty data in the attrition column, so null data is removed at the beginning of the dataset load.

Some columns are categorical, so they must be converted to numeric in order to use the model. The columns were labeled with mapping value using LabelEncoder.

After that, we looked at the correlation between columns in the dataset to see which column has the most influence on attrition. This was done to ensure optimal model performance by including important columns.

Then, the x variable is determined in the form of employee data columns and the y variable is the attrition status. 

The dataset was then divided into train and test with a ratio of 80:20.
The reason why this number is set is because 80/20 is generally considered good enough (unless the training data is very large, then the split data ratio can be changed). In this dataset, the number of columns is not too large so enough *training* data is needed to ensure the model is well trained.

By setting random_state = 42, the dataset will output the same random data for training data and testing data. Through this sharing configuration, we get 80% of the training data from the dataset and 20% of the testing data from the dataset.

Next, data balancing is done with the SMOTE oversampling method because the amount of data is not balanced. Data balancing is done so that the model can achieve higher accuracy. The choice of oversampling is because with little data, undersampling will further reduce the amount of data and potentially reduce accuracy. With the SMOTE oversampling method, the minority class (in this case is class “1” or leaving the company) will be added to the amount of data by creating synthesized data.

**Steps of building the XGBoost model**

1. Import the XGBoost algorithm.
2. Model training/fitting process using the previously divided x_train and y_train.
3. Predicting the testing data with the trained model.

After the model is trained and makes predictions from the testing data, the model is evaluated with several metrics that will be discussed further in the evaluation section. The evaluation results will determine whether the model is good.

## Evaluation
The model that has been created will be evaluated with various metrics, namely precision, recall, f1-score, accuracy, and roc-auc score.

In addition, confusion matrix is also used to see the value of true positive, true negative, false positive, and false negative.

**Metrics used in this project:**

Accuracy: Accuracy is the most commonly used metric to measure the performance of machine learning models. Accuracy is defined as the proportion of test data that is correctly classified by the model.

$$ Accuracy = (True Positives + True Negatives) / (Total Number of Cases) $$

Recall: Recall is a metric that measures the model's ability to detect positive cases. Recall is defined as the proportion of positive cases (or in this case, employees who left the company) that are correctly classified by the model. 

$$ Recall = True Positives / (True Positives + False Negatives) $$

Precision: Precision is a metric that measures the model's ability to detect true positive cases. Precision is defined as the proportion of positive cases classified by the model as true positive cases.

$$ Precision = True Positives / (True Positives + False Positives) $$

AUC-ROC: AUC-ROC (Area Under the Receiver Operating Characteristic Curve) is a metric that measures the performance of machine learning models on binary classification tasks. AUC-ROC is calculated by graphing the True Positive Rate (TPR) against the False Positive Rate (FPR) at various thresholds, and then calculating the area under the resulting curve.

$$ AUC-ROC = ∫_0^1 TPR(t) dt $$

F1-score: F1-score is a combination of precision and sensitivity/recall.

$$ F1-score = 2 * (Precision * Recall) / (Precision + Recall) $$

**Metric Measurement Results**

Based on the results of the metric measurements on the XGBoost algorithm, the algorithm has shown a fairly good performance in identifying both classes with an accuracy of 0.83 and precision, recall, and f1-score values that also reached 0.72. The AUC score also shows a fairly good model performance with a score of 0.72. This shows that the model is able to predict the potential attrition of an employee in general, but due to data imbalance, the model struggles to predict class “1” or attrition.

## Prediction
The model can be tested with the 'Prediction' file which will predict an employee's attrtition status with the XGBoost model that has been saved in the .pkl file. To predict, simply run the code and change the row of data you want to predict by changing the value of the variable 'row_index'. This file will show the prediction results with the actual labels.

## Conclusion
From the exploration of data analysis conducted (both from Python code and PowerBI Dashboard), there are the following insights:

- The most influential factors for attrition are young age, being in a certain department or position, distance from home, number of business trips, marital status, employee satisfaction with work (relationship with coworkers, engagement, environment, etc.), work-life balance, overtime, length of time with manager, and amount of training. Explanations will be provided in the following points.

- The number of attrition experienced by the company reached 179 employees or 16.9% of the total number of employees. This number is quite high because it can affect the company's performance if not addressed (20% is a high limit).

- The number of male and female employees is quite balanced with a percentage of 59% and 41% for each. Regarding the number of attrition, male employees tend to leave the company more. But this number is not much different from female employees so gender does not affect the overall employee exit.

- The highest number of attrition comes from employees with an age range of 20 - 30 years. This can be caused because at that age, employees tend to focus on developing careers rather than stability. This can be seen from the decreasing trend of attrition the older the age.

- Attrition is most prevalent in the sales department reaching 20.7% with a difference of about 5% when compared to other departments. This could be due to the heavy workload of sales department.

- When compared by gender, the number of female employees in the human resources and sales departments who leave is higher. While male employees in the RnD department leave the company the most.

- The farther the office is from home, the more likely employees are to leave the company. This can be caused by high transportation costs and other factors.

- When viewed by gender, both men and women will be more likely to leave if the office is far from their residence, so there is no relationship with gender.

- The more business trips taken, the greater the potential for employees to leave the company. This can be caused by travel fatigue.

- Marital status is one of the factors that influence employee departure from the company, single employees leave the company the most while divorced employees leave less often. This could be due to the tendency of single employees not having dependents so they can be more free to look for other opportunities while for married or divorced employees, they already usually have dependents (e.g. spouse or children). This is not influenced by gender as for both men and women, the same trend can be seen.

- Employees who have had more work experience are more likely to leave the company, but the difference is not much with employees with less experience.

- The education level of employees does not affect their exit from the company much. Employees with an education level of category 5 do have a lower attrition rate, but this is not a trend when viewed from other categories.

- Employees' satisfaction with the work environment is one of the factors they leave the company. The less satisfied they are with the work environment, the greater their potential to leave the company.

- Feeling engaged with work is one of the factors they stay in the company. The more they contribute, the less likely they are to leave the company. This can be caused by employees' desire to grow and contribute to their respective job desks.

- Although the trend goes up and down, there is a tendency that employees with higher positions will stay with the company.

- The positions of sales representative, sales executive, human resources, laboratory technician, and research scientist experience considerable attrition compared to other positions. Attrition is highest for employees in the sales representative position which may be due to high pressure.

- Employees' satisfaction with their jobs has an influence on their chances of leaving the company. The more satisfied they are with their jobs, the more likely employees are to stay.

- Work-life balance moderately influences employees' decision to resign. The better the work-life balance, the less likely employees are to leave the company although attrition increases for employees who score 4.

- Employees' salaries (hourly or monthly) do not greatly influence their decision to leave the company.

- Employee salary increases do not greatly influence their decision to leave the company.

- Employee performance ratings do not greatly influence their decision to leave the company.

- Employees who work overtime are much more likely to leave the company. This can be caused by work fatigue.

- Employees who have less working time with their manager are more likely to leave the company. This could be due to a mismatch or not having the right chemistry between the employee and manager.

- An employee's length of time in the company does not have much influence on their chances of leaving the company.

- Length of promotion does not have much influence on the chances of employees leaving the company.

- The amount of employee training in a year does not really affect the chances of employee departure. The difference is only seen in employees who are not given training at all. If employees are given training, their chances of leaving will be smaller.

- Good relationships with coworkers have little influence on the chances of employees leaving the company. However, the better the employee's relationship with their coworkers, the less likely they are to leave (although the trend increases slightly for employees who give a score of 4).

### Recommended Action Items 
From the insights that have been obtained from the data, it can be seen that the attrition rate in the company is quite high so action is needed to overcome the problem. Some of the suggestions below can be considered to improve the company's performance:

**Tackle the Overall Attrition Rate:**
- Conduct periodic surveys to identify aspects that need improvement in the work environment. Create a positive and supportive work culture.

- Improve employee development and training programs to help them grow and feel challenged in their work. Provide quality and relevant training and development programs.
- Clarify the company's vision, mission and values to increase employees' sense of engagement. Make employees feel that their contributions result in something good for the company and themselves. Utilize reward and recognition programs to reward employee contributions. 
- Implement a flexible working hours policy.
- Support leave and other work-life balance programs.
- Limit overtime as much as possible. One way is to implement more effective and efficient workflows that minimize the need for overtime.
- Conduct market research to ensure salaries and benefits are competitive.
- Improve communication between managers and employees, and encourage cooperation and collaboration between employees. One way is to conduct team building activities for employees or provide leadership and team development training for leaders.
- Conduct a transparent and fair promotion process. Consider employee performance, experience, and skills in the promotion process. Provide opportunities for employees to grow and take a bigger role in the company.

**Handling Employee Problems in the Sales Department:**
- Reduce sales workload by improving sales process efficiency.

- Increase commissions and incentives for outstanding sales.

**Help Remote Employees:**
- Provide transportation allowance or other compensation to help remote employees.

- Improve communication and collaboration technologies to help remote employees stay connected with their teams.
- Limit business travel to essentials to minimize employee fatigue and use of company resources.

**Retain employees with a lot of work experience:**
- Offer challenging and impactful career development programs for employees with extensive work experience.
- Provide opportunities for employees with high work experience to take on leadership roles within the company.

**Improve Employee Attrition Rates in Specific Positions:**
- Conduct further analysis to identify specific reasons for attrition in certain positions, such as sales representative (most urgent), sales executive, human resources, laboratory technician, and research scientist.
- Develop targeted intervention programs to address these specific reasons.

