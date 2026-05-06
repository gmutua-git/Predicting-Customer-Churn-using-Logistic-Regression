# README: Customer Churn Prediction using Logistic Regression

## 1. Problem Statement
This project aims to predict customer churn for a telecommunications company using a Logistic Regression model. Customer churn is a critical business problem, as retaining existing customers is often more cost-effective than acquiring new ones. By identifying customers at risk of churning, the company can proactively implement retention strategies.

## 2. Dataset
The analysis utilizes the 'Telco Churn dataset.xlsx' which contains various customer attributes and their churn status. The dataset includes features such as:
-   `customerID`
-   `gender`
-   `SeniorCitizen`
-   `MaritalStatus`
-   `Dependents`
-   `tenure` (months)
-   `PhoneService`, `MultipleLines`
-   `InternetService`, `OnlineSecurity`, `OnlineBackup`, `DeviceProtection`, `TechSupport`, `StreamingTV`, `StreamingMovies`
-   `Contract`, `PaperlessBilling`, `PaymentMethod`
-   `InternationalPlan`, `VoiceMailPlan`, `NumbervMailMessages`
-   `TotalDayMinutes`, `TotalDayCalls`, `TotalEveMinutes`, `TotalEveCalls`, `TotalNightMinutes`, `TotalNightCalls`
-   `TotalIntlMinutes`, `TotalIntlCalls`
-   `CustomerServiceCalls`
-   `TotalCall` (sum of all call types)
-   `TotalRevenue`
-   `Churn` (Target variable: Yes/No)

## 3. Methodology

### 3.1. Data Loading and Initial Inspection
The dataset was loaded into a Pandas DataFrame. Initial inspection confirmed no missing values across the dataset.

### 3.2. Data Preprocessing
-   **Target Variable Conversion**: The `Churn` column (categorical 'Yes'/'No') was converted into a numerical format (1 for 'Yes', 0 for 'No').
-   **Categorical Variable Encoding**: All other categorical features were transformed into numerical format using One-Hot Encoding (`pd.get_dummies`). The `drop_first=True` argument was used to avoid multicollinearity.
-   **Handling Missing Values**: Any remaining rows with missing values were dropped (though none were identified initially).

### 3.3. Model Training
-   **Feature and Target Definition**: `Churn` was defined as the target variable (`y`), and all other columns were considered features (`X`).
-   **Train-Test Split**: The dataset was split into training (80%) and testing (20%) sets to evaluate the model's performance on unseen data, with `random_state=42` for reproducibility.
-   **Model Initialization**: A Logistic Regression model was initialized with `max_iter=1000` to ensure convergence, especially given the high dimensionality after one-hot encoding.
-   **Model Training**: The model was trained on the `X_train` and `y_train` datasets.

### 3.4. Model Evaluation
The model's performance was evaluated using the following metrics:
-   **Accuracy**: The proportion of correctly predicted instances.
-   **Confusion Matrix**: Provides a breakdown of true positives (TP), true negatives (TN), false positives (FP), and false negatives (FN).
-   **Precision**: Of all instances predicted as churn (positive), how many actually churned (TP / (TP + FP)).
-   **Recall (Sensitivity)**: Of all actual churn instances (positive), how many were correctly identified by the model (TP / (TP + FN)).

Initially, a default probability threshold of 0.5 was used for classification. The impact of lowering the threshold to 0.3 on these metrics was also analyzed.

### 3.5. Interpretation of Coefficients
The coefficients of the Logistic Regression model were examined to understand the impact of each feature on the probability of churn. Positive coefficients indicate an increased likelihood of churn, while negative coefficients suggest a reduced likelihood.

## 4. Key Findings and Results

### Initial Threshold (0.5):
-   **Accuracy**: 89.66%
-   **Confusion Matrix**:
    -   True Negatives (TN): 559
    -   False Positives (FP): 19
    -   False Negatives (FN): 50
    -   True Positives (TP): 39
-   **Precision**: 67.24% (67.2% of customers predicted to churn actually churned)
-   **Recall**: 43.82% (Only 43.82% of actual churners were correctly identified)

### Adjusted Threshold (0.3):
-   **Accuracy**: 88.31% (Slight decrease)
-   **Confusion Matrix**:
    -   True Negatives (TN): 523
    -   False Positives (FP): 55
    -   False Negatives (FN): 23
    -   True Positives (TP): 66
-   **Precision**: 54.55% (Decrease)
-   **Recall**: 74.16% (Significant improvement)

### Coefficient Interpretation:
-   **Positive Impact on Churn Probability**: Features like `CustomerServiceCalls` had a positive coefficient, indicating that an increase in customer service calls is associated with a higher probability of churn.
-   **Negative Impact on Churn Probability**: Features like `tenure` (long tenure) had a negative coefficient, suggesting that longer customer tenure reduces the likelihood of churn.

## 5. Conclusion
The initial model with a 0.5 threshold showed high accuracy but low recall, meaning it missed over 50% of actual churn cases. For a business problem like churn prediction, identifying as many churners as possible (high recall) is often more important than overall accuracy, even if it means a slight increase in false positives. By lowering the classification threshold to 0.3, the model achieved a significantly improved recall of 74.16%, albeit with a minor reduction in accuracy and precision. This trade-off is often acceptable or even desirable in churn prediction scenarios to enable more effective early intervention strategies.
