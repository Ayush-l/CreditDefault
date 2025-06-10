Credit Default Prediction Report

1. Objective & Problem Overview
Goal: Predict whether a customer will default on their credit card payment in the next month.


Target Variable: next_month_default (1 = default, 0 = no default).


Business Context: Minimizing defaults reduces financial risk to the bank. Misclassifications (especially false negatives) can lead to significant monetary loss.



2. Data Overview
Dataset size: (e.g., 25,247 rows × 26 columns)


Key features:


Demographic: sex, age, marriage, education


Credit Behavior: LIMIT_BAL, PAY_0 to PAY_6, BILL_AMT1 to BILL_AMT6, PAY_AMT1 to PAY_AMT6


Engineered: AVG_BILL_AMT, PAY_TO_BILL_RATIO



3. Data Preprocessing
Handled missing values of age using correlation between the fields (linear interpolation).


All the variables are already in numeric form, no need for encoding.


Scaled features are not necessarily required as we are using XGBoost and ADABoost.


Created new financial features:


payment_delay_score= Weighted average of all the delays.


bill_score= Weighted average of all the bills.


payment_score= Weighted average of all the pay_amt variables.




4. Exploratory Data Analysis (EDA)
Demographics:


Default is more likely among young and single users.


Behavioral patterns:


Customers with high PAY_0 to PAY_6 values have higher default rates.


Visuals:


Histograms, boxplots of default vs. non-default groups.




Correlation matrix and heatmap.




5. Financial Insights
Delinquency Streaks: Long sequences of overdue payments strongly predict default.


Minimum Payments: Users making only minimum payments (PAY = 0) often become defaulters later.


Overpayment behavior: Linked to very low or no risk.



6. Handling Class Imbalance
Detected imbalance in next_month_default (81% no default, 19% default).


Applied:


SMOTE (Synthetic Minority Oversampling Technique) to increase the number of datapoints with next_month_default=1



7. Modeling Approach
Models Tested:


Logistic Regression (baseline) -> As there is no strong correlation between next_month_default and any other field hence we didn’t use Logistic Regression.


XGBoost and AdaBoost (best performer) -> Captures non-linear complex relationships along with their robustness to outliers and noisy data and can handle mixed data values(categorical and numerical).
Hyper Parameters are tuned using Optuna for better outcomes.
Ensemble technique is used with the Logistic Regression meta model for more accurate prediction of defaulters.



8. Evaluation Results (Train/Test Split)
	
XGBoost
Accuracy-> 0.85
F1-Score
	0.86 for class 0
	0.84 for class 1
F2-Score-> 0.81
Confusion Matrix

AdaBoost
Accuracy-> 0.79
F1-Score
0.79 for both class 1 and 0
F2-Score-> 0.78
Confusion Matrix

OverAll Model
Accuracy-> 0.84
F1-Score
0.84 for both class 0 and 1.
F2-Score-> 0.84
Confusion Matrix


9. Classification Threshold Tuning
Default threshold of 0.5 resulted in high false negatives.


Final threshold: 0.1 for better recall with acceptable precision



10. Business Implications
False Negatives (high-risk customers predicted as low-risk):


Costly for the bank; leads to defaulted loans.


False Positives (safe customers denied credit):


Lost business and customer dissatisfaction.
