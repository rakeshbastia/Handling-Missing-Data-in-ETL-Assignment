# Section A – Theoretical Questions
## Q1. What are the most common reasons for missing data in ETL pipelines?
### Answer:

Missing data can occur in an ETL pipeline for several reasons. Some common reasons are:
1. Data was not entered – A user may leave a field empty while entering information.
2. Data source problems – Sometimes the source system may not provide a particular
value.
3. Data transfer errors – Data may be lost while transferring it from one system to
another.
4. Different data formats – When combining data from different sources, some fields may
not match properly.
5. System or technical issues – Database errors, API problems, or software failures can
also result in missing values.
Therefore, it is important to understand why the data is missing before deciding how to handle it.

## Q2. Why is blindly deleting rows with missing values considered a bad practice in ETL?
### Answer:

Blindly deleting rows with missing values is not a good practice because we may lose useful
information along with the missing value.
For example, if a dataset contains 1,000 customer records and 100 records have one missing
value, deleting all 100 records means losing 10% of the customer information.
It can also create biased results if the missing values are related to a particular group of
customers. Therefore, we should first understand the reason and amount of missing data and then
choose a suitable method such as imputation, flagging, or deletion.

## Q3. Explain the difference between Listwise deletion and Column deletion. Also mention one scenario where each is appropriate.
### Answer:

Listwise Deletion:
In listwise deletion, the entire row is removed if it contains a missing value in the required
column or columns.
For example, if a customer has a missing Region value and Region is necessary for the analysis,
that complete customer record can be removed.
Appropriate scenario:
It can be used when only a small number of rows have missing values and removing them will
not significantly affect the analysis.
Column Deletion:
In column deletion, an entire column is removed when it contains too many missing values or is
not useful for the analysis.
Appropriate scenario:
It can be used when a particular column has a very large percentage of missing values and the
column is not important for the analysis.
In simple terms, listwise deletion removes rows, while column deletion removes an entire
column.

## Q4. Why is median imputation preferred over mean imputation for skewed data such as income?
## Answer:

Median imputation is preferred for skewed data such as income because income values can have
some extremely high values.
These high values can increase the mean and make it less representative of a normal customer.
The median is less affected by extremely high or low values. Therefore, replacing missing
income values with the median usually gives a more realistic value when the income data is
highly skewed.

## Q5. What is forward fill and in what type of dataset is it most useful?
### Answer:

Forward fill is a method of handling missing values where a missing value is replaced with the
previously available value.
For example:-
Before:
65000
NaN
58000
After forward fill:-
65000
65000
58000
Forward fill is most useful for time-series or sequential data, where the previous value is likely to
remain relevant for the next record.For example, it can be useful for sales, stock prices, sensor
readings, or other regularly recorded data.

## Q6. Why should flagging missing values be done before imputation in an ETL workflow?
### Answer:

Missing values should be flagged before imputation because once we replace the missing values,
it becomes difficult to identify which values were originally missing.
For example, if a missing Income value is replaced with ₹50,000, we can no longer tell whether ₹50,000
was the original value or an imputed value.
By creating a flag such as:
Income_Missing_Flag
0 = Value was present
1 = Value was missing
we can keep track of the original missing information even after filling the value.
This can also be useful for future analysis and business decisions.

## Q7. Consider a scenario where income is missing for many customers. How can this missingness itself provide business insights?
### Answer:

A large number of missing income values can itself provide useful business information.
For example, customers may be unwilling to provide their income information because they
consider it private or sensitive. It could also mean that a particular customer group does not
provide complete information.
If income is missing more often for a particular group, location, or customer type, the company
can investigate the reason.
Therefore, missing data is not always just a technical problem. The pattern of missingness can also
provide useful business insights.

# Section B – Practical Questions
## Q8. Listwise Deletion
## Task 1: Identify affected rows
### Answer:

There is only one row where the Region value is missing.
The affected record is:
Customer_ID Name City Monthly_Sales Income Region
105 Amit Verma Pune 18000 58000 NaN
Therefore, Customer_ID 105 (Amit Verma) will be removed.

## Task 2: Dataset after deletion
### Answer:

After removing the row where Region is missing, the dataset becomes:
Customer_ID Name City Monthly_Sales Income Region
101 Rahul Mehta Mumbai 12000 65000 West
104 Neha Singh Delhi NaN NaN North
102 Anjali Rao Bengaluru NaN NaN South
107 Pooja Das Kolkata 14000 NaN East
103 Suresh Iyer Chennai 15000 72000 South
106 Karan Shah Ahmedabad NaN 61000 West
108 Riya Kapoor Jaipur 16000 69000 North

## Task 3: Mention how many records were lost
### Answer:

Only 1 record was lost during listwise deletion.
The deleted record was:
Customer_ID: 105 – Amit Verma

## Q9. Imputation Using Forward Fill
## The assignment asks us to handle missing values in Monthly_Sales using Forward Fill.
## Task 1: Apply forward fill
### Answer:

The original Monthly_Sales values are:
12000
NaN
NaN
18000
14000
15000
NaN
16000
Using forward fill, each missing value is replaced with the previous available Monthly_Sales
value.
After forward fill:
12000
12000
12000
18000
14000
15000
15000
16000

## Task 2: Before vs After

Customer_ID Name Before After Forward Fill
101 Rahul Mehta 12000 12000
104 Neha Singh NaN 12000
102 Anjali Rao NaN 12000
105 Amit Verma 18000 18000
107 Pooja Das 14000 14000
103 Suresh Iyer 15000 15000
106 Karan Shah NaN 15000
108 Riya Kapoor 16000 16000

## Task 3: Explain why forward fill is suitable here.
### Answer:

Forward fill is suitable here because the missing Monthly_Sales values can be filled using the
most recent available sales value before them.
For example, Neha Singh and Anjali Rao have missing Monthly_Sales values after Rahul
Mehta's value of 12000, so forward fill uses 12000 for both missing values.
Similarly, Karan Shah's missing value comes after Suresh Iyer's 15000, so it is filled with 15000.
The assignment specifically asks to use Forward Fill for Monthly_Sales.

## Q10. Flagging Missing Data
  The assignment asks us to create an Income_Missing_Flag, where:
  • 0 = Income is present
  • 1 = Income is missing
## Task 1: Create Income_Missing_Flag
### Answer:

The updated dataset will be:
Customer_ID Name Income Income_Missing_Flag
101 Rahul Mehta 65000 0
104 Neha Singh NaN 1
102 Anjali Rao NaN 1
105 Amit Verma 58000 0
107 Pooja Das NaN 1
103 Suresh Iyer 72000 0
106 Karan Shah 61000 0
108 Riya Kapoor 69000 0

## Task 2: Count customers with missing income
### Answer:

There are 3 customers with missing Income:
1. Neha Singh – Customer_ID 104
2. Anjali Rao – Customer_ID 102
3. Pooja Das – Customer_ID 107
Therefore:
Total customers with missing income = 3
Final Summary
This assignment helped me understand that missing data should not always be deleted
immediately. Different situations require different methods. Listwise deletion can be useful
when only a few records are affected, while imputation can help preserve data. Forward fill is
useful for sequential data, and missing-value flags help us remember which values were
originally missing.
The practical part also shows how these techniques can be applied to a real customer dataset.
