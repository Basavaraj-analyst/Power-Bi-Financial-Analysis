# Power-Bi-Financial-Analysis
Project Overview

The Finance Analytics Dashboard is an interactive Power BI business intelligence solution designed to help a financial organization monitor and analyze financial transactions, customer behavior, fees, taxes, transaction performance, customer segments, and regional performance.

The dashboard provides management with a centralized analytical view for monitoring financial KPIs, identifying high-performing customer segments and states, analyzing transaction patterns, tracking fees and taxes, and supporting data-driven financial decision-making.

The project is developed using Power BI, DAX, Power Query, and SQL with a focus on realistic financial analytics and interactive business reporting.

Business Objective

The primary objective is to build a centralized Finance Analytics solution that enables stakeholders to:

Monitor overall transaction growth and financial performance
Analyze monthly transaction amount trends
Compare successful, failed, and pending transactions
Analyze customer segment contribution
Evaluate state-wise financial performance
Analyze transaction type performance and profitability
Understand gender-based customer participation
Track Year-over-Year (YoY) performance
Monitor operational fees and taxes
Identify important financial and customer trends
Support business and financial decision-making


Customers Dataset

File: customers.csv

Records: 5,000 customers

Customer Fields
Column	Description
customer_id	Unique customer identifier
fisrt_name	Customer first name
second_name	Customer surname
gender	Customer gender
date_of_birth	Customer date of birth
city	Customer city
state	Customer state
occupation	Customer occupation
customer_segment	Customer business segment
annual_income	Customer annual income
join_date	Customer joining date
Customer Segments
Retail
Premium
SME
Corporate
Wealth
Occupations
The customer dataset contains occupations such as:

Salaried
Business Owner
Self Employed
Student
Professional
Retired
Homemaker


Finance Transactions Dataset

File: finance_transactions.csv

Records: 50,069 transaction rows

Transaction Fields
Column	Description
transaction_id	Unique transaction identifier
transaction_date	Date of transaction
account_id	Account identifier
customer_id	Related customer
transaction_type	Type of financial transaction
channel	Transaction channel
merchant_category	Merchant/business category
amount	Transaction amount
fee_amount	Transaction fee
tax_amount	Tax generated
currency	Transaction currency
transaction_status	Success / Failed / Pending
is_fraud	Fraud indicator
risk_score	Transaction risk score
reference_no	Transaction reference number


Data Preparation

The transaction dataset contains data-quality issues that provide realistic Power Query transformation opportunities.

Examples include inconsistent formatting in:

Transaction channels
Currency values
Text spacing
Date formats
Categorical values

Merging of First and last Name
Removing Duplicates
Trimming text
Calculated Absolute Value
Replaced Value
Uppercase Text




Currency values also contain variations such as:

INR
inr
inR

These should be standardized to:

INR


Power Query Transformation Steps
Change appropriate data types
Trim text columns
Clean text values
Standardize transaction channels
Standardize currency values
Validate transaction statuses
Validate customer segments
Handle missing values
Check duplicate transaction IDs
Validate financial amounts
Create a proper Date table
Merge customer attributes where required
Create relationships between fact and dimension tables


Dashboard Structure

The Power BI report is designed around two major analytical pages.

Dashboard 1 — Finance Executive Overview

The executive dashboard provides a high-level view of financial performance.

KPI Cards
Total Amount
Total Transactions
Average Transaction Value
Total Fees
Total Tax

        Total Amount = SUM(finance_transactions[amount])
        PY Transaction = CALCULATE([Total Transaction],SAMEPERIODLASTYEAR('Calender Table'[Date]))
        YOY = [Total Amount]-[PY Year]
        YOY % Amount = DIVIDE([YOY],[PY Year],0)

        Total Transaction = DISTINCTCOUNT(finance_transactions[transaction_id])
        PY Transaction = CALCULATE([Total Transaction],SAMEPERIODLASTYEAR('Calender Table'[Date]))
        YOY Transaction = [Total Transaction]-[PY Transaction]
        YOY % Transaction = DIVIDE([YOY Transaction],[PY Transaction],0)

        Avg Transaction Value = AVERAGE(finance_transactions[amount])
        PY Avg Transaction = CALCULATE([Avg Transaction Value],SAMEPERIODLASTYEAR('Calender Table'[Date]))
        YOY Avg Transaction = [Avg Transaction Value]-[PY Avg Transaction]
        YOY % Avg Transaction = DIVIDE([YOY Avg Transaction],[PY Avg Transaction],0)

        Total Fee = SUM(finance_transactions[fee_amount])
        PY Fee = CALCULATE([Total Fee],SAMEPERIODLASTYEAR('Calender Table'[Date]))
        YOY fee = [Total Fee]-[PY Fee]
        YOY % Fee = DIVIDE([YOY fee],[PY Fee],0)

        Total Tax = SUM(finance_transactions[tax_amount])
        PY Tax = CALCULATE([Total Tax],SAMEPERIODLASTYEAR('Calender Table'[Date]))
        YOY Tax = [Total Tax]-[PY Tax]
        YOY % Tax = DIVIDE([YOY Tax],[PY Tax],0)

The business requirements specifically define these five KPIs.


Dynamic Measure Selection

A disconnected measure table is used to allow users to dynamically switch the metric displayed in selected visuals.

Measures
Total Amount
Total Transactions
Average Transaction Value
Total Fees
Total Tax

    Dynamic Title = SWITCH('Dynamic Metrics'[Dynamic Metrics Order],
                                        0, "Total Amount",
                                        1, "Total Transaction",
                                        2, "Total Fee",
                                        3, "Total Tax", "Others")

    Select Dynamic Measure = SELECTEDVALUE('Dynamic Metrics'[Dynamic Title])
    Select Dynamic Year = SELECTEDVALUE('Calender Table'[Year])
    Dynamic Metrics = {
    ("Total Amount", NAMEOF('Calender Table'[Total Amount]), 0),
    ("Total Transaction", NAMEOF('finance_transactions'[Total Transaction]), 1),
    ("Total Fee", NAMEOF('finance_transactions'[Total Fee]), 2),
    ("Total Tax", NAMEOF('finance_transactions'[Total Tax]), 3)
Time Intelligence
Created Calender Table

      Calender Table = CALENDAR(MIN(finance_transactions[transaction_date]),MAX(finance_transactions[transaction_date]))
      Year = YEAR('Calender Table'[Date])
      Month = FORMAT('Calender Table'[Date],"MMM")
      Month Num = MONTH('Calender Table'[Date])
Dashboard Structure

Dashboard 1 --Over View Analysis


1. Total Amount by Month
Chart Type:
Line Chart / Area Chart
Objective:
Analyze monthly transaction amount trends throughout the year and identify seasonal spikes or drops.

            X-axis:

       DimDate[Month Year]

        Y-axis:

        [Total Amount]


3. Total Amount by Transaction Status
Chart Type:
Donut Chart
Objective:
Compare transaction amounts based on transaction status:
•	Success 
•	Failed 
•	Pending 
Helps measure operational efficiency and transaction success rate.

        Legend

        Transaction_Status

         Values

        [Total Amount]

        Categories:

        Success
        Failed
        Pending
4. Total Amount by Customer Segment
Chart Type:
Horizontal Bar Chart
Objective:
Analyze contribution of different customer segments:
•	Retail 
•	Premium 
•	SME 
•	Corporate 
•	Wealth 
Helps identify the most valuable customer groups.


        Horizontal Bar Chart

        Axis:

          Customer_Segment

        Values:

            [Total Amount]

        Segments:

        Retail
        Premium
        SME
        Corporate
        Wealth


5. Total Amount by State
Chart Type:
Horizontal Bar Chart
Objective:
Compare state-wise transaction amounts to identify top-performing regions.

            Horizontal Bar Chart

        Axis:

        State

        Values:

        [Total Amount]

        Sort:

       Descending

        Add:

        Total Transactions
        Fees
        Tax
        Average Transaction Value


6. Transaction Type Analysis
Chart Type:
Matrix / Heatmap Table
Metrics Included:
•	Amount 
•	Fees 
•	Tax 
•	Transactions Count 
Transaction Types:
•	Bill Payment 
•	Card Payment 
•	Deposit 
•	Fee Charge 
•	Interest Credit 
•	Investment 
•	Loan EMI 
•	Refund 
•	Transfer 
•	Withdrawal 
Objective:
Understand performance and profitability by transaction category.

        Matrix

        Rows:

        Transaction_Type

        Columns:

        Metrics

        Values:

        Amount
        Fees
        Tax
        Transactions

7. Total Amount by Gender
Chart Type:
Donut Chart
Objective:
Analyze transaction amount contribution by:
•	Male 
•	Female 
Helps understand customer demographic participation


        Donut Chart

        Legend:

        Gender

        Values:

        [Total Amount]

        Categories:

        Male
        Female

Dashboard 2 — Transactions
Create a detailed grid view and drill down for underlying records as well.

Interactive Filters

Users can dynamically filter the report by:

Year
Dynamic Measure
Occupation
Category


  Portfolio Project Highlights...
50K+ Financial Transactions
5K Customers
Multiple Customer Segments
10 Transaction Types
Monthly Financial Analysis
State-wise Performance
Gender Analysis
Transaction Status Analysis
Fees & Tax Analysis
Dynamic Measures
YoY Analysis
Drill-down
Drill-through
Transaction-level Analysis
Fraud & Risk Data Available
Power Query Transformation
DAX Analytics
Star Schema Data Model 

Star Schema reference is attached in screenshot<img width="2297" height="1095" alt="Screenshot 2026-08-11 223331" src="https://github.com/user-attachments/assets/6d43305a-94b0-4de1-9a11-3d8158061374" />



[Finacial Analysis.pdf](https://github.com/user-attachments/files/30944428/Finacial.Analysis.pdf)
[finance_transactions.csv](https://github.com/user-attachments/files/30944418/finance_transactions.csv)
[customers.csv](https://github.com/user-attachments/files/30944416/customers.csv)


Project Architecture


                 FINANCE ANALYTICS
                        │
                        ▼
                Power BI Report
                        │
          ┌─────────────┴─────────────┐
          │                           │
          ▼                           ▼
     Dashboard 1                  Dashboard 2
    Executive Overview             Detail Analysis
          │                           │
          │                           ▼
          │                    Transaction Grid
          │                           │
          │                           ▼
          │                    Drill Through
          │                           │
          └──────────────┬────────────┘
                         ▼
                   Star Schema
                         │
          ┌──────────────┼──────────────────┐
          ▼              ▼         	     ▼
    FactTransactions   DimDate     	 Dimensions
                    
                           		           ▼
                       			          Customer
                                        
