# Module 8: A Slice of NumPy and Advanced Data Wrangling

## Learning Goals (ToC)
- [NumPy](#numpy)
- [Arrays in Python](#arrays-in-python)
- [Making stepped array items using `np.linspace()`](#nplinspace)
- [Making random array items using `np.random.rand()`](#nprandomrand)
- [Elementwise operations](#elementwise-operations)
- [Boolean Indexing](#boolean-indexing)
- [DataFrames are arrays!](#dataframes-are-arrays)
- [Constants and functions](#constants-and-functions)
- [Multi-dimensional arrays](#multi-dimensional-arrays)
- [Array shapes](#array-shapes)
- [Indexing and slicing arrays](#indexing-and-slicing-2d-arrays)
- [Working with null values](#working-with-null-values)
- [Dates and Time](#dates-and-time)
- [String manipulation](#string-manipulation)

## NumPy
"NumPy" stands for "__Num__erical __Py__thon Extensions" and is a library helping do mathy things.

It has helper tools for dealing with multidimensional array objects.

It also does other stuff.

### The Breakdown
NumPy is a fundamental package for scientific computing in Python. It offers a powerful N-dimensional array object, sophisticated (broadcasting) functions, tools for integrating C/C++ and Fortran code, and useful linear algebra, Fourier transform, and random number capabilities. NumPy arrays provide much more efficient storage and data operations as the arrays grow larger in size, compared to Python lists. This efficiency makes NumPy an essential library for doing numerical computations and data analysis in Python.

### Prototype Example:

```python
import numpy as np

# Create a numpy array
arr = np.array([1, 2, 3, 4, 5])

# Print the array
print(arr)

# Print the type of the array
print(type(arr))
```

### Contextual Example:

Let's say you're working on a project where you need to perform mathematical operations on a set of numbers. You decide to use NumPy because of its efficiency with large data sets and its ability to perform element-wise operations.

```python
import numpy as np

# Creating a 2D array (matrix)
matrix = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])

# Compute the transpose of the matrix
transpose_matrix = matrix.T

# Calculate the mean of the matrix
mean_value = np.mean(matrix)

print(f"Original Matrix:\n{matrix}")
print(f"Transpose Matrix:\n{transpose_matrix}")
print(f"Mean Value of the Matrix: {mean_value}")
```

In this example, we first import NumPy and create a 2D array. We then use `matrix.T` to compute the transpose of the matrix, which flips the matrix over its diagonal. We also calculate the mean of all the elements in the matrix using `np.mean()`. This demonstrates how NumPy can easily handle common mathematical operations on arrays, making it a go-to library for numerical computations in Python.

## Arrays in Python
In Python, lists look like what we call arrays in other languages, except they are not. Le Sigh.

To create an array, you take a list and force it to become an array:

```python

my_array = np.array((1, 2, 3, 4, 5))
my_array # outputs array([1, 2, 3, 4, 5])

```

Why this difference? Because while lists can have a mix of data types in their items, arrays cannot. They Must All Be The Same!

If you try to mix types, the system tries to force them into one type - usually `str`.

```python
# Behold, the int that magically became a str:
my_array = np.array((1, "hi"))
my_array # returns array(['1', 'hi'], dtype='<U21')

```

You can make arrays from `list` objects and `tuple` objects

## NumPy array functions

There's a pattern of methods in NumPy you can use to autogenerate arrays. Examples:

```python
zeros = np.zeros(10) # produces ten items of 0
zeros # returns array([0., 0., 0., 0., 0., 0., 0., 0., 0., 0.])

ones = np.ones(4) #produces four items of 1
ones # returns array([1., 1., 1., 1.])
```

The `arange(start, stop, interval)` function ("A Range") produces an array with a range of items:

```python
linear = np.arange(5)
linear # returns array([0, 1, 2, 3, 4])

stepped = np.arange(0, 10, 2) 
stepped # returns array([0, 2, 4, 6, 8])
```

## `np.linspace()`
The `linspace(start, stop, steps)` function ("__Lin__early __Space__d") produces `steps` number of evenly spaced items between `start` and `stop`. The items default to `float`.

```python
spaced = np.linspace(1,5,10)
spaced # returns array([
       #   1.,
       #   1.44444444, 
       #   1.88888889, 
       #   2.33333333, 
       #   2.77777778, 
       #   3.22222222, 
       #   3.66666667, 
       #   4.11111111, 
       #   4.55555556,5.        
       # ])
```

### The Breakdown
The `np.linspace()` function in NumPy is used to create an array of evenly spaced values within a specified range. It is particularly useful when you need to generate a specific number of points between two values, for example, when you want to create a set of points for plotting a function over a specific interval. The function takes three primary arguments: the start value, the end value, and the number of points you want to generate between these two values. It returns a NumPy array of linearly spaced values.

### Prototype Example:

```python
import numpy as np

# Create an array of 10 linearly spaced points between 1 and 10
linear_spaced_array = np.linspace(1, 10, 10)

print(linear_spaced_array)
```

### Contextual Example:

Imagine you're plotting a mathematical function, such as a sine wave, and you want to generate a smooth curve. You would need a set of x-values that are evenly spaced over the interval of interest. Here's how you can use `np.linspace()` to achieve that:

```python
import numpy as np
import matplotlib.pyplot as plt

# Generate 100 linearly spaced points between 0 and 2π
x = np.linspace(0, 2 * np.pi, 100)

# Compute the sine of each x point
y = np.sin(x)

# Plot the sine wave
plt.plot(x, y)
plt.title('Sine Wave')
plt.xlabel('x')
plt.ylabel('sin(x)')
plt.grid(True)
plt.show()
```

In this example, `np.linspace()` generates 100 points between 0 and \(2\pi\), which are then used as the x-values for plotting a sine wave. This results in a smooth curve because the x-values are evenly spaced across the specified interval, and for each x-value, we compute `sin(x)` to get the corresponding y-value. The plot is generated using Matplotlib, showcasing the utility of `np.linspace()` in data visualization and mathematical computations.

## np.random.rand()
`np.random.rand(points, dimensions)` generates `points` number of random values between 0 and 1 across `dimensions` dimensions. The items default to `float`.

### The Breakdown
The `np.random.rand()` function in NumPy is used to generate an array of random numbers from a uniform distribution over the interval \([0, 1)\). The function can create an array of any shape with random floats, and the numbers are uniformly distributed, meaning each value within the specified range is equally likely to occur.

### Prototype Example:

```python
import numpy as np

# Generate a 1D array of 5 random numbers
random_array_1D = np.random.rand(5)

# Generate a 2D array of random numbers (3 rows, 4 columns)
random_array_2D = np.random.rand(3, 4)

print("1D Random Array:\n", random_array_1D)
print("2D Random Array:\n", random_array_2D)
```

### Contextual Example:

Let's say you're simulating a scenario where you need random samples for analysis, or perhaps you're initializing weights in a machine learning algorithm. Here's how you might use `np.random.rand()` in a practical context:

```python
import numpy as np
import matplotlib.pyplot as plt

# Generate a 2D array of random numbers (1000 points, 2 dimensions)
points = np.random.rand(1000, 2)

# Plot the points to visualize the uniform distribution
plt.scatter(points[:, 0], points[:, 1], alpha=0.5)
plt.title('Random Points from a Uniform Distribution')
plt.xlabel('X coordinate')
plt.ylabel('Y coordinate')
plt.grid(True)
plt.show()
```

In this example, `np.random.rand(1000, 2)` generates 1000 points (rows) in 2 dimensions (columns), where each point's coordinates are random numbers between 0 and 1. Plotting these points using Matplotlib's `scatter()` function visualizes how the points uniformly cover the unit square, demonstrating the function's utility in generating random samples for simulations or visualizations.

## Elementwise operations
In Python, arrays are for math apparently. Therefore, you can do weird stuff like "elementwise" operations meaning you do an operation on an array and it's done on each item (element) individually:

```python
array1 = np.ones(4)
array1 # returns array([1., 1., 1., 1.])

array2 = array1 + 1
array2 # returns array([2., 2., 2., 2.])
```

If you try to pull this stunt with a `list`, the world burns. To make it work you'd have to use a loop, which is annoying.

## Calling array items
Array items are called by naming the array and the index position.

Slicing an array works like `iloc[]`: The frist item is included, the last one is not:

```python
arr = np.arange(10) # array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
arr[7] # returns 7
arr[2,6] # returns array([2, 3, 4, 5])
arr[-1] # counts backwards. returns 9
```

## Boolean indexing
Because arrays do elementwise operations and Python is nuts, you can do stuff like this:

```python
grade_array = np.array([98, 87, 103, 92, 67, 107, 78, 104, 85, 105])
threshold = np.array([98, 87, 103, 92, 67, 107, 78, 104, 85, 105]) > 100
threshold # returns array([False, False,  True, False, False,  True, False,  True, False,  True])

# Replace all items that are true with 100 since that's the highest possible score:
grade_array[threshold] = 100
grade_array # returns array([ 98,  87, 100,  92,  67, 100,  78, 100,  85, 100])

# There's a shorthand for this last operation:
new_grade_array[new_grade_array > 100] = 100
```

Cool beans.

## DataFrames are Arrays
DataFrames are 2-dimensional arrays. In other words, Pandas is built using NumPy.

You can convert a column from a DataFrame to an array using the `.to_numpy()` method:

```python
cereal['calories'].to_numpy()
```

## Constants and functions
NumPy has baked-in constants and functions including:

- `np.pi` - Pi
- `np.inf` - Infinity
- `np.e` - mathematical e
- `np.prod()` - the product of values in an array
- `np.diff()` - the difference between element (left element subtracted from the right element)
- `np.log()` - logarithm
- `np.sin()` - sin

## Multi-dimensional Arrays
Multi-dimensional arrays allow you to create nested arrays of arrays - advanced data structures.

```python
list_2d = [[1, 2], [3, 4], [5, 6]]
array_2d = np.array(list_2d)
array_2d # returns array([[1, 2],
         # [3, 4],
         # [5, 6]])
```

The array functions allow you to create multi-dimensional arrays in a consistent way:

```python
np.zeros((3,4)) # 3 rows, 4 columns

np.random.rand(4, 2) # 4 rows, 2 columns

# Reshape a one-dimensional array to a multi-dimensional array:
np.arange(0,12).reshape(3,4)
```

`reshape(rows, columns)` allows you to reshape a 1D array to a multi-dimensional array.

## Array shapes
We use nouns (properties) to get info about the shape of an array:

- `.ndim`: the number of dimensions of an array
- `.shape`: the number of elements in each dimension (like calling `len()` on each dimension)
- `.size`: the total number of elements in an array (i.e., the product of `.shape`)

## Indexing and slicing 2D arrays
See the following example:

```python
arr2 = np.arange(0,12).reshape(3,4)
arr2 # returns array([[ 0,  1,  2,  3],
     #                [ 4,  5,  6,  7],
     #                [ 8,  9, 10, 11 ]])

arr2[1, 2] # returns 6 -> [row,col]
arr2[2] # returns row 2 -> array([ 8,  9, 10, 11])
arr2[:,2] # just like iloc[] returns column 2 -> array([ 2,  6, 10])
arr2[:2,1:] # first 2 rows, last 3 columns

# Set a value:
arr2[1,1] = 999
arr2 # returns array([[ 0,  1,  2,  3],
     #                [ 4,  999,  6,  7],
     #                [ 8,  9, 10, 11 ]])

```

`.T` for Transpose converts rows to columns and vice versa:

```python
arr2.T # returns array([[ 0,  4,  8],
       #                [ 1,  999,  9],
       #                [ 2,  6, 10],
       #                [ 3,  7, 11]])
```

## Working with null values
Null-values, aka `NaN` or `NA` values are empty values - "Not a Number". Computers don't like emptiness. It makes them stressed.

`df.info()` is used to check a DataFrame from `NaN` values. It outputs a "Non-null count" for each column.

It's good practice to start a project by checking `df.info()` so you know if you have some emptiness you need to fill.

If we find a column with null values, we can use `.isnull()` to identify the rows with null entries. `.isnull()` returns a Boolean series where `True` is a null value.

```python
# Find rows with null values in a specific column:
cycling[cycling['Distance'].isnull()]

# Find rows with null values in ANY column:
cycling[cycling.isnull().any(axis=1)]
```

The last example uses `.any()` to signify any column in the DataFrame. Duh.

### Dealing with null values

Two methods:

`.dropna()`: Drops any rows with null values in any column.
`.dropna(subset=['column'])` drops rows only in the 'column' column.

`.fillna()`: Fills the null value with whatever value we define.
`df.fillna(value=0)` fills the NaN values with the `float` `0.00`.

Introducing random data like `0.00` into your data may fuck up your data. You can also fill with other stuff, for example the mean of the other numbers in the column (neutral value):

```python
cycling_mean_fill = cycling.fillna(value=cycling['Distance'].mean().round(2))
```

You can also use built-in methods to solve this problem. For example:

```python
# Grab the next valid row observation and use it:
cycling.fillna(method='bfill')

# Grab the previous valid row observation and use it:
cycling.fillna(method='ffill')
```

## A fancy example from the exercises

```python
import pandas as pd

canucks = pd.read_csv('data/canucks.csv')

# Identify any columns with null values with .info()
# Save this dataframe as canucks_info

canucks_info = canucks.info()
canucks_info

# Create a new column in the dataframe named Wealth
# where all the values equal "comfortable"
# Name the new dataframe canucks_comf

canucks_comf = canucks.assign(Wealth = "comfortable")
canucks_comf

# Do conditional replacement, where if the value in the salary column is null,
# we replace "comfortable" with "unknown" 

canucks_comf.loc[canucks_comf['Salary'].isnull(), "Wealth"] = "unknown"
canucks_comf
```

## Dates and Time
Parsing dates as strings or objects is complex and tedious. That's why we never do that, in any programming language. Python is no different.

Pandas is partially built using the [`datetime`](https://docs.python.org/3/library/datetime.html) library.

When importing data into a DataFrame, we can use the `parse_dates` argument to parse the values of a column as `datetime64` data:

```python
cycling_dates = pd.read_csv('cycling_data.csv', parse_dates = ['Date'])
```

Once that's done the data is treated as true date and time and we can do things like sort based on the date.

```python
cycling_dates.sort_values('Date')
```

If the original data has the date and time split across multiple columns (eg month, date, year, time, etc) we can still import it using `parse_dates`, we just need to break it down some more:

```python
(pd.read_csv('cycling_data_split_time.csv',
              parse_dates={'Date': ['Year', 'Month', 'Day', 'Clock']})
              .head())
```

To convert existing data in a DataFrame to a datetime dtype, do use the `pd.to_datetime()` method like this:

```python
new_cycling = cycling.assign(Date = pd.to_datetime(cycling['Date']))
```

Unsurprisingly Pandas has a [ton of datetime tools](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#time-date-components) including:

- `.dt.year`
- `.dt.month`
- `.dt.month_name()`
- `.dt.day`
- `.dt.day_name()`
- `.dt.hour`
- `.dt.minute`

These are self-explanatory.

When you select an individual entry of the datetype dtype you get a timestamp. From that timestamp you can derive any information based on the tools above.

The `.diff()` method outputs the difference between two values. It can be used as below to output the difference in time between different entries:

```python
cycling_intervals = new_cycling['Date'].sort_values().diff()
```

This outputs a `timedelta64` series describing an interval of time. We can extract measurements from a `timedelta64` object as `days`, `seconds`, and `microseconds` like this:

```python
cycling_intervals[1].seconds
```

Time is just a number, so we can do things like summary statistics with `min()`, `max()`, etc on a time object.

### Reference example from the lesson

```python
DAYS_PER_YEAR = 365.25

# Read in the canucks.csv file from the data folder and parse the "Birth Date" column
# Save this as an object named canucks

canucks = pd.read_csv('data/canucks.csv', parse_dates = ['Birth Date'])

# Find the oldest player's date of birth 
# Save the Timstamp as oldest

oldest = canucks['Birth Date'].min()

# Find the youngest player's date of birth 
# Save the Timestamp as youngest

youngest = canucks['Birth Date'].max()

# Find the age difference between the two players in number of years to 2 decimal places
# Save this an an object name age_range

age_range = round((youngest - oldest).days/DAYS_PER_YEAR, 2)

# Display age_range

age_range
```

## String manipulation
[Full string documentation.](https://pandas.pydata.org/pandas-docs/stable/user_guide/text.html#method-summary)

Some string methods:

- `.upper()`: set to uppercase
- `.lower()`: set to lowercase
- `.count('i')`: how many 'i's
- `.split('i')`: split along 'i's into series
- `.capitalize()`: capitalizes the first word in a string
- `.title()`: capitalizes every word in a string
- `.strip()`: removes characters starting or ending a string - default to remove whitespace at front and/or back.

In a DataFrame, `str` entries have the `object` dtype.

To target every entry in a column in a DataFrame with a string manipulation, use the `.str.` prefix:

```python
upper_cycle = cycling.assign(Comments = cycling['Comments'].str.upper())
rain_cycle = upper_cycle.assign(Rain = upper_cycle['Comments'].str.count('RAIN'))
```

If you want to, you can use `.str.split()` to split each word into its own column:

```python
upper_cycle['Comments'].str.split(expand=True)
```

The `+` symbol can be used for string concatenation. This also applies when doing the column manipulation thing, in the same way as above.

```python
combined_cycle = cycling.assign(Distance_str = cycling['Distance'].astype('str') + ' km')
```

### Reference example from the lesson

```python
import pandas as pd

canucks = pd.read_csv('data/canucks.csv', parse_dates = ['Birth Date'])

# Convert the Position and Country columns into uppercase 
# Save this in a dataframe named canucks_upper

canucks_upper = canucks.assign(Position = canucks['Position'].str.upper(),
                               Country = canucks['Country'].str.upper())

# Create a new column in the canucks_upper dataframe named number_ts
# where you count the total number of times the letter T
# (lowercase or uppercase) appears in their name
# Save this  dataframe named as canucks_upper_ts

canucks_upper_ts = canucks_upper.assign(number_ts=canucks_upper['Player'].str.lower().str.count('t'))

# How many players have more than 1 letter T in their name? 

canucks_upper_ts[canucks_upper_ts['number_ts'] > 1].shape[0]
```

### Other useful methods
The `.replace(target,result)` method does what it sounds like

```python
cycling_rain = cycling_lower.assign(Comments = cycling_lower['Comments'].str.replace('whether', 'weather'))
```

`.contains('target')` finds any entry that contains the target:

```python
cycling_lower['Comments'].str.contains('rain')
cycling_lower[cycling_lower['Comments'].str.contains('rain')]

# Replace all entries mentioning "rain" with just "rained":
cycling_lower.loc[cycling_lower['Comments'].str.contains('rain'), 'Comments'] = 'rained'
```

This last example finds any mention and then replaces the entire string, so "tut tut, it looks like rain" gets replaced with "rained".

### More examples from the lesson
```python
import pandas as pd

lego = pd.read_csv('data/lego-sets.csv')

# Convert the name column in the lego dataset to lowercase and
# overwrite the dataframe by saving it as an object named lego 

lego = lego.assign(name = lego['name'].str.lower())


# Filter the dataset to find all the lego sets that contain "weetabix"  in the name column
# Save this as a object named lego_weetabix

lego_weetabix = lego[lego['name'].str.contains('weetabix')]


# Replace the word "weetabix" in the name column of the lego_wetabix dataframe
# with the string "cereal-brand"
# Save this in an object called lego_cereal

lego_cereal = lego_weetabix.assign(name = lego_weetabix['name'].str.replace('weetabix', 'cereal-brand'))


# If the row contains the word "promotional" in the name column,
# change the entire value to "cereal-brand freebie"

lego_cereal.loc[lego_cereal['name'].str.contains('promotional'), 'name'] = 'cereal-brand freebie'

# Display lego_cereal

lego_cereal
```