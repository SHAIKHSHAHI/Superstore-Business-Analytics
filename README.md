# Superstore-Business-Analytics
## Project Overview:
- This Python-based deep dive project analyzes an eCommerce superstore dataset to uncover valuable business insights. --- It includes advanced analyses such as customer segmentation, product basket analysis, and customer lifetime value (CLV). 
- The project also features key performance indicators (KPIs) visualization to track sales, profit, and customer metrics effectively.
- This comprehensive approach helps in making data-driven decisions to boost business performance.
### Importing Of All Required Liabraries
```python
import pandas as pd
import datetime as dt
import matplotlib.pyplot as plt
import seaborn as sns
import matplotlib.ticker as ticker
import numpy as np
pd.set_option('display.max_columns', None)
```
## Importing File
```python
Data=pd.read_csv('/storage/emulated/0/Download/archive(2) (4)/Sample - Superstore.csv',
encoding='latin1')
```
## changing Datatypes
```python
Data['Order Date']=pd.to_datetime(Data['Order Date'])
Data['Ship Date']=pd.to_datetime(Data['Ship Date'])
#Renaming Column
Data.rename(columns={'Segment':'Customer Segment','Category':'Product Category'},inplace=True)--
```
## Adding Columns
```python
Data['Year']=Data['Order Date'].dt.year
Data['Day']=Data['Order Date'].dt.day_name()
Data['Delay Days']=(Data['Ship Date']-Data['Order Date']).dt.days
```
```python
Top10RevenueCities=Data.groupby('City')
[['Sales','Profit']].sum().sort_values(by='Sales',ascending=False)
.head(10)

Top10ProfitCities=Data.groupby('City')
[['Sales','Profit']].sum().sort_values(by='Profit',ascending=False)
.head(10)
```
```plaintext
Sales      Profit
City
New York City  256368.1610  62036.9837
Los Angeles    175851.3410  30440.7579
Seattle        119540.7420  29156.0967
San Francisco  112669.0920  17507.3854
Philadelphia   109077.0130 -13837.7674
Houston         64504.7604 -10153.5485
Chicago         48539.5410  -6654.5688
San Diego       47521.0290   6377.1960
Jacksonville    44713.1830  -2323.8350
Springfield     43054.3420   6200.6974
                    Sales      Profit
City
New York City  256368.161  62036.9837
Los Angeles    175851.341  30440.7579
Seattle        119540.742  29156.0967
San Francisco  112669.092  17507.3854
Detroit         42446.944  13181.7908
Lafayette       25036.200  10018.3876
Jackson         24963.858   7581.6828
Atlanta         17197.840   6993.6629
Minneapolis     16870.540   6824.5846
San Diego       47521.029   6377.1960
```
## Interpretation:
- 1.The first four cities — New York City, Los Angeles, Seattle, and San Francisco — appear in both Top Sales and Top Profit lists, indicating strong alignment between sales revenue and profitability.

- 2.This suggests these cities generate high sales efficiently, converting revenue into profit effectively.
- 3.Cities like Philadelphia, Chicago, Houston, and Jacksonville have high sales but show negative profits, possibly due to:

- i.Higher operational costs

- ii.Increased discounts or returns

- iii.Other expenses affecting profitability

- These areas require further investigation to understand why profits are negative despite good sales.

- 4.Conversely, cities such as Detroit, Lafayette, Jackson, Atlanta, and Minneapolis have moderate or low sales but are among the top profit earners.

- 5.This indicates efficient operations or higher profit margins in these locations, contributing positively to overall profitability.
- 
#
```python
Count the number of customers in each CLV segment
segment_counts = CLV_Final_Data['CLV_Segment'].value_counts()
````
## 4.💰 Customer Lifetime Value (CLV) Analysis

In this step, we calculate the CLV for each customer by combining their average purchase value, purchase frequency, and lifespan. Based on the average CLV, customers are segmented into High Value and Low Value categories.

✔️ Key Metrics:

- 1.Average Purchase Value = Total Sales / Number of Unique Orders

- 2.Purchase Frequency = Number of Unique Orders per Customer

- 3.Customer Lifespan = Duration between First and Last Purchase (in years)

- 4.CLV = Average Purchase Value × Purchase Frequency × Lifespan

```python
# Calculate Average Purchase Value
1.Avg_Purchase_Value = Data.groupby('Customer ID')['Sales'].sum() / Data.groupby('Customer ID')['Order ID'].nunique()
Avg_Purchase_Value = Avg_Purchase_Value.reset_index(name='Avg_Purchase_Value')

# Calculate Customer Purchase Frequency
2.Customer_Purchase_Frequency = Data.groupby('Customer ID')['Order ID'].nunique().reset_index(name='Purchase Frequency')

# Calculate Customer Lifespan in Years
3.lifespan = Data.groupby('Customer ID').agg(
    First_Purchase=('Order Date', 'min'),
    Last_Purchase=('Order Date', 'max')
).reset_index()
4.lifespan['Lifespan_Years'] = np.floor((lifespan['Last_Purchase'] - lifespan['First_Purchase']).dt.days / 365)
5.lifespan['Lifespan_Years'] = lifespan['Lifespan_Years'].astype(int)

# Merge All DataFrames
5.CLV_Data = pd.merge(Avg_Purchase_Value, Customer_Purchase_Frequency, on='Customer ID')
6.CLV_Final_Data = pd.merge(CLV_Data, lifespan, on='Customer ID')

# Calculate CLV
7.CLV_Final_Data['Customers_Lifetime_Value'] = (
    CLV_Final_Data['Avg_Purchase_Value'] *
    CLV_Final_Data['Purchase Frequency'] *
    CLV_Final_Data['Lifespan_Years']
)

# Segment Customers Based on Average CLV
8.Avg_Clv = CLV_Final_Data['Customers_Lifetime_Value'].mean()
9.CLV_Final_Data['CLV_Segment'] = np.where(
    CLV_Final_Data['Customers_Lifetime_Value'] > Avg_Clv,
    'High Value',
    'Low Value'
)
```

![CLV Distribution](images/%28CLV%291.png)
## 5. 📊 CLV Segment Distribution Analysis

This visualization helps to understand how the CLV values are distributed between High Value and Low Value customer segments.
```python
Visual Code

plt.figure(figsize=(6,5),dpi=300)

sns.boxplot(data=CLV_Final_Data,  x='Customers_Lifetime_Value',y='CLV_Segment', palette='flare')
plt.xlabel('Customers Value',fontsize=12, fontweight='heavy')
plt.ylabel('CLV Segment',fontsize=12, fontweight='heavy')
plt.yticks(rotation=90)


plt.suptitle('(CLV) Distribution Analysis',fontsize=16, fontweight='heavy')

plt.savefig('(CLV)1.png')
plt.tight_layout(rect=[0,0,0,1])
plt.subplots_adjust(hspace=0.5,wspace=0.5)
plt.show()
```
## 📈 Year-wise Customer & Order Analysis
```python
CustomersPerYear=Data.groupby('Year')['Customer ID'].nunique().reset_index()
CustomersPerYear.columns=['Year','Customer Count']

OrdersPerYear=Data.groupby('Year')['Order ID'].nunique().sort_values(ascending=False)
PerYearOrders=OrdersPerYear.reset_index()
PerYearOrders.columns=['Year','Total Orders']
```
```python
visual code
plt.figure(figsize=(8,6),dpi=300)
plt.subplot(1,2,1)
sns.lineplot(data=CustomersPerYear,x='Year',y='Customer Count',
color='green',marker='o')
plt.title('Customer Trend By Year',fontsize=12,fontweight='bold',)
plt.xlabel('Year',fontsize=12,fontweight='bold')
plt.ylabel('Total Customers',fontsize=12,fontweight='bold')
plt.grid(axis='y',color='pink', alpha=0.7)
plt.gca().xaxis.set_major_locator(ticker.MaxNLocator(integer=True))

plt.subplot(1,2,2)

sns.lineplot(data=PerYearOrders, x='Year', y='Total Orders', marker='o', linewidth=2, palette='Oranges')

plt.title('Orders Per Year', fontsize=12, fontweight='heavy')
plt.xlabel('Years', fontsize=12,fontweight='heavy')
plt.ylabel('Number of Orders Per Year', fontsize=12,fontweight='heavy')
plt.grid(axis='y',color='pink', alpha=0.7)

plt.suptitle('Yearly Business Performance',
fontsize=14,fontweight='heavy')
plt.tight_layout(rect=[1,1,1,3])
plt.subplots_adjust(hspace=0.5, wspace=0.5)
plt.savefig('Yearly Trend.png')

plt.show()
```
## Repeated vs Non Repeated
- We first extracted all unique customers using .unique() to avoid counting duplicates. Then, we filtered the dataset to include only those unique customers for accurate analysis of their order behavior.
- This code identifies whether each customer is a repeated or non-repeated buyer based on their number of unique orders. If a customer has placed more than one order, they are labeled as Repeated, else Non-Repeated.
```python
UniqueCustomers=pd.DataFrame(Data['Customer ID'].unique(),columns=['Customer ID']) UniqueCustomersData=Data[Data['Customer ID'].isin(UniqueCustomers['Customer ID'])] CustomersWithOrderCounts=UniqueCustomersData.groupby('Customer ID')['Order ID'].nunique() CustomersWithOrderCounts = CustomersWithOrderCounts.reset_index() CustomersWithOrderCounts['Customer Type'] = np.where( CustomersWithOrderCounts['Order ID'] > 1, 'Repeated', 'Non-Repeated' ) Explain in 2 line
```



```python



