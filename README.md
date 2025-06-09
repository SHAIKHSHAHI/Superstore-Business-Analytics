# Superstore-Business-Analytics
## Project Overview:
- This Python-based deep dive project analyzes an eCommerce superstore dataset to uncover valuable business insights. --- It includes advanced analyses such as customer segmentation, product basket analysis, and customer lifetime value (CLV). 
- The project also features key performance indicators (KPIs) visualization to track sales, profit, and customer metrics effectively.
- This comprehensive approach helps in making data-driven decisions to boost business performance.
## Importing Of All Required Liabraries
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
--changing Datatypes
Data['Order Date']=pd.to_datetime(Data['Order Date'])
Data['Ship Date']=pd.to_datetime(Data['Ship Date'])
#Renaming Column
Data.rename(columns={'Segment':'Customer Segment','Category':'Product Category'},inplace=True)--

#Adding Columns
Data['Year']=Data['Order Date'].dt.year
Data['Day']=Data['Order Date'].dt.day_name()
Data['Delay Days']=(Data['Ship Date']-Data['Order Date']).dt.days

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
