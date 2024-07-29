# Sales Data Analysis Project

## Objective
The objective of this project is to analyze sales data using Power BI. The analysis provides insights into sales performance, customer demographics, product categories, and returns, enabling better business decisions.

## Datasets
The data for this project was sourced from various flat files, including:
- **Sales Data:** Contains details of sales transactions.
- **Customer Lookup:** Contains customer demographic information.
- **Product Categories Lookup:** Provides product category information.
- **Product Subcategories Lookup:** Provides product subcategory information.
- **Territory Lookup:** Contains details about sales territories.
- **Calendar Lookup:** Provides date-related information for time-based analysis.
- **Returns Data:** Contains details of product returns.
- **Rolling Calendar:** Helps in rolling period calculations.

## ETL Process
The ETL (Extract, Transform, Load) process involved the following steps:
1. **Extract:** Data was extracted from multiple flat files.
2. **Transform:** Data transformation included:
   - Cleaning and formatting data.
   - Creating calculated columns and measures using DAX.
   - Merging datasets based on common keys.
   - Creating hierarchies for time-based analysis.
3. **Load:** Transformed data was loaded into Power BI for visualization.

## Data Model
The data model is designed to connect various datasets through common keys, allowing comprehensive analysis. Below is an overview of the data model:

![Data Model](./path_to_data_model_image.png)

The key tables in the data model include:
- **Calendar Lookup**
- **Customer Lookup**
- **Territory Lookup**
- **Product Categories Lookup**
- **Product Subcategories Lookup**
- **Sales Data**
- **Returns Data**
- **Rolling Calendar**
- **Measure Table**

## Measures (DAX)
Several DAX measures were created to support the analysis:
- % of All Orders
```Dax
% of All Orders = 
DIVIDE(
    [Total Orders], 
    [All Orders]
)
  ```
- % of All Returns
```Dax
% of All Returns = 
DIVIDE(
    [Total Returns],
    [All Returns]
)
```
- 10 Day Rolling Revenue
```Dax
10-day Rolling Revenue = 
CALCULATE(
    [Total Revenue],
    DATESINPERIOD(
        'Calendar Lookup'[Date],
        MAX(
            'Calendar Lookup'[Date]
        ),
        -10,
        DAY
    )
)
```
- 90 Day Rolling Profit
```Dax
90-day Rolling Profit = 
CALCULATE(
    [Total Profit],
    DATESINPERIOD(
        'Calendar Lookup'[Date],
        LASTDATE(
            'Calendar Lookup'[Date]
        ),
        -90,
        DAY
    )
)
```
- Adjusted Price
```Dax
Adjusted Price = [Average Retail Price] * (1 + 'Price Adjustment (%)'[Price Adjustment (%) Value])
```
- Adjusted Profit
```Dax
Adjusted Profit = 
[Adjusted Revenue] - [Total Cost]
```
- Adjusted Revenue
```Dax
Adjusted Revenue = 
SUMX(
    'Sales Data',
    'Sales Data'[OrderQuantity]
    *
   [Adjusted Price])
```
- All Orders
```dax
All Orders = 
CALCULATE(
    [Total Orders], 
    ALL(
        'Sales Data'
    )
)
```
- All Returns
```dax
All Returns = 
CALCULATE(
    [Total Returns],
    ALL(
        'Returns Data'
    )
)
```
- Average Retail Price
```dax
Average Retail Price = 
AVERAGE(
    'Product Lookup'[ProductPrice]
)
```
- Average Revenue per Customer
```dax
Average Revenue per Customer = 
DIVIDE(
    [Total Revenue], 
    [Total Customers]
)
```
- Bike Return Rate
```dax
Bike Return Rate = 
CALCULATE(
    [Return Rate],
    'Product Categories Lookup'[CategoryName] = "Bikes"
)
```
- Bike Returns
```
Bike Returns = 
CALCULATE(
    [Quantity Returned],
    'Product Categories Lookup'[CategoryName] = "Bikes"
)
```

- Bike Sales
```Dax
Bike Sales = 
CALCULATE(
    [Quantity Sold],
    'Product Categories Lookup'[CategoryName] = "Bikes"
)
```
- Bulk Orders
```Dax
Bulk Orders = 
CALCULATE(
    [Total Orders],
    'Sales Data'[OrderQuantity] > 1
)
```

- High Ticket Orders
```Dax
High Ticket Orders = 
CALCULATE(
    [Total Orders],
    FILTER(
        'Product Lookup',
        'Product Lookup'[ProductPrice] > [Overall Average Price]
    )
)
```
- Order Target
```dax
Order Target = 
[Previous Month Orders] * 1.1
```
- Order Target Gap
```dax
Order Target Gap = [Total Orders] - [Order Target]
```
- Overall Average Price
```dax
Overall Average Price = 
CALCULATE(
    [Average Retail Price],
    ALL(
        'Product Lookup'
    )
)
```
- Previous Month Orders
```dax
Previous Month Orders = 
CALCULATE(
    [Total Orders],
    DATEADD(
        'Calendar Lookup'[Date],
        -1,
        MONTH
    )
)
```
- Previous Month Profit
```dax
Previous Month Profit = 
CALCULATE(
    [Total Profit],
    DATEADD(
        'Calendar Lookup'[Date],
        -1,
        MONTH
    )
)
```
- Previous Month Returns
```dax
Previous Month Returns = 
CALCULATE(
    [Total Returns],
    DATEADD(
        'Calendar Lookup'[Date],
        -1,
        MONTH
    )
)
```
- Previous Month Revenue
```dax
Previous Month Revenue = 
CALCULATE(
    [Total Revenue],
    DATEADD(
        'Calendar Lookup'[Date],
        -1,
        MONTH
    )
)
```
- Profit Target = 
```dax
Profit Target = 
[Previous Month Profit] * 1.1
```
- Profit Target Gap
```dax
Profit Target Gap = [Total Profit] - [Profit Target]
```
- Quantity Returned = 
```dax
Quantity Returned = 
SUM(
    'Returns Data'[ReturnQuantity]
)
```
- Quantity Sold = 
```dax
Quantity Sold = 
SUM(
    'Sales Data'[OrderQuantity]
)
```
- Return Rate
```dax
Return Rate = 
DIVIDE(
    [Quantity Returned],
    [Quantity Sold],
    "No Sales"
)
```
- Revenue Target
```dax
Revenue Target = 
[Previous Month Revenue] * 1.1
```
- Revenue Target Gap
```dax
Revenue Target Gap = [Total Revenue] - [Revenue Target]
```
- Total Cost
```dax
Total Cost = 
SUMX(
    'Sales Data',
    'Sales Data'[OrderQuantity]
    *
    RELATED(
        'Product Lookup'[ProductCost]
    )
)
```
- Total Customers
```dax
Total Customers = 
DISTINCTCOUNT(
    'Sales Data'[CustomerKey]
)
```
- Total Orders
```dax
Total Orders = 
DISTINCTCOUNT(
    'Sales Data'[OrderNumber]
)
```
- Total Profit
```dax
Total Profit = 
[Total Revenue] - [Total Cost]
```
- Total Returns
```dax
Total Returns = 
COUNT(
    'Returns Data'[ReturnQuantity]
)
```

- Total Revenue
```dax
Total Revenue = 
SUMX(
    'Sales Data',
    'Sales Data'[OrderQuantity]
    *
    RELATED(
        'Product Lookup'[ProductPrice]
    )
)
```
- Weekend Orders
```dax
Weekend Orders = 
CALCULATE(
    [Total Orders],
    'Calendar Lookup'[Weekend] = "Weekend"
)
```
-YTD Revenue
```dax
YTD Revenue = 
CALCULATE(
    [Total Revenue],
    DATESYTD(
        'Calendar Lookup'[Date]
    )
) 
```

## Dashboards
The project includes several dashboards to visualize the insights derived from the data. Key dashboards cover:
- Sales Performance
- Customer Demographics
- Product Category Analysis
- Return Rates

To add images of your dashboards, include them in the `images` directory and reference them as shown below:
```markdown
![Sales Performance Dashboard](./images/sales_performance_dashboard.png)
![Customer Demographics Dashboard](./images/customer_demographics_dashboard.png)
