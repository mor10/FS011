# Module 3: Tidy Data and Joining Dataframes

## Learning outcomes (ToC)
- [Explain what tidy data is.](#Tidy-data)
- [Reshaping long-to-wide with `.pivot()`.](#Reshaping-with-pivot_table)
- [Reshaping (and preserving) long-to-wide with `.pivot_table()`](#Reshaping-with-pivot_table)
- [Reshaping wide-to-long with `.melt()`.](#Reshaping-with-melt)
- [Joining dataframes with `.merge()`](#Joining-Dataframes-Using-merge)
- [Joining dataframes with `.concat()`](#joining-using-concat)
- [Deciding between `.merge()` and `.concat()`](#choosing-between-merge-and-concat)
- [How to reset the index of a dataframe.](#how-to-reset-the-index-using-reset_index)


## Tidy data
Tidy data means 
- each row is a single _observation_
- each _variable_ is a single column
- each _value_ is a single cell

Tidy data is structured and consistent so it can be processed easily by multiple tools without having to reformat or filter the data.

Think one row for each thing, one column for each type of data about the thing, and one data point for each cell.

An example of untidy / non-tidy data is when two or more variables are mixed in a single column (eg both 'protein' and 'calories'). To make this data tidy, you have to first filter based on each variable.

### Statistical Questions and Tidy Data
The concept of Tidy data depends on whether the data is easy to process. 

You can modify (filter/transform) existing dataframes with more data or a different structure to make it tidy for your specific task: 

- Add a new column containing one data point for each observation. This produces a _wide_ dataframe (because more columns = more width).
- Reorganize a dataframe by pulling variables (columns) from the original and adding a new isolated observation based on the value of each column (effectively turning the data on its side). This produces a _long_ dataframe (because more instances of observations means more rows).

Example of long dataframe:
```
continent  country  2012  2013  2014  2015
Africa     Algeria  26.1  25.8  25.8  25.6
```

Turns into
```
continent  country  year  mort
Africa     Algeria  2012  26.1
Africa     Algeria  2013  25.8
Africa     Algeria  2014  25.8
Africa     Algeria  2015  25.6
```

## Reshaping with `.pivot()`
```python
DataFrame.pivot(*, columns, index=_NoDefault.no_default, values=_NoDefault.no_default)
```

> Return reshaped DataFrame organized by given index / column values.
>
> Reshape data (produce a “pivot” table) based on column values. Uses unique values from specified index / columns to form axes of the resulting DataFrame. This function does not support data aggregation, multiple values will result in a MultiIndex in the columns.

[`.pivot()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.pivot.html) can be used to transform long dataframes into wide dataframes. For example, if you have a long df with a `nutrition` column containing two variables - `calories` and `protein` - you can transform it into a wide df with a column for `calories` and another for `protein`.

So for this:
```
          name mfr nutrition  value
0    Special K   K  calories    110
1    Special K   K   protein      6
2  Apple Jacks   K  calories    110
3  Apple Jacks   K   protein      2
4  Raisin Bran   K  calories    120
5  Raisin Bran   K   protein      3
6     Cheerios   G  calories    110
7     Cheerios   G   protein      6
8     Wheaties   G  calories    100
9     Wheaties   G   protein      3
```

We can reshape to wide df using this:

`cereal_wide = cereal_long.pivot(index='name', columns='nutrition', values='value')`

- `index` - the new df index
- `columns` - the column to be split into new cols based on the containing values
- `values` - the values to be transferred into the new columns.

When using `.pivot()` the index is lost. 
To reassert the index, use `cereal_wide.reset_index()`

Doing so may pull a header to the index column. 
Clean that up using `cereal_wide.rename_axis('', axis='columns')` where the first argument is the new name and the `axis` argument is the target column.

### Breakdown
The `pivot()` function in Pandas is a powerful tool for reshaping data frames. It allows you to rearrange or transform data from a long format to a wide format, making it easier to analyze or display. The function primarily takes three arguments: `index`, `columns`, and `values`. Here’s a brief explanation followed by examples:

- `index`: This parameter specifies the columns to use as the new dataframe's index.
- `columns`: This indicates which column to use to create the new columns in the pivoted table.
- `values`: The cells of the new table are filled with the values from this column.

Here's a basic example:

```python
import pandas as pd

# Sample data
data = {
    'Date': ['2021-01-01', '2021-01-01', '2021-01-02', '2021-01-02'],
    'Type': ['A', 'B', 'A', 'B'],
    'Value': [10, 20, 30, 40]
}

df = pd.DataFrame(data)

# Pivot the DataFrame
pivot_df = df.pivot(index='Date', columns='Type', values='Value')
print(pivot_df)
```
In this simple example, we create a DataFrame with dates, types, and values, and then pivot it so that each type (A and B) becomes a separate column with values corresponding to each date.

Here's a more detailed example:

Imagine you are analyzing sales data for a store that sells two products (`A` and `B`). The sales records are kept daily, and you're interested in comparing the sales of products `A` and `B` side by side for each day.

```python
import pandas as pd

# Sample sales data
sales_data = {
    'Date': ['2021-01-01', '2021-01-01', '2021-01-02', '2021-01-02', '2021-01-03', '2021-01-03'],
    'Product': ['A', 'B', 'A', 'B', 'A', 'B'],
    'Sales': [100, 150, 200, 250, 300, 350]
}

df_sales = pd.DataFrame(sales_data)

# Pivot the sales DataFrame to compare product sales by date
pivot_sales = df_sales.pivot(index='Date', columns='Product', values='Sales')

print(pivot_sales)
```
After pivoting, the DataFrame `pivot_sales` will have dates as its index and two columns `A` and `B`, representing the sales of products `A` and `B` for each day. This transformation makes it straightforward to compare daily sales between the two products, enabling more efficient analysis and visualization. For instance, it's now easy to plot the sales trends of both products over time, identify which product performed better on specific dates, or calculate daily sales differences.

## Reshaping with `.pivot_table()`
```python
DataFrame.pivot_table(values=None, index=None, columns=None, aggfunc='mean', fill_value=None, margins=False, dropna=True, margins_name='All', observed=_NoDefault.no_default, sort=True)
```

> Create a spreadsheet-style pivot table as a DataFrame.
>
> The levels in the pivot table will be stored in MultiIndex objects (hierarchical indexes) on the index and columns of the result DataFrame.

`.pivot()` discards any column not being directly affected by the pivot (specified in the `index` argument).

Using [`.pivot_table()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.pivot_table.html) we can preserve the unaffected columns by adding them to the `index` argument:

`cereal_long.pivot_table(index=['name', 'mfr'], columns='nutrition', values='value')`

Remember to `.reset_index()` and `.rename_axis()`.

### Problem
If the `index` column targeted in a `.pivot()` has duplicate entries, we get a conflict. df can't be reshaped because of conflicting variable data in a single observation.

Using `.pivot()` on the same data causes the two conflicting values to be averaged in the new column. __This may or may not be what you want.__

### Solution
Use `.duplicated()` to check for duplicates in the original df before running `.pivot()` or `.pivot_table()`:

`cereal_problem.duplicated(subset=['name', 'nutrition'], keep=False)`

- `subset` - array of relevant columns (in this use case `index` and `column` arguments from the `.pivot()` method.
- `keep` - `true` shows only duplicate rows, `false` also shows original row.

Filter duplicate rows from original dataframe:

```python
duplicate_info =cereal_problem.duplicated(subset=['name', 'nutrition'], keep=False)
cereal_problem[duplicate_info]
```

To remove a duplicate, use `.drop()`:

`cereal_no_problem = cereal_problem.drop(axis=0, index=1)`

- `axis` - which axis to check, can be 0, 1, `index`, or `column`
- `index` - index number of row to be dropped.

### Expanded example

```python
# Convert the untidy data into tidy data using pivot_table 
# Assign set_num as your index and name the new dataframe tidied_lego

tidied_lego = (lego.pivot_table(
					index=['set_num','name','year'],
					columns='lego_info',
					values='value')
                   .reset_index()
              )

# Find the mean number of parts for each production year and save it in an object name year_parts_mean

year_parts_mean = tidied_lego.groupby(by='year').mean()[['num_parts']].round()

year_parts_mean
```

### Breakdown
The `pivot_table()` function in Pandas is an extremely versatile tool used for creating spreadsheet-style pivot tables as DataFrame objects. It provides a way to aggregate and summarize complex data sets, making it easier to analyze and extract insights. Compared to the `pivot()` function, `pivot_table()` is more powerful and flexible, especially when dealing with duplicate entries in the pivot index/columns. It can aggregate multiple values for the same index/column pairs using a specified aggregation function (e.g., sum, mean, etc.).

Parameters:
- `data`: The DataFrame to pivot.
- `values`: The column to aggregate, optional.
- `index`: The column to make the new frame’s index. If omitted, the existing index is used.
- `columns`: The column to make the new frame’s columns.
- `aggfunc`: The aggregation function to use (e.g., `sum`, `mean`).

Here's a basic example:

```python
import pandas as pd
import numpy as np

# Sample data
data = {
    'Date': ['2021-01-01', '2021-01-01', '2021-01-02', '2021-01-02'],
    'Type': ['A', 'B', 'A', 'B'],
    'Value': [10, 20, 30, 40]
}

df = pd.DataFrame(data)

# Create a pivot table
pivot_table_df = df.pivot_table(index='Date', columns='Type', values='Value', aggfunc=np.sum)
print(pivot_table_df)
```
This example aggregates the values by summing them up for each date and type combination, effectively handling scenarios where there might be multiple entries for each combination.

Now for a more built-out example:

Suppose you're analyzing a dataset of sales transactions that includes multiple transactions per day for various products. You want to understand the average daily sales for each product.

```python
import pandas as pd

# Sample sales data
sales_data = {
    'Date': ['2021-01-01', '2021-01-01', '2021-01-02', '2021-01-02', '2021-01-01', '2021-01-02'],
    'Product': ['A', 'B', 'A', 'B', 'A', 'B'],
    'Sales': [100, 150, 200, 250, 300, 350]
}

df_sales = pd.DataFrame(sales_data)

# Pivot table to find average sales by product for each day
pivot_sales_avg = df_sales.pivot_table(index='Date', columns='Product', values='Sales', aggfunc='mean')

print(pivot_sales_avg)
```
In this scenario, the `pivot_table()` function calculates the average (`mean`) sales for each product (`A` and `B`) on each date. This is especially useful for datasets with multiple entries per group (e.g., multiple sales transactions for the same product on the same day), as it condenses the data into a more manageable form, allowing for easy analysis of trends, patterns, and differences between groups.

## Reshaping with `.melt()`
```python
DataFrame.melt(id_vars=None, value_vars=None, var_name=None, value_name='value', col_level=None, ignore_index=True)
```

> Unpivot a DataFrame from wide to long format, optionally leaving identifiers set.
>
> This function is useful to massage a DataFrame into a format where one or more columns are identifier variables (id_vars), while all other columns, considered measured variables (value_vars), are “unpivoted” to the row axis, leaving just two non-identifier columns, ‘variable’ and ‘value’.

[`.melt()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.melt.html) is the reverse of `.pivot()`. It reshapes _wide_ dfs to _long_ dfs.

```python
melted_cereal  = (cereal.melt(id_vars=['name', 'mfr'] , 
                              value_vars=['calories', 'protein'], 
                              var_name='nutrition', 
                              value_name='value')
                  )
```

- `id_vars` - main (identifying) column and other columns to keep
- `value_vars` - columns to be melted together
- `var_name` - new melted column name
- `value_name` - the value column corresponding to the new column

## Concatenation
Two different methods for joining dataframes:
- `pd.concat()` - sticking two dfs together
- `.merge()` - merging two dfs under common cols etc.

### Horizontal concatenation
`candy_nutrition = pd.concat([candy, candy2], axis=1)`

- `[df1, df2]` - the dfs to combine
- `axis` - `0` combine along row axis, `1` combine along col axis.

This can cause duplicate columns. To fix it:

`candy_nutrition.loc[:,~candy_nutrition.columns.duplicated()]`

- The `~` operator inverts the boolean values.
Translated: "Give me all columns except the duplicated ones."

### Vertical concatenation
`large_candybars = pd.concat([candy, candy_more], axis=0)`

This causes index chaos as new entries are added.

Reset index using `.reset_index(drop=True)`

- `drop=True` - discards original index

### Caveat
Don't concat dfs horizontally when the rows are out of order. This causes chaos.

### Breakdown
The `melt()` function in Pandas is used for transforming or reshaping data. It's the opposite of pivot operations like `pivot()` and `pivot_table()`. While pivot functions turn a long DataFrame into a wide format by creating new columns and aggregating, `melt()` takes wide data and melts it into a long format. This is particularly useful when you want to make your dataset longer (i.e., increase the number of rows) by moving values from columns into rows, effectively turning columns into two variables: one for the variable names and one for the values.

Parameters:
- `id_vars`: Columns to use as identifier variables.
- `value_vars`: Columns to unpivot. If not specified, uses all columns that are not set as `id_vars`.
- `var_name`: Name to use for the `variable` column. If `None` it uses `frame.columns.name` or `variable`.
- `value_name`: Name to use for the `value` column.

Basic example:
```python
import pandas as pd

# Sample data in wide format
data = {
    'Date': ['2021-01-01', '2021-01-02'],
    'A_Sales': [100, 200],
    'B_Sales': [150, 250]
}

df = pd.DataFrame(data)

# Melting the DataFrame
melted_df = pd.melt(df, id_vars=['Date'], value_vars=['A_Sales', 'B_Sales'], var_name='Product', value_name='Sales')

print(melted_df)
```
This transforms the DataFrame from a wide format (where `A_Sales` and `B_Sales` are separate columns) to a long format where there's a single `Sales` column with values for both `A` and `B`, and a `Product` column indicating the type of sale.

Contextual Example:

Imagine you have a dataset containing monthly sales data for different products (e.g., Product A, Product B) across several years, and you want to perform time series analysis or comparisons between products. The data is in a wide format, with each product's sales in separate columns for each month.

```python
import pandas as pd

# Sample wide-format data
data = {
    'Year': [2020, 2021],
    'Product_A': [1200, 1300],
    'Product_B': [800, 850]
}

df_wide = pd.DataFrame(data)

# Melting to long format
df_long = pd.melt(df_wide, id_vars=['Year'], value_vars=['Product_A', 'Product_B'], var_name='Product', value_name='Sales')

print(df_long)
```

After melting, the dataset is in a long format with one row for each product's sales per year, making it easier to compare sales across products or perform other analyses that benefit from a long-format dataset. This format is also preferable for many types of visualizations and statistical models that require data in a long format.

## Joining Dataframes Using `.merge()`
```python
DataFrame.merge(right, how='inner', on=None, left_on=None, right_on=None, left_index=False, right_index=False, sort=False, suffixes=('_x', '_y'), copy=None, indicator=False, validate=None)
```

> Merge DataFrame or named Series objects with a database-style join.
>
> A named Series object is treated as a DataFrame with a single named column.
>
> The join is done on columns or indexes. If joining columns on columns, the DataFrame indexes _will be ignored_. Otherwise if joining indexes on indexes or indexes on a column or columns, the index will be passed on. When performing a cross merge, no column specifications to merge on are allowed.

[`.merge()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.merge.html) can only combine dataframes horizontally.

`leftDataFrame.merge(rightDataFrame, left_on='left_column_name', right_on='right_column_name', how='inner', indicator=True)`

Note: `left_on` and `right_on` can be string or array

### `how` parameter
- `inner` - default - returns only rows present in both dfs.
- `outer` - returns all rows. Sets resulting empty cells to `NaN`. Result rows sorted as both, left, right.
- `left` - returns rows present in both dfs plus remaining rows in left.
- `right` - returns rows present in both dfs plus remaining rows in right.

### `indicator` parameter
- `False` - default - does nothing
- `True` - creates a new column `_merge` indicating `both`, `left_only`, or `right_only`

### Breakdown
The `merge()` function in Pandas is a powerful tool for combining two DataFrames based on common columns or indices, similar to SQL joins. It allows for inner, outer, left, and right joins, providing flexibility in how you want to combine your data based on shared keys.

Parameters:
- `left`: The DataFrame on the left side of the merge.
- `right`: The DataFrame on the right side of the merge.
- `on`: Column or index level names to join on. Must be found in both DataFrames.
- `how`: Specifies how to determine which keys are to be included in the resulting table. It can be `left`, `right`, `outer`, or `inner`.
- `left_on`: Columns from the left DataFrame to use as keys.
- `right_on`: Columns from the right DataFrame to use as keys.
- `suffixes`: A tuple of string suffixes to apply to overlapping columns but not to the key columns.

Here's a basic example:

```python
import pandas as pd

# Sample DataFrames
df1 = pd.DataFrame({'Key': ['K0', 'K1', 'K2', 'K3'],
                    'A': ['A0', 'A1', 'A2', 'A3'],
                    'B': ['B0', 'B1', 'B2', 'B3']})

df2 = pd.DataFrame({'Key': ['K0', 'K1', 'K2', 'K3'],
                    'C': ['C0', 'C1', 'C2', 'C3'],
                    'D': ['D0', 'D1', 'D2', 'D3']})

# Merge DataFrames on the 'Key' column
merged_df = pd.merge(df1, df2, on='Key')

print(merged_df)
```

This example merges `df1` and `df2` on the `Key` column, resulting in a DataFrame that combines the columns from both based on the shared key values.

A broader contextual example:

Imagine you are working with two datasets: one containing employee information (e.g., Employee ID, Name) and another containing employee salaries (e.g., Employee ID, Salary). You want to combine these datasets to analyze employee costs.

```python
import pandas as pd

# Employee information DataFrame
employee_info = pd.DataFrame({
    'EmployeeID': [101, 102, 103, 104],
    'Name': ['Alice', 'Bob', 'Charlie', 'David']
})

# Employee salary DataFrame
employee_salary = pd.DataFrame({
    'EmployeeID': [101, 102, 103, 105],
    'Salary': [50000, 60000, 55000, 65000]
})

# Merge the DataFrames on 'EmployeeID'
merged_employee_data = pd.merge(employee_info, employee_salary, on='EmployeeID', how='inner')

print(merged_employee_data)
```

In this scenario, merging on `EmployeeID` with an `inner` join means the resulting DataFrame will only include employees found in both `employee_info` and `employee_salary`. This is useful for ensuring that you're only working with complete records where you have both the employee information and their salary. An `outer` join could be used instead if you wanted to retain all employees, filling in missing values from either table with `NaNs`, providing a comprehensive view of the data despite some missing elements.

## Joining using `.concat()`
```python
pandas.concat(objs, *, axis=0, join='outer', ignore_index=False, keys=None, levels=None, names=None, verify_integrity=False, sort=False, copy=None)
```

> Concatenate pandas objects along a particular axis.
>
> Allows optional set logic along the other axes.
>
> Can also add a layer of hierarchical indexing on the concatenation axis, which may be useful if the labels are the same (or overlapping) on the passed axis number.

The [`concat()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.concat.html) function in Pandas is a versatile tool used to concatenate pandas objects along a particular axis (either rows or columns). This function is useful for combining data from different sources, merging datasets with a similar structure, or simply adding rows or columns to an existing DataFrame. It can handle multiple objects at once and offers various options to specify how the concatenation should be performed.

Parameters:
- `objs`: A sequence or mapping of Series or DataFrame objects to be concatenated.
- `axis`: The axis to concatenate along. 0 means to concatenate along rows (default), and 1 means to concatenate along columns.
- `join`: Specifies how to handle indexes on other axes. Can be `inner` or `outer` (default is `outer`).
- `ignore_index`: If True, do not use the index values along the concatenation axis. The resulting axis will be labeled `0`, `1`, ..., `n-1`. This is useful if you want to concatenate objects where the concatenation axis does not have meaningful indexing information.

Here's a basic example:

```python
import pandas as pd

# Sample DataFrames
df1 = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
                    'B': ['B0', 'B1', 'B2', 'B3']})

df2 = pd.DataFrame({'A': ['A4', 'A5', 'A6', 'A7'],
                    'B': ['B4', 'B5', 'B6', 'B7']})

# Concatenate DataFrames along rows
concatenated_df = pd.concat([df1, df2], axis=0)

print(concatenated_df)
```

This example concatenates `df1` and `df2` along rows, effectively stacking them on top of each other.

A more contextual example:

Imagine you're analyzing monthly sales data for two different years, and the data for each year is stored in separate DataFrames with the same structure (columns for months and sales). You want to combine these DataFrames into a single one for a comprehensive analysis over the two years.

```python
import pandas as pd

# Sales data for 2020
sales_2020 = pd.DataFrame({
    'Month': ['Jan', 'Feb', 'Mar', 'Apr'],
    'Sales': [200, 240, 300, 310]
})

# Sales data for 2021
sales_2021 = pd.DataFrame({
    'Month': ['Jan', 'Feb', 'Mar', 'Apr'],
    'Sales': [220, 250, 320, 330]
})

# Concatenate sales data along rows
combined_sales = pd.concat([sales_2020, sales_2021], axis=0, ignore_index=True)

print(combined_sales)
```

In this case, concatenating along rows and setting `ignore_index=True` creates a new DataFrame with sales data from both years, with a continuous index. This setup is particularly useful when you need to append datasets with identical columns or to compile a comprehensive dataset from segmented parts.

## Choosing between `.merge()` and `.concat()`
Choosing between `concat()` and `merge()` in Pandas depends on the structure of your data and what you're trying to achieve with it. Both functions are designed to combine data, but they serve different purposes and operate in distinct ways.

### Use `concat()` when:

- **You want to stack multiple DataFrames vertically (row-wise concatenation) or horizontally (column-wise concatenation).** This is particularly useful when you have data in similar formats that you want to append to each other. For example, stacking monthly sales data from different months or years into a single DataFrame.
- **You need a simple way to combine data without needing to align on specific columns.** `concat()` is great for situations where you just want to "stack" data together without worrying about matching keys or indexes.
- **You are dealing with data that shares the same columns or rows.** `concat()` can handle situations where the DataFrames have the same columns but different rows (or vice versa) and you simply want to join them together.

### Use `merge()` when:

- **You want to combine DataFrames based on one or more common keys (similar to SQL joins).** This is essential for when you're trying to enrich one dataset with information from another based on a common column(s), such as joining a customer orders DataFrame with a customer details DataFrame based on customer ID.
- **You need more complex join operations.** `merge()` allows for inner, outer, left, and right joins, giving you the flexibility to specify exactly how you want non-matching rows to be handled.
- **You're looking for database-style joining or merging capabilities.** If your task involves aligning data based on keys and performing operations similar to those in relational databases, `merge()` is the more appropriate choice.

### Summary:

- **Use `concat()` for straightforward, axis-aligned concatenation tasks** where the goal is to append or stack DataFrames on top of each other or side by side, without the need for aligning on specific column values.

- **Use `merge()` for complex merging and joining operations** where you need to align DataFrames based on one or more keys and have control over how non-matching entries are handled.

In practice, the choice between `concat()` and `merge()` often comes down to whether your data combination task is primarily about appending datasets (`concat()`) or about relating datasets through shared keys (`merge()`).

## How to reset the index using `.reset_index()`
```python
DataFrame.reset_index(level=None, *, drop=False, inplace=False, col_level=0, col_fill='', allow_duplicates=_NoDefault.no_default, names=None)
```

> Reset the index, or a level of it.
>
> Reset the index of the DataFrame, and use the default one instead. If the DataFrame has a MultiIndex, this method can remove one or more levels.

The [`reset_index()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.reset_index.html) method in Pandas is used to reset the index of a DataFrame. This operation is particularly useful when you need to transform the index into a regular column or when you want to start the index count anew. It's often used after operations that result in an index composed of non-sequential numbers, such as after sorting values, filtering rows, or after some groupby operations where the index becomes a multi-index.

### Parameters:

- `drop`: If `True`, do not add the index column back to the DataFrame; otherwise, if `False` (the default), the index is added as a column.
- `level`: Specify the level in case of a MultiIndex to reset. If not provided, all levels are reset.
- `inplace`: If `True`, modifies the DataFrame in place (does not create a new object).
- `col_level`: If the columns have multiple levels, determines which level the labels are inserted into.
- `col_fill`: What to use to fill missing values in the case of a MultiIndex.

### Prototype Example

```python
import pandas as pd

# Creating a sample DataFrame
df = pd.DataFrame({'A': range(1, 6),
                   'B': range(10, 15)})
df.index = pd.date_range('20230101', periods=5)

# Resetting the index
reset_df = df.reset_index()

print(reset_df)
```

In this example, `reset_index()` is used to convert the DateTimeIndex into a regular column and create a new integer index starting from 0.

### Contextual Example

Imagine you have a DataFrame resulting from a groupby operation, which might have a MultiIndex (multiple levels of indexing based on the groupby keys). After performing your analysis, you decide you want a flat, tabular structure for visualization or further processing.

```python
import pandas as pd

# Sample data
data = {
    'Category': ['A', 'A', 'B', 'B', 'C', 'C'],
    'Values': [100, 150, 200, 145, 120, 165]
}

df = pd.DataFrame(data)

# Performing a groupby operation
grouped = df.groupby('Category').sum()

# Resetting the index after groupby
flat_df = grouped.reset_index()

print(flat_df)
```

In this scenario, `reset_index()` converts the `Category` index, which was the grouping key, back into a regular column. This allows for the DataFrame to be used in contexts where a simple, non-hierarchical structure is required, making data manipulation and visualization tasks more straightforward.