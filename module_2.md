# Module 2: Tables and Wrangling

## Learning outcomes
- Demonstrate how to rename columns of a dataframe using [`.rename()`](#Rename-columns-using-rename).
- Create new or columns in a dataframe using [`.assign()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.assign.html) notation.
- Drop columns in a dataframe using [`.drop()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.drop.html)
- Use `df[]` notation to filter rows of a dataframe.
- Calculate summary statistics on grouped objects using [`.groupby()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.groupby.html#pandas.DataFrame.groupby) and [`.agg()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.agg.html#pandas.DataFrame.agg).
- Explain when chaining is appropriate.
- Demonstrate chaining over multiple lines and verbs.

## Rename columns using `.rename()`
```python
DataFrame.rename(mapper=None, *, index=None, columns=None, axis=None, copy=None, inplace=False, level=None, errors='ignore')
```

The [`.rename()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.rename.html) method in Pandas is a flexible way to rename the labels of an index or columns in a DataFrame or Series. It's particularly useful for making data more readable or for preparing data for further analysis by ensuring column names and index labels conform to a preferred naming convention.

This method can take a dictionary mapping old names to new names, a function that modifies the labels, or a combination of both. Changes can be made in place by setting the `inplace=True` parameter or can return a modified copy of the DataFrame/Series without altering the original data.

Here's a basic example to demonstrate its usage:

```python
import pandas as pd

# Create a simple DataFrame
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6],
    'C': [7, 8, 9]
})

# Rename columns
df.rename(columns={'A': 'Alpha', 'B': 'Beta'}, inplace=True)

print(df)
```
In this example, columns `A` and `B` are renamed to `Alpha` and `Beta`, respectively, using a dictionary to map old names to new names. The inplace=True parameter is used to modify the original DataFrame directly.

For a more detailed example:

Imagine you have a DataFrame containing user data, and you want to rename several columns to make them more descriptive. Additionally, you want to modify the index to be more informative.

```python
# Creating a DataFrame with user data
user_df = pd.DataFrame({
    'first_name': ['John', 'Jane', 'Doe'],
    'last_name': ['Doe', 'Doe', 'Smith'],
    'age': [28, 34, 23],
}, index=[101, 102, 103])

# Rename columns and index
user_df.rename(columns={'first_name': 'FirstName', 'last_name': 'LastName', 'age': 'Age'}, 
               index={101: 'User101', 102: 'User102', 103: 'User103'}, 
               inplace=True)

print(user_df)
```
In this detailed example, the columns `first_name`, `last_name`, and `age` are renamed to `FirstName`, `LastName`, and `Age` for clarity. The index labels are also changed from numeric IDs to string IDs (e.g., `101` to `User101`) to make them more descriptive. This makes the DataFrame easier to understand and work with, especially for further data analysis or presentation.

## Creating new columns using `.assign()`
The [`.assign()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.assign.html#pandas.DataFrame.assign) method 

in Pandas is used for adding new columns to a DataFrame in a chainable way. This means you can create or modify columns while keeping the DataFrame immutable, thus not modifying the original DataFrame in place. Instead, `assign()` returns a new DataFrame with the added columns, making it particularly useful for creating pipelines that require intermediate steps without altering the original data.

Here's a simple example to illustrate its usage:

```python
import pandas as pd

# Create a simple DataFrame
df = pd.DataFrame({
    'A': range(1, 5),
    'B': range(10, 50, 10)
})

# Use assign to create a new column 'C' as the sum of columns 'A' and 'B'
new_df = df.assign(C=lambda x: x['A'] + x['B'])

print(new_df)
```
In this example, `assign()` is used to create a new column `C` which is the sum of columns `A` and `B`. The lambda function `lambda x: x['A'] + x['B']` is used within `assign()` to perform row-wise operations for creating the new column. The original DataFrame df remains unchanged.

For a more detailed example:

Suppose you have a DataFrame containing sales data, and you want to add two new columns: one for the GST (General Service Tax) of each sale (assuming a flat rate of 5%) and another for the total amount including GST.

```python
# Creating a DataFrame with sales data
sales_df = pd.DataFrame({
    'ProductID': ['P001', 'P002', 'P003', 'P004'],
    'SalePrice': [100, 200, 150, 300]
})

# Use assign to add 'GST' and 'TotalPrice' columns
# GST is 5% of the 'SalePrice'
# 'TotalPrice' is the sum of 'SalePrice' and 'GST'
updated_sales_df = sales_df.assign(
    GST=lambda x: x['SalePrice'] * 0.05,
    TotalPrice=lambda x: x['SalePrice'] + x['GST']
)

print(updated_sales_df)
```
In this detailed example, two new columns are added:

- The `GST` column is calculated as 5% of the `SalePrice`, reflecting the GST rate in British Columbia.
- The `TotalPrice` column is the sum of `SalePrice` and `GST`, giving the total amount to be paid including the tax.

By using assign(), these new columns are added without altering the original sales_df DataFrame, and the operation is performed in a concise, chainable manner. This method enhances code readability and maintainability, especially in data transformation and preprocessing pipelines.


## Dropping columns with `.drop()`
The [`.drop()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.drop.html#pandas.DataFrame.drop) method in Pandas is used to remove rows or columns from a DataFrame. This method is highly versatile, allowing you to specify whether you want to drop labels from the index (rows) or columns by setting the `axis` parameter accordingly (`axis=0` for __rows__, `axis=1` for __columns__). You can also directly specify whether to drop labels from the index or columns using the `index` or `columns` parameters, respectively. By default, `drop()` does not modify the original DataFrame in place; instead, it returns a new DataFrame with the specified rows or columns removed, unless you set `inplace=True`.

Here's a basic example of its usage to drop columns:

```python
import pandas as pd

# Create a DataFrame
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6],
    'C': [7, 8, 9]
})

# Drop column 'B'
new_df = df.drop('B', axis=1)

print(new_df)
```
And here's how to drop rows by index labels:
```python
# Drop the row with index label 0
new_df = df.drop(index=0)

print(new_df)
```
For a more detailed example:

Suppose you have a DataFrame containing various metrics for a series of products, and you want to remove specific rows based on an index label or condition, and also drop unnecessary columns to streamline the dataset for analysis.

```python
# Creating a DataFrame with product metrics
metrics_df = pd.DataFrame({
    'ProductID': ['P001', 'P002', 'P003', 'P004'],
    'Sales': [250, 150, 320, 210],
    'Returns': [10, 20, 30, 40],
    'Revenue': [1000, 800, 1200, 900]
}, index=['A', 'B', 'C', 'D'])

# Drop the row for product 'B' and the 'Returns' column
updated_metrics_df = metrics_df.drop(index='B').drop('Returns', axis=1)

print(updated_metrics_df)
```
In this detailed example, the `drop()` method is first used to remove the row corresponding to product `B` by specifying its `index` label. Then, it's used again to drop the `Returns` column by setting `axis=1`. The operations are chained together, showcasing how `drop()` can be used flexibly to remove both rows and columns in a single statement. This method is essential for data cleaning and preprocessing, enabling you to easily remove irrelevant or unnecessary data from your DataFrame.


## Filtering columns
Filtering columns in a Pandas DataFrame using the `df[]` notation is straightforward and intuitive. This approach is commonly used for selecting a subset of columns from the DataFrame by specifying the column names directly within square brackets. If you want to select a single column, you can pass the column name as a string. To select multiple columns, you pass a list of column names.

Here's a basic example of selecting a single column:

```python
import pandas as pd

# Create a simple DataFrame
df = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie'],
    'Age': [25, 30, 35],
    'City': ['New York', 'Paris', 'London']
})

# Select the 'Name' column
name_column = df['Name']

print(name_column)
```
And here's how to select multiple columns:
```python
# Select the 'Name' and 'City' columns
subset = df[['Name', 'City']]

print(subset)
```
For a more contextual example:

Imagine you have a DataFrame containing detailed information about employees, including their ID, name, position, age, and department. You're interested in generating a report that only requires the employee's name and department.

```python
# Creating a DataFrame with employee information
employee_df = pd.DataFrame({
    'EmployeeID': ['E001', 'E002', 'E003', 'E004'],
    'Name': ['John Smith', 'Jane Doe', 'Emily Davis', 'Michael Johnson'],
    'Position': ['Software Engineer', 'Data Scientist', 'HR Manager', 'Product Manager'],
    'Age': [28, 34, 40, 30],
    'Department': ['Engineering', 'Data Science', 'Human Resources', 'Product']
})

# Select the 'Name' and 'Department' columns for the report
report_df = employee_df[['Name', 'Department']]

print(report_df)
```
In this example, by using the `df[]` notation with a list of column names, you can easily filter out the columns relevant to the report, namely `Name` and `Department`. This method of filtering is very effective for quickly accessing or transforming subsets of the data based on column names, making it a fundamental technique for data manipulation and analysis in Pandas.

## Calculate summary statistics on groups using `.groupby()`
The [`.groupby()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.groupby.html#pandas.DataFrame.groupby) method in Pandas is a cornerstone of data analysis, enabling the grouping of data based on one or more keys. It involves splitting the data into groups based on some criteria, applying a function to each group independently, and then combining the results into a data structure. This method is particularly useful for aggregate computations, such as summing data, computing averages, or applying statistical methods over grouped data.

Here's a basic example of its usage:

```python
import pandas as pd

# Create a simple DataFrame
df = pd.DataFrame({
    'Key': ['A', 'B', 'A', 'B'],
    'Data': [1, 2, 3, 4]
})

# Group the DataFrame by the 'Key' column and sum the grouped data
grouped = df.groupby('Key').sum()

print(grouped)
```
In this example, the DataFrame is grouped by the values in the `Key` column, and then the sum of the `Data` column for each group is calculated. The result is a new DataFrame with the unique `Key` values as the index and the summed `Data` as the column.

For a more detailed example:

Suppose you have a dataset containing sales information, including the date of sale, the product sold, and the revenue generated. You want to analyze the total revenue generated by each product.

```python
# Creating a DataFrame with sales data
sales_df = pd.DataFrame({
    'Date': ['2021-01-01', '2021-01-01', '2021-01-02', '2021-01-02'],
    'Product': ['WidgetA', 'WidgetB', 'WidgetA', 'WidgetB'],
    'Revenue': [100, 150, 200, 250]
})

# Group by 'Product' and sum the 'Revenue' for each product
total_revenue_by_product = sales_df.groupby('Product').sum()

print(total_revenue_by_product)
```
In this detailed example, the `groupby()` method is used to group the sales data by the `Product` column. Then, the sum of the `Revenue` for each product is computed, providing insights into the total revenue generated by each product across all the dates. This type of analysis is crucial for understanding sales performance, identifying best-selling products, and making informed business decisions.

The `groupby()` method is incredibly flexible, supporting various aggregation functions such as `mean()`, `median()`, `count()`, `max()`, and `min()`, among others. It's an essential tool for data exploration, allowing you to efficiently organize, summarize, and analyze your data based on specific grouping criteria.

## Create summary statistics using `.agg()`
The [`.agg()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.agg.html#pandas.DataFrame.agg) method in Pandas, short for "aggregate", is used in conjunction with `groupby()` to perform multiple aggregation operations on one or more columns of grouped data. It's a powerful tool for summarizing data, allowing you to apply different functions to different columns and perform multiple aggregations at once. This method enhances the flexibility of data analysis tasks by enabling concise, readable code for complex aggregations.

Here's a basic usage example:

```python
import pandas as pd

# Create a DataFrame
df = pd.DataFrame({
    'Category': ['A', 'A', 'B', 'B'],
    'Values': [10, 15, 10, 20],
    'OtherValues': [100, 150, 100, 200]
})

# Group the DataFrame by the 'Category' column
grouped = df.groupby('Category')

# Aggregate using different functions for each column
result = grouped.agg({
    'Values': ['sum', 'mean'],
    'OtherValues': 'max'
})

print(result)
```
In this example, the DataFrame is grouped by `Category`, and then `agg()` is used to apply multiple aggregation functions to different columns. For the `Values` column, both the sum and mean are calculated, whereas for the `OtherValues` column, only the maximum value is determined for each group.

For a more detailed example:

Imagine you have a dataset containing information about sales transactions, including the date, product, and revenue. You want to analyze this data to find the total revenue, average revenue, and maximum single transaction revenue for each product.

```python
# Creating a DataFrame with sales data
sales_df = pd.DataFrame({
    'Date': ['2021-01-01', '2021-01-01', '2021-01-02', '2021-01-02'],
    'Product': ['WidgetA', 'WidgetB', 'WidgetA', 'WidgetB'],
    'Revenue': [120, 200, 150, 300]
})

# Group the data by 'Product'
grouped_sales = sales_df.groupby('Product')

# Aggregate with specific functions for the 'Revenue' column
aggregated_sales = grouped_sales.agg({
    'Revenue': ['sum', 'mean', 'max']
})

print(aggregated_sales)
```
In this detailed example, the `agg()` method is used to calculate the total `(sum)`, average `(mean)`, and maximum `(max)` revenue for each product. This approach provides a comprehensive overview of the sales performance of each product, showcasing the versatility of `agg()` for conducting sophisticated data analysis with minimal code.




## Sort rows using `.sort_values()`
The [`.sort_values()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.sort_values.html) method in Pandas is utilized to sort a DataFrame or Series based on the values in one or more columns. This method offers flexibility in sorting, allowing ascending or descending order, and can handle missing values as specified. It's a fundamental tool for data analysis, enabling you to order your data in a meaningful way for inspection, reporting, or further analysis.

### Basic Usage
Here's a basic example of using sort_values() to sort a DataFrame:

```python
import pandas as pd

# Create a simple DataFrame
df = pd.DataFrame({
    'Name': ['Charlie', 'Alice', 'Bob'],
    'Age': [35, 25, 30]
})

# Sort the DataFrame by the 'Age' column in ascending order
sorted_df = df.sort_values(by='Age')

print(sorted_df)
```
This code snippet sorts the DataFrame by the `Age` column in `ascending` order (which is the default behavior).

### Sorting by Multiple Columns
You can also sort by multiple columns. This is useful for breaking ties in the values of the first column by using a second column, and so on.

```python
# Adding a new column for demonstration
df['JoiningYear'] = [2015, 2013, 2015]

# Sort by 'Age' in ascending order, then by 'JoiningYear' in descending order
sorted_df = df.sort_values(by=['Age', 'JoiningYear'], ascending=[True, False])

print(sorted_df)
```
This example first sorts the DataFrame by `Age` in `ascending` order and then sorts by `JoiningYear` in `descending` order for rows where `Age` is the same.

### Handling Missing Values

```python
DataFrame.sort_values(by, *, axis=0, ascending=True, inplace=False, kind='quicksort', na_position='last', ignore_index=False, key=None)
```

The `sort_values()` method also allows you to control how missing values (`NaN`) are sorted. By default, missing values are placed at the end, but you can change this behavior with the `na_position` argument.

```python
# Assume 'df' contains some missing values in 'Age'
# Sort 'Age' in ascending order, placing missing values at the beginning
sorted_df = df.sort_values(by='Age', na_position='first')

print(sorted_df)
```
### Sorting in Place
By default, sort_values() returns a new DataFrame. If you want to modify the original DataFrame, you can use the inplace=True parameter.

```python
# Sort the DataFrame in place by 'Age'
df.sort_values(by='Age', inplace=True)
```