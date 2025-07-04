# Analyze-and-predict-customer-churn-rate-with-Power-BI-and-Random-Forest
<div align="center">
  <img src="https://github.com/user-attachments/assets/e87a9a1f-fb25-45ac-abb0-c3f6bf22e674" alt="mô tả" width="600">
</div>


# I. Goal
Objective:
- Maximize Revenue
- Cost of Acquiring New Customers
# II. Company overview
Telecommunications companies provide services such as telephone and internet.
<div align="center">
  <img src="https://github.com/user-attachments/assets/6ff14952-c2f2-4b66-a8d8-bd3fa49adc80" alt="mô tả" width="600">
</div>

# III. Data overoverview
Telecom customer churn data overview
<div align="center">
  <img src="https://github.com/user-attachments/assets/f404651c-e800-4a23-9711-faa43fcd1ee1" alt="mô tả" width="800">
</div>

# IV. Process
## STEP 1 – ETL Process in SQL Server
![image](https://github.com/user-attachments/assets/6c8476a6-9737-4738-8d2b-f3fff2f91ead)

**Remove null and insert the new data into Prod table**
```
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
    ISNULL(Multiple_Lines, 'No') As Multiple_Lines,
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
    ISNULL(Churn_Reason , 'Others') AS Churn_Reason
 
INTO [db_Churn].[dbo].[prod_Churn]
FROM [db_Churn].[dbo].[stg_Churn];
```

**Create View for Power BI for prediction**
```
Create View vw_ChurnData as
    select * from prod_Churn where Customer_Status In ('Churned', 'Stayed')
 ```
``` 
Create View vw_JoinData as
    select * from prod_Churn where Customer_Status = 'Joined'
```
## STEP 2 & 3 – Power BI Transform

**Transform** 

- Churn status: convert from text to number (Stayed-0, churned-1)
- Create value range for monthly charge: <20, 20-50, 50-100, >100

**Measure**
  
- Total customer
- New joiners
- Total Churn
- Churn rate

**Create reference table for Age**
  
- Age group: <20, 20-35, 36-50, >50

**Create reference table for Tenure**
  
- Tenure group: <6 months, 6- 12 months, 12-18 months, 18-24 months, >=24 months

**Create reference table for Service**

- Unpivot columns related to service
- Set status

## STEP 4 – Power BI Visualization
<div align="center">
  <img src="https://github.com/user-attachments/assets/7f3aebb0-3adc-47e8-b65b-c70142a979e4" alt="mô tả" width="800">
</div>
<div align="center">
  <img src="https://github.com/user-attachments/assets/3111a86d-b5b1-455d-b3d1-4b49df2407b7" alt="mô tả" width="800">
</div>


## STEP 5 – Predict Customer Churn
A random forest is a machine learning algorithm that consists of multiple decision trees. Each decision tree is trained on a random subset of the data and features. The final prediction is made by averaging the predictions (in regression tasks) or taking the majority vote (in classification tasks) from all the trees in the forest. This ensemble approach improves the accuracy and robustness of the model by reducing the risk of overfitting compared to using a single decision tree.

 - Import both vw_ChurnData & vw_JoinData from database
 - With vw_ChurnData for training model and forecast values of vw_JoinData

   [Source code](https://github.com/cuongdaoo/Analyze-and-predict-customer-churn-rate-with-Power-BI-and-Random-Forest/blob/main/Churn_Analyst.ipynb)

Visual prediction data into Power BI
<div align="center">
  <img src="https://github.com/user-attachments/assets/ca6cd96a-f78f-4ed1-9c2a-e40ccded972a" alt="mô tả" width="800">
</div>

* 🔗 **Live Power BI Dashboard**: [View Dashboard](https://app.powerbi.com/view?r=eyJrIjoiYjM1MTk0NzUtNzdlNS00MDIwLTlkNTAtNWE1NjY4MDcxYmU5IiwidCI6IjA0NjRjNWRlLTQzNmItNDJjMi05NzQ4LTc0NTZmNWQzYTU2NCIsImMiOjEwfQ%3D%3D)
# V. Project Resources

* 📘 **Jupyter Notebook**:
  [View full notebook here](https://github.com/cuongdaoo/Analyze-and-predict-customer-churn-rate-with-Power-BI-and-Random-Forest/blob/main/Churn_Analyst.ipynb)

* 📑 **Detailed Analysis Report**:
  [Access the full report here](https://github.com/cuongdaoo/Analyze-and-predict-customer-churn-rate-with-Power-BI-and-Random-Forest/blob/main/Analysis.md)
