# Churn Prediction Project Overview

## Feature Overview

### 1. ETL Process & Data Integration
- Developed a comprehensive ETL (Extract, Transform, Load) process to aggregate data from various sources into a unified dataset.
- Ensured data consistency and accuracy for reliable churn prediction analysis.

### 2. Machine Learning Model - Random Forest Implementation

#### Data Analysis
- Analyzed a robust dataset consisting of 7,043 rows and 35 features, including customer demographics, service usage, and account information.

#### Model Training
- Leveraged the Random Forest Algorithm for its robustness in handling large datasets with high dimensionality and reducing overfitting.
- Key features: `Customer_Status`, `Monthly_Charge`, `Tenure_in_Months`, `Internet_Service`, `Contract`.

#### Performance Metrics
- Achieved an accuracy of 84% in predicting customer churn.
- Performance evaluation metrics:
  - **Precision**: 0.86 for non-churners, 0.78 for churners.
  - **Recall**: 0.92 for non-churners, 0.65 for churners.
  - **F1-Score**: Balanced precision and recall.

  ![Confusion Matrix](path/to/confusion_matrix_image) <!-- Insert your confusion matrix image here -->
  
  ![Classification Report](path/to/classification_report_image) <!-- Insert your classification report image here -->

### 3. Data Exploration & Cleaning

#### Distinct Value Analysis
```sql
-- Gender Distribution
SELECT Gender, Count(Gender) as TotalCount, 
Count(Gender) * 1.0 / (Select Count(*) from stg_Churn) as Percentage 
FROM stg_Churn 
GROUP BY Gender;

-- Contract Distribution
SELECT Contract, Count(Contract) as TotalCount, 
Count(Contract) * 1.0 / (Select Count(*) from stg_Churn) as Percentage 
FROM stg_Churn 
GROUP BY Contract;

-- Customer Status and Revenue Distribution
SELECT Customer_Status, Count(Customer_Status) as TotalCount, Sum(Total_Revenue) as TotalRev, 
Sum(Total_Revenue) / (Select sum(Total_Revenue) from stg_Churn) * 100 as RevPercentage 
FROM stg_Churn 
GROUP BY Customer_Status;

-- State Distribution
SELECT State, Count(State) as TotalCount, 
Count(State) * 1.0 / (Select Count(*) from stg_Churn) as Percentage 
FROM stg_Churn 
GROUP BY State 
ORDER BY Percentage desc;

-- Null Value Analysis
SELECT 
    SUM(CASE WHEN Customer_ID IS NULL THEN 1 ELSE 0 END) AS Customer_ID_Null_Count,
    SUM(CASE WHEN Gender IS NULL THEN 1 ELSE 0 END) AS Gender_Null_Count,
    SUM(CASE WHEN Age IS NULL THEN 1 ELSE 0 END) AS Age_Null_Count,
    SUM(CASE WHEN Married IS NULL THEN 1 ELSE 0 END) AS Married_Null_Count,
    SUM(CASE WHEN State IS NULL THEN 1 ELSE 0 END) AS State_Null_Count,
    SUM(CASE WHEN Number_of_Referrals IS NULL THEN 1 ELSE 0 END) AS Number_of_Referrals_Null_Count,
    SUM(CASE WHEN Tenure_in_Months IS NULL THEN 1 ELSE 0 END) AS Tenure_in_Months_Null_Count,
    SUM(CASE WHEN Value_Deal IS NULL THEN 1 ELSE 0 END) AS Value_Deal_Null_Count,
    SUM(CASE WHEN Phone_Service IS NULL THEN 1 ELSE 0 END) AS Phone_Service_Null_Count,
    SUM(CASE WHEN Multiple_Lines IS NULL THEN 1 ELSE 0 END) AS Multiple_Lines_Null_Count,
    SUM(CASE WHEN Internet_Service IS NULL THEN 1 ELSE 0 END) AS Internet_Service_Null_Count,
    SUM(CASE WHEN Internet_Type IS NULL THEN 1 ELSE 0 END) AS Internet_Type_Null_Count,
    SUM(CASE WHEN Online_Security IS NULL THEN 1 ELSE 0 END) AS Online_Security_Null_Count,
    SUM(CASE WHEN Online_Backup IS NULL THEN 1 ELSE 0 END) AS Online_Backup_Null_Count,
    SUM(CASE WHEN Device_Protection_Plan IS NULL THEN 1 ELSE 0 END) AS Device_Protection_Plan_Null_Count,
    SUM(CASE WHEN Premium_Support IS NULL THEN 1 ELSE 0 END) AS Premium_Support_Null_Count,
    SUM(CASE WHEN Streaming_TV IS NULL THEN 1 ELSE 0 END) AS Streaming_TV_Null_Count,
    SUM(CASE WHEN Streaming_Movies IS NULL THEN 1 ELSE 0 END) AS Streaming_Movies_Null_Count,
    SUM(CASE WHEN Streaming_Music IS NULL THEN 1 ELSE 0 END) AS Streaming_Music_Null_Count,
    SUM(CASE WHEN Unlimited_Data IS NULL THEN 1 ELSE 0 END) AS Unlimited_Data_Null_Count,
    SUM(CASE WHEN Contract IS NULL THEN 1 ELSE 0 END) AS Contract_Null_Count,
    SUM(CASE WHEN Paperless_Billing IS NULL THEN 1 ELSE 0 END) AS Paperless_Billing_Null_Count,
    SUM(CASE WHEN Payment_Method IS NULL THEN 1 ELSE 0 END) AS Payment_Method_Null_Count,
    SUM(CASE WHEN Monthly_Charge IS NULL THEN 1 ELSE 0 END) AS Monthly_Charge_Null_Count,
    SUM(CASE WHEN Total_Charges IS NULL THEN 1 ELSE 0 END) AS Total_Charges_Null_Count,
    SUM(CASE WHEN Total_Refunds IS NULL THEN 1 ELSE 0 END) AS Total_Refunds_Null_Count,
    SUM(CASE WHEN Total_Extra_Data_Charges IS NULL THEN 1 ELSE 0 END) AS Total_Extra_Data_Charges_Null_Count,
    SUM(CASE WHEN Total_Long_Distance_Charges IS NULL THEN 1 ELSE 0 END) AS Total_Long_Distance_Charges_Null_Count,
    SUM(CASE WHEN Total_Revenue IS NULL THEN 1 ELSE 0 END) AS Total_Revenue_Null_Count,
    SUM(CASE WHEN Customer_Status IS NULL THEN 1 ELSE 0 END) AS Customer_Status_Null_Count,
    SUM(CASE WHEN Churn_Category IS NULL THEN 1 ELSE 0 END) AS Churn_Category_Null_Count,
    SUM(CASE WHEN Churn_Reason IS NULL THEN 1 ELSE 0 END) AS Churn_Reason_Null_Count
FROM stg_Churn;

-- Data Cleansing and Loading
INSERT INTO [db_Churn].[dbo].[prod_Churn]
SELECT 
    Customer_ID,
    Gender,
    Age,
    Married,
    State,
    Number_of_Referrals,
    Tenure_in_Months,
    ISNULL(Value_Deal, 'None') AS Value_Deal,
    Phone_Service,
    ISNULL(Multiple_Lines, 'No') AS Multiple_Lines,
    Internet_Service,
    ISNULL(Internet_Type, 'None') AS Internet_Type,
    ISNULL(Online_Security, 'No') AS Online_Security,
    ISNULL(Online_Backup, 'No') AS Online_Backup,
    ISNULL(Device_Protection_Plan, 'No') AS Device_Protection_Plan,
    ISNULL(Premium_Support, 'No') AS Premium_Support,
    ISNULL(Streaming_TV, 'No') AS Streaming_TV,
    ISNULL(Streaming_Movies, 'No') AS Streaming_Movies,
    ISNULL(Streaming_Music, 'No') AS Streaming_Music,
    ISNULL(Unlimited_Data, 'No') AS Unlimited_Data,
    Contract,
    Paperless_Billing,
    Payment_Method,
    Monthly_Charge,
    Total_Charges,
    Total_Refunds,
    Total_Extra_Data_Charges,
    Total_Long_Distance_Charges,
    Total_Revenue,
    Customer_Status,
    ISNULL(Churn_Category, 'Others') AS Churn_Category,
    ISNULL(Churn_Reason, 'Others') AS Churn_Reason
FROM [db_Churn].[dbo].[stg_Churn];
```
```sql
-- Create Views for Power BI
CREATE VIEW vw_ChurnData AS
SELECT * FROM prod_Churn WHERE Customer_Status IN ('Churned', 'Stayed');

CREATE VIEW vw_JoinData AS
SELECT * FROM prod_Churn WHERE Customer_Status = 'Joined';

-- Add new columns in prod_Churn
ALTER TABLE prod_Churn ADD
Churn_Status AS (CASE WHEN Customer_Status = 'Churned' THEN 1 ELSE 0 END),
Monthly_Charge_Range AS (CASE WHEN Monthly_Charge < 20 THEN '< 20'
                              WHEN Monthly_Charge < 50 THEN '20-50'
                              WHEN Monthly_Charge < 100 THEN '50-100'
                              ELSE '> 100' END);
```
**Power BI Visualizations**:

-   Created a detailed dashboard in Power BI featuring 14 visuals to present churn analysis and prediction insights.
-   Visuals include:
    -   **Summary Metrics**: Total Customers, New Joiners, Total Churn, Churn Rate.
    -   **Demographic Analysis**: Churn by Gender, Age Group, Payment Method, and Contract Type.
    -   **Geographic Analysis**: Churn Rate by State.
    -   **Service Analysis**: Churn Rate by Internet Type and Services Used.
    -   **Churn Prediction**: Predicted Churner Profile by Age, Gender, State, Marital Status, Tenure, Payment Method, and Contract.
    -   **Customer Details**: Table listing customer ID, monthly charge, total revenue, total refunds, and referrals for predicted churners.
