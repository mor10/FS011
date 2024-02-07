# Module 1: Python & Pandas

## Learning Outcomes (ToC)
- [Read a standard `.csv` file using Pandas `pd.read_csv()`](#Reading-csv-files-with-pdread_csv).
- [Displaying rows using the `.head()` method](#Displaying-rows-using-the-head-method)
- Demonstrate indexing and slicing with [`.loc[]`](#Selecting-data-from-a-dataframe-using-loc) and [`.iloc[]`](#Selecting-data-from-a-dataframe-using-iloc).
- [Sort a dataframe using `.sort_values()`](#Sort-a-dataframe-using-sort_values).
- [Create simple summary statistics using `.describe()`](#Create-simple-summary-statistics-using-describe).
- Construct simple visualizations using Altair. (__to be added__)
- [Create a `.csv` file from a dataframe using `.to_csv()`](#Write-to-csv-files-using-to_csv).

## Reading `.csv` files with `pd.read_csv()`
```python
pandas.read_csv(filepath_or_buffer, *, sep=_NoDefault.no_default, delimiter=None, header='infer', names=_NoDefault.no_default, index_col=None, usecols=None, dtype=None, engine=None, converters=None, true_values=None, false_values=None, skipinitialspace=False, skiprows=None, skipfooter=0, nrows=None, na_values=None, keep_default_na=True, na_filter=True, verbose=_NoDefault.no_default, skip_blank_lines=True, parse_dates=None, infer_datetime_format=_NoDefault.no_default, keep_date_col=_NoDefault.no_default, date_parser=_NoDefault.no_default, date_format=None, dayfirst=False, cache_dates=True, iterator=False, chunksize=None, compression='infer', thousands=None, decimal='.', lineterminator=None, quotechar='"', quoting=0, doublequote=True, escapechar=None, comment=None, encoding=None, encoding_errors='strict', dialect=None, on_bad_lines='error', delim_whitespace=_NoDefault.no_default, low_memory=True, memory_map=False, float_precision=None, storage_options=None, dtype_backend=_NoDefault.no_default
```

>Read a comma-separated values (csv) file into DataFrame.
>
> Also supports optionally iterating or breaking of the file into chunks.

[`pd.read_csv()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html) is a function from the Pandas library in Python, widely used for reading a CSV (Comma Separated Values) file into a Pandas DataFrame. This function is incredibly versatile, offering numerous parameters to handle different types of CSV files, including those with various delimiters, quoting characters, and file encodings. A DataFrame is a two-dimensional, size-mutable, and potentially heterogeneous tabular data structure with labeled axes (rows and columns). This makes `pd.read_csv()` a fundamental tool in data science for data loading and preprocessing.

Here's a simple prototype example to demonstrate its basic usage:
```python
import pandas as pd

# Reading a CSV file into a DataFrame
df = pd.read_csv('path/to/your/file.csv')

# Display the first few rows of the DataFrame
print(df.head())
```
Let's delve into a more contextual example:

Imagine you have a CSV file named sales_data.csv that contains sales information with columns for Date, Product, Price, and Quantity Sold. You want to load this data, inspect the first few rows, and then perform some basic analysis, such as calculating the total sales.

```python
import pandas as pd

# Load the CSV file
sales_df = pd.read_csv('sales_data.csv')

# Display the first 5 rows to inspect the data
print(sales_df.head())

# Assuming 'Price' and 'Quantity Sold' are columns in the CSV,
# calculate total sales
sales_df['Total Sales'] = sales_df['Price'] * sales_df['Quantity Sold']

# Display the first 5 rows with the new 'Total Sales' column
print(sales_df.head())
```
In this example, `pd.read_csv()` is used to load the sales data into a DataFrame, providing a convenient way to work with the data using Python. The `.head()` method is then used to peek at the first few rows of the DataFrame to ensure it's loaded correctly. Finally, a new column, Total Sales, is created by multiplying the Price and Quantity Sold columns, demonstrating how Pandas can be used for data manipulation.

## Displaying rows using the `.head()` method
```python
DataFrame.head(n=5)
```

> Return the first n rows.
>
> This function returns the first n rows for the object based on position. It is useful for quickly testing if your object has the right type of data in it.
>
> For negative values of n, this function returns all rows except the last |n| rows, equivalent to df[:n].
>
> If n is larger than the number of rows, this function returns all rows.

The [`.head()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.head.html) method in Pandas is used to return the first n rows of a DataFrame or Series, where `n` defaults to `5` if not specified. This method is particularly useful for getting a quick overview of the dataset, especially when working with large datasets where displaying the entire data at once is impractical. It allows data scientists and analysts to quickly check the structure and the first few entries of the data without loading everything into the memory, which can be both time-consuming and resource-intensive.

Here's a basic example of its usage:

```python
import pandas as pd

# Assuming df is a Pandas DataFrame
df = pd.DataFrame({
    'A': [1, 2, 3, 4, 5, 6],
    'B': ['a', 'b', 'c', 'd', 'e', 'f']
})

# Display the first 5 rows of the DataFrame
print(df.head())
```
This will output:
```
   A  B
0  1  a
1  2  b
2  3  c
3  4  d
4  5  e
```
To display a different number of rows, you can pass an integer to `head()`. For example, `df.head(3)` will display the first 3 rows of the DataFrame.

Let's explore a more detailed example:

Suppose you have a DataFrame sales_data containing several months of sales data for a retail store. You're interested in quickly checking the format of the data and the first few records to ensure it has loaded correctly and to understand its structure.

```python
import pandas as pd

# Example sales data
data = {
    'Date': ['2021-01-01', '2021-01-02', '2021-01-03', '2021-01-04'],
    'Product': ['WidgetA', 'WidgetB', 'WidgetA', 'WidgetC'],
    'Price': [20, 30, 20, 50],
    'Quantity Sold': [5, 2, 7, 3]
}

sales_data = pd.DataFrame(data)

# Use head() to display the first 3 rows
print(sales_data.head(3))
```
Output:
```
         Date  Product  Price  Quantity Sold
0  2021-01-01  WidgetA     20              5
1  2021-01-02  WidgetB     30              2
2  2021-01-03  WidgetA     20              7
```
In this example, head(3) is used to quickly inspect the first three entries of the sales_data DataFrame, allowing you to verify the data's structure, column names, and a few values. This is an essential step in the initial data analysis phase, helping to ensure that subsequent data manipulation and analysis tasks proceed smoothly.

## Selecting data from a dataframe using `.loc[]`
```python
property DataFrame.loc
```

> Access a group of rows and columns by label(s) or a boolean array.
>
> .loc[] is primarily label based, but may also be used with a boolean array.
>
> Allowed inputs are:
>
> - A single label, e.g. `5` or `'a'`, (note that `5` is interpreted as a _label_ of the _index_, and __never__ as an integer position along the index).
> - A list or array of labels, e.g. `['a', 'b', 'c']`.
> - A slice object with labels, e.g. `'a':'f'`.
> - A boolean array of the same length as the axis being sliced, e.g. `[True, False, True]`.
> - An alignable boolean Series. The index of the key will be aligned before masking.
> - An alignable Index. The Index of the returned selection will be the input.
> - A `callable` function with one argument (the calling Series or DataFrame) and that returns valid output for indexing (one of the above)

The [`.loc[]`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.loc.html) accessor in Pandas is a powerful tool for selecting data from a DataFrame. It allows for selecting rows and columns by labels. You can use it to access a single value, a series of values, rows, columns, or even slices of the DataFrame based on row and column labels. The syntax for `loc[]` includes specifying the row labels first, followed by the column labels, separated by a comma.

Here's a basic example to illustrate its usage:

```python
import pandas as pd

# Create a simple DataFrame
df = pd.DataFrame({
   'Name': ['Alice', 'Bob', 'Charlie'],
   'Age': [25, 30, 35],
   'City': ['New York', 'Paris', 'London']
}, index=['A', 'B', 'C'])

# Access a single value
print(df.loc['A', 'Name'])

# Access a row
print(df.loc['B'])

# Access a column
print(df.loc[:, 'Age'])

# Access a subset of rows and columns
print(df.loc[['A', 'C'], ['Name', 'City']])
```
This code demonstrates different ways to use `loc[]`:

- To access a single value, you specify the row label and the column label.
- To access a full row, you only specify the row label.
- To access a full column, you use : to select all rows and then specify the column label.
- To access a subset, you provide lists of row labels and column labels.

Now, let's look at a more contextual example:

Suppose you have a dataset of employee information with their EmployeeID as the index. You want to select specific information about certain employees and specific columns related to their profile.

```python
# Creating a DataFrame with EmployeeID as index
employee_df = pd.DataFrame({
    'Name': ['John Doe', 'Jane Smith', 'Emily Davis', 'Michael Brown'],
    'Position': ['Software Engineer', 'Data Scientist', 'HR Manager', 'Product Manager'],
    'Age': [28, 34, 40, 32],
    'Salary': [95000, 105000, 78000, 98000]
}, index=['E001', 'E002', 'E003', 'E004'])

# Select data for employee E002 and E004 including Name and Salary
selected_info = employee_df.loc[['E002', 'E004'], ['Name', 'Salary']]

print(selected_info)
```
In this example, `loc[]` is used to select rows for employees with IDs `E002` and `E004` and columns `Name` and `Salary`. This approach is particularly useful when dealing with large datasets and you need to extract specific pieces of information based on row or column labels, offering both flexibility and precision in data selection and manipulation.

## Selecting data from a dataframe using `.iloc[]`
```python
property DataFrame.iloc
```

>Purely integer-location based indexing for selection by position.
>
> `.iloc[]` is primarily integer position based (from `0` to `length-1` of the axis), but may also be used with a boolean array.
>
> Allowed inputs are:
> - An integer, e.g. `5`.
> - A list or array of integers, e.g. `[4, 3, 0]`.
> - A slice object with ints, e.g. `1:7`.
> - A boolean array.
> - A `callable` function with one argument (the calling Series or DataFrame) and that returns valid output for indexing (one of the above). This is useful in method chains, when you don’t have a reference to the calling object, but would like to base your selection on some value.
> - A tuple of row and column indexes. The tuple elements consist of one of the above inputs, e.g. `(0, 1)`.
>
> `.iloc` will raise `IndexError` if a requested indexer is out-of-bounds, except _slice_ indexers which allow out-of-bounds indexing (this conforms with python/numpy _slice_ semantics).

The [`iloc[]`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.iloc.html) accessor in Pandas is utilized for integer-location based indexing, allowing for selections by the integer positions of the rows and columns. This method is particularly useful when you need to access elements by their index positions without referring to their labels. `iloc[]` works similarly to Python's standard list slicing, thus it supports a range of integer-based options, including individual integers, lists of integers, or even slice objects.

Here's a basic example to demonstrate its usage:
```python
import pandas as pd

# Create a simple DataFrame
df = pd.DataFrame({
   'Name': ['Alice', 'Bob', 'Charlie'],
   'Age': [25, 30, 35],
   'City': ['New York', 'Paris', 'London']
})

# Access a single value
print(df.iloc[0, 1])  # Age of the first row (Alice)

# Access a row
print(df.iloc[1])  # All details about Bob

# Access a column
print(df.iloc[:, 2])  # All values from the 'City' column

# Access a subset of rows and columns
print(df.iloc[0:2, 0:2])  # First two rows and first two columns
```
In these examples, iloc[] is used in various ways to select data:

- To access a single value, both the row and column positions are specified.
- To access an entire row, only the row position is specified, and : is used to imply all columns.
- To access an entire column, : is used to select all rows, followed by the column position.
- To access a specific subset, ranges for rows and columns are specified using slice notation.

Now, let's explore a more detailed example:

Imagine you have a dataset stored in a DataFrame student_grades with several students and their grades for different subjects. You want to select grades for a particular set of students and subjects based on their positions in the DataFrame.

```python
# Creating a DataFrame for student grades
student_grades = pd.DataFrame({
    'Math': [88, 92, 79, 85],
    'Science': [94, 77, 88, 92],
    'English': [89, 80, 95, 93]
})

# Select grades for the first and third students in Math and English
selected_grades = student_grades.iloc[[0, 2], [0, 2]]

print(selected_grades)
```
In this example, `iloc[]` is used to select specific rows (first and third students) and columns (Math and English) by their integer positions. This method is essential when the row or column labels are either unknown or irrelevant to the task at hand, allowing for efficient data selection based purely on positional indexing.

## Sort a dataframe using `.sort_values()`
```python
DataFrame.sort_values(by, *, axis=0, ascending=True, inplace=False, kind='quicksort', na_position='last', ignore_index=False, key=None)
```
> Sort by the values along either axis.

The [`sort_values()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.sort_values.html) method in Pandas is used to sort a DataFrame or Series by the values along either the rows or columns. This function is highly flexible, allowing you to sort the data by one or more columns in ascending or descending order. Sorting is an essential operation when you are analyzing data, as it helps in understanding the data better, preparing it for further analysis, or improving its presentation.

Here's a basic example of its usage with a DataFrame:

```python
import pandas as pd

# Create a simple DataFrame
df = pd.DataFrame({
    'Name': ['Charlie', 'Alice', 'Bob'],
    'Age': [35, 25, 30],
    'Salary': [60000, 80000, 70000]
})

# Sort the DataFrame by the 'Name' column in ascending order
sorted_df = df.sort_values(by='Name')

print(sorted_df)
```
In this example, the DataFrame `df` is sorted by the `Name` column. By default, `sort_values()` sorts the data in _ascending_ order. To sort in descending order, you can set the `ascending` parameter to `False`.

You can also sort by multiple columns. This is useful when you want to sort the DataFrame based on a primary column, then further sort rows that have the same value in the primary column by a secondary column, and so on.

Here's an example:

```python
# Sort the DataFrame first by 'Age' in ascending order, then by 'Salary' in descending order
sorted_df = df.sort_values(by=['Age', 'Salary'], ascending=[True, False])

print(sorted_df)
```
In this more detailed example, the DataFrame is sorted by `Age` in ascending order. For rows where the `Age` is the same, the sorting is then determined by `Salary` in descending order. This multi-level sorting is specified by passing a list of column names to the `by` parameter and a corresponding list of boolean values to the `ascending` parameter, indicating the sort order for each column.

The `sort_values()` method is a powerful tool for organizing your data, making it easier to analyze and visualize. Whether you're preparing your data for analysis, looking for specific patterns, or simply trying to get a better understanding of your dataset, sorting is an essential step in the data manipulation process.

## Create simple summary statistics using `.describe()`
```python
DataFrame.describe(percentiles=None, include=None, exclude=None)
```

> Generate descriptive statistics.
>
> Descriptive statistics include those that summarize the central tendency, dispersion and shape of a dataset’s distribution, excluding NaN values.
>
> Analyzes both numeric and object series, as well as DataFrame column sets of mixed data types. The output will vary depending on what is provided. Refer to the notes below for more detail.

The [`describe()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.describe.html) method in Pandas is a convenient tool for generating descriptive statistics that summarize the central tendency, dispersion, and shape of a dataset's distribution, excluding `NaN` values. It is primarily used on numerical columns but can also be applied to object-type columns to provide summary statistics such as count, unique, top, and freq. For numerical data, the summary statistics include count, mean, standard deviation, minimum, maximum, and the quartiles of the data.

Here's a basic example of its usage:

```python
import pandas as pd
import numpy as np

# Create a DataFrame with both numeric and object columns
df = pd.DataFrame({
    'A': [1, 2, 3, 4, 5],
    'B': [10, 20, np.nan, 40, 50],
    'C': ['foo', 'bar', 'foo', 'bar', 'foo']
})

# Use describe() to get the summary statistics of the DataFrame
print(df.describe())

# For object type data, use describe with include parameter
print(df.describe(include=[object]))
```
The first call to `describe()` will by default return the summary statistics for numerical columns, such as count, mean, std (standard deviation), min, 25% (first quartile), 50% (median), 75% (third quartile), and max.

The second call to `describe()` with the `include=[object]` parameter will return summary statistics for columns of type `object`, such as count, unique (number of unique values), top (most common value), and freq (most common value's frequency).

Now, let's delve into a more contextual example:

Suppose you have a dataset containing information about a group of people's ages and salaries, along with their job titles. You're interested in getting a quick overview of the numeric information such as ages and salaries, and also understanding the diversity of job titles.

```python
# Creating a DataFrame with mixed data types
people_df = pd.DataFrame({
    'Age': [25, 30, 35, 40, 45],
    'Salary': [50000, 60000, 75000, 80000, 90000],
    'Job Title': ['Engineer', 'Designer', 'Manager', 'Engineer', 'Manager']
})

# Summary statistics for numeric columns
print(people_df.describe())

# Summary statistics for the object column
print(people_df.describe(include=[object]))
```
In this example, `describe()` first provides an overview of the age and salary distributions, giving insights such as the average age and salary, the range (min to max), and the quartiles. Then, by including object types in the `describe()` call, you get a summary of the `Job Title` column, which shows how many unique job titles there are, the most common job title, and its frequency. 

## Write to `.csv` files using `.to_csv()`
```python
DataFrame.to_csv(path_or_buf=None, *, sep=',', na_rep='', float_format=None, columns=None, header=True, index=True, index_label=None, mode='w', encoding=None, compression='infer', quoting=None, quotechar='"', lineterminator=None, chunksize=None, date_format=None, doublequote=True, escapechar=None, decimal='.', errors='strict', storage_options=None)
```

> Write object to a comma-separated values (csv) file.

The [`to_csv()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.to_csv.html) method in Pandas is used to write a DataFrame or Series to a comma-separated values (CSV) file. It provides a wide range of parameters to handle various CSV formatting options, such as specifying the delimiter, choosing the columns to write, and handling missing values. This method is essential for data scientists and analysts when they need to export Pandas DataFrames to CSV files, either for sharing data with others, further processing in other software, or simply for data storage.

Here's a simple example to demonstrate its basic usage:

```python
import pandas as pd

# Create a DataFrame
df = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie'],
    'Age': [25, 30, 35],
    'City': ['New York', 'Paris', 'London']
})

# Write the DataFrame to a CSV file
df.to_csv('my_dataframe.csv', index=False)
```
In this example, `df.to_csv('my_dataframe.csv', index=False)` writes the DataFrame `df` to a file named `my_dataframe.csv` in the current working directory. The `index=False` parameter is used to prevent Pandas from writing row indices into the CSV file. If you want the index to be included, you can omit this parameter or set it to `True`.

Let's explore a more detailed example:

Suppose you've performed some data cleaning and analysis on a dataset containing sales information and you want to export the cleaned data, including only specific columns and handling missing values by replacing them with a placeholder.

```python
# Creating a DataFrame with some sales data
sales_df = pd.DataFrame({
    'Date': ['2021-01-01', '2021-01-02', '2021-01-03', None],
    'Product': ['WidgetA', 'WidgetB', 'WidgetC', 'WidgetA'],
    'Price': [20.5, 30.75, 15.0, 22.5],
    'Quantity': [10, 5, None, 7]
})

# Replace missing values with a placeholder and export selected columns
sales_df.fillna({'Date': 'Unknown', 'Quantity': 0}, inplace=True)
sales_df.to_csv('cleaned_sales_data.csv', columns=['Date', 'Product', 'Quantity'], index=False)
```
In this more detailed example, `fillna()` is used to replace missing values in the `Date` and `Quantity` columns before exporting. The `to_csv()` method is then called with the columns parameter to specify that only the `Date`, `Product`, and `Quantity` columns should be included in the output file. The `index=False` argument is again used to exclude row indices from the CSV.

The `to_csv()` method is highly versatile, allowing for significant customization to meet your data export needs, making it an invaluable tool in the data science workflow for both data sharing and storage.