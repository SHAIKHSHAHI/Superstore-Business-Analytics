# Superstore-Business-Analytics
## Project Overview:
This Python-based deep dive project analyzes an eCommerce superstore dataset to uncover valuable business insights. It includes advanced analyses such as customer segmentation, product basket analysis, and customer lifetime value (CLV). The project also features key performance indicators (KPIs) visualization to track sales, profit, and customer metrics effectively. This comprehensive approach helps in making data-driven decisions to boost business performance.
The first four cities — New York City, Los Angeles, Seattle, and San Francisco — appear in both the Top Sales and Top Profit lists. This shows a strong alignment between revenue generation and profitability, meaning these cities are not only generating high sales but are also doing so efficiently.
```python
Top10RevenueCities=Data.groupby('City')[['Sales','Profit']].sum().sort_values(by='Sales',ascending=False).head(10)

Top10ProfitCities=Data.groupby('City')[['Sales','Profit']].sum().sort_values(by='Profit',ascending=False).head(10)
```

## Interpretation:
Cities like Philadelphia, Chicago, Houston, and Jacksonville have high sales but show negative profits, possibly due to:

Higher operational costs

Increased discounts or returns

Other expenses affecting profitability


These areas require further investigation to understand why profits are negative despite good sales.

Conversely, cities such as Detroit, Lafayette, Jackson, Atlanta, and Minneapolis have moderate or low sales but are among the top profit earners.

This indicates efficient operations or higher profit margins in these locations, contributing positively to overall profitability.
