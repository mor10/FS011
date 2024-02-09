# Basic Python

## Learning goals (ToC)
- [Python objects and data types](#python-objects-and-data-types)
- [Convert data types by casting](#convert-data-types-by-casting)
- [Split strings into lists with `.list()`](#split-strings-into-lists-with-split)
- [Understanding lists (arrays by another name)](#lists---arrays-by-another-name)
- [Turn lists into DataFrames](#turn-lists-into-dataframes)
- [Tuples - immutable lists](#tuples---lists-but-immutable)
- [Sets - piles of data without repeats](#sets---unordered-no-duplicates-piles-of-data)
- [Dictionaries - collections of key:value pairs](#dictionaries-aka-dict)
- [Series - what DataFrames are made of](#series)
- [Python operators (math and Boolean)](#python-operators)
- [Splitting a column](#splitting-a-column)
- [Use Python to determine the type and structure of an object.]
- [Demonstrate how to create data structures and convert them to another.]
- [Identify which operations can be applied to different data types and columns dtypes.]

## Python objects and data types
In Python, values are stored in objects (analagous to variables in other languages).

- __Everything is an Object__: In Python, everything is an object. This includes not just more complex data structures like lists, dictionaries, and user-defined classes but also numbers, strings, and even functions. Each object in Python comes with attributes and methods, which are contextually related data and functionality.
- __Immutability__: Some Python objects are immutable (e.g., integers, floats, strings, tuples), meaning their state cannot be changed after they are created. If you perform an operation that modifies an immutable object, a new object is created instead.
- __Reference__: When you assign a variable to an object, you're actually assigning a reference to that object, not the object itself. If you assign another variable to the same object, both variables will refer to the same object in memory.

Here are some data types built-in to the Python language:

- `int` - Integers
- `float` - Floating-point numbers (aka decimals)
- `str` - Strings (any string of symbols, numbers, characters. Strings are immutable.)
- `bool` - Booleans (`true`/`false`)
- `list` - Lists (an array; ordered collection of items that can contain elements of different types, including other lists)
- `tuple` - Tuples (immutable and ordered collection of elements; used to store multiple items in a single variable.) 
- `set` - Sets (unordered collection of unique elements)
- `dict` - Dictionaries (mutable, unordered collection of key-value pairs; similar to JS object)
- `NaN` - Empty `float`, literally "Not A Number" 
- `NoneType` - Untyped data. Only takes value of `none`

If creating an empty object:
```python
empty_object = ''
```
the empty object has the type of `string` because of the empty quotes. If no quotes, it would become a `NoneType`.

### String methods
- `.len()` - Returns the length of the string as an `int`
- `.upper()` - Returns the string in uppercase.
- `.lower()` - Returns the string in lowercase.
- `.count(pat)` - Returns the number of instances matching the pattern.

## Convert data types by casting
Transforming an object from one datatype to another is called "casting."

Casting is done using methods with names that match the target data type:

Here's how you can transform data types with `int()`, `float()`, `bool()`, and `str()` in Python:

- `int()` - Converts a value to an integer. This can be used to convert float numbers or strings that represent numbers to integer form, truncating any decimal part in floats.
- `float()` - Converts a value to a floating-point number. This is useful for converting integers or strings to floats.
- `bool()` - Converts a value to a boolean (`True` or `False`). By default, any non-zero number or non-empty container (like a list or string) will convert to `True`, while zero, `None`, and empty containers will convert to `False`.
- `str()` - Converts a value to a string. This can be used to convert numbers, boolean values, and other objects to their string representation.

### Examples

```python
# Converting to integer
print(int(3.14))  # Output: 3
print(int("10"))  # Output: 10

# Converting to float
print(float(5))  # Output: 5.0
print(float("2.65"))  # Output: 2.65

# Converting to boolean
print(bool(1))  # Output: True
print(bool(0))  # Output: False
print(bool(""))  # Output: False
print(bool("Python"))  # Output: True

# Converting to string
print(str(10))  # Output: '10'
print(str(3.14))  # Output: '3.14'
print(str(True))  # Output: 'True'
```

## Split strings into lists with `.split()`
```python
str.split(sep=None, maxsplit=-1)
```

> Split strings around given separator/delimiter.

The [`split()`](https://docs.python.org/3/library/stdtypes.html#str.split) method in Python is a string method used to split a string into a list of substrings based on a specified separator. If no separator is specified, it defaults to splitting on whitespace.

### Parameters:
- `sep`: The delimiter according to which the string is split. If `sep` is not specified or is `None`, any whitespace string is a separator.
- `maxsplit`: Defines the maximum number of splits. The default value `-1` means no limit on the number of splits.

### Example:

```python
text = "Python is great for data science"
# Splitting without specifying a separator, splits at whitespace
words = text.split()
print(words)  # Output: ['Python', 'is', 'great', 'for', 'data', 'science']

# Splitting with a specified separator
data = "apple,banana,cherry"
fruits = data.split(',')
print(fruits)  # Output: ['apple', 'banana', 'cherry']

# Using maxsplit
sentence = "Python is fun; really fun"
parts = sentence.split(';', maxsplit=1)
print(parts)  # Output: ['Python is fun', ' really fun']
```

This method is particularly useful for parsing and processing text data, allowing for easy extraction of individual words, values, or components from a string.

## Lists - arrays by another name
Because reasons, Python uses other names for known things. Case in point: data type `list`, known in other languages like JavaScript as `array`.

Thus lists are item-agnostic, nestable, __mutable__ (meaning you can change any value within a list), and you can do array-things with them (because they are arrays).

A list in Python is a mutable, ordered collection of items that can contain elements of different types, including numbers, strings, other lists, and more. Lists are defined by enclosing elements in square brackets `[]`, and they support a variety of operations, such as adding, removing, or modifying elements. Lists are versatile and widely used in Python for data storage and manipulation due to their flexibility and the extensive set of methods available for working with them.

### Example:

```python
# Creating a list
my_list = [1, "Python", [2, 3], True]

# Getting the length of a list
len(my_list)  # Output: 4 (the nested list counts as one item)

# Adding an element
my_list.append("new item")
print(my_list)  # Output: [1, 'Python', [2, 3], True, 'new item']

# Accessing an element
print(my_list[1])  # Output: Python

# Slicing a list (like with iloc[])
print(my_list[1:4])  # Output: ['Python', [2, 3], True] (index position 1, 2, and 3 (end is exclusive)).

# Access a list within a list
print(my_list[2][1]). # Output: 3

# Modifying an element (mutable)
my_list[1] = "Java"
print(my_list)  # Output: [1, 'Java', [2, 3], True, 'new item']

# Removing an element
my_list.remove("Java")
print(my_list)  # Output: [1, [2, 3], True, 'new item']
```

_Note: Strings can be sliced like lists, but you cannot assign new values within strings._

### Useful list methods

- `.max(list)` - Returns max value
- `.sum(list)` - Returns sum of list values

Turn a `list` into a `set` using the `set()` method:

```python
my_set = set(my_list)
```

## Turn lists into DataFrames
```python
class pandas.DataFrame(data=None, index=None, columns=None, dtype=None, copy=None)
```

> Two-dimensional, size-mutable, potentially heterogeneous tabular data.
>
> Data structure also contains labeled axes (rows and columns). Arithmetic operations align on both row and column labels. Can be thought of as a dict-like container for Series objects. The primary pandas data structure.

Use [`pd.DataFrame()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html) to create a new dataframe from lists or other objects.

### Example
```python
item1 = ['toothpaste', 'London Drugs', 3.99]
item2 = ['apples', 'Produce Store', 4.00]
item3 = ['bread', 'Bakery', 3.50]
column_names = ['item', 'location', 'price']

shopping_items = pd.DataFrame(data=[item1, item2, item3], columns=column_names)
shopping_items
         item       location  price
0  toothpaste   London Drugs   3.99
1      apples  Produce Store   4.00
2       bread         Bakery   3.50
```

## Tuples - lists, but immutable
Tuples are lists, wrapped in parenthesis, and they are _immutable_.

```python
my_tuple = ('I', 'lose', None,  'socks', 'when', 1, 'do', 'laundry.', False)
```

Turn a `tuple` into a `list` using the `.list()` method:

```python
my_list = list(my_tupple)
```

Now you can do list things with the (former) tuple data.

## Sets - unordered no-duplicates piles of data
Sets contain sets of data, wrapped in curly braces. __No repeats in sets__. They don't have indexes like lists or tuples.

```python
my_set = {2, 1.0, 'Buckle my shoe'}
```

You can add items to a set using the `.add(value)` method. When you do, they are placed in the set according to _secret mysterious Python rules_.

## Dictionaries (aka "dict")
A dictionary in Python is a collection of `key`:`value` pairs, similar to an object in JavaScript.

A dictionary in Python is a mutable, unordered collection of key-value pairs, allowing fast data retrieval by key. Each key-value pair in a dictionary is separated by a colon (`:`), keys are unique within a dictionary, and the pairs are enclosed in curly braces `{}`.

__Important:__ Each `key` in a dictionary must be unique. There can be only one! It's the lookup key. A `key` can be a `string`, a `number`, and technically also a `tuple` and a `boolean` though the latter two make little sense as keys.

Values can be any type.

To get the `value` of a dict entry, call the `key`:

```python
retrieved_value = dict[key]
```

To set a new `key`:`value` pair (or to set an existing `key`)"

```python
cake['temp'] = 350
```

To get all the items in a dict, use `.items()`

```python
cake.items()
```

### Example:

```python
# Creating a dictionary
my_dict = {
    "name": "Alice",
    "age": 30,
    "city": "New York"
}

# Accessing a value by key
print(my_dict["name"])  # Output: Alice

# Adding a new key-value pair
my_dict["email"] = "alice@example.com"
print(my_dict)  # Output will include the new email key-value pair

# Updating a value
my_dict["age"] = 31
print(my_dict["age"])  # Output: 31

# Removing a key-value pair
del my_dict["city"]
print(my_dict)  # Output will no longer include the "city" key-value pair

# Using get() to safely access a value
print(my_dict.get("city", "Not found"))  # Output: Not found

# Iterating over keys (directly or with .keys()), values (.values()), or both (.items())
for key, value in my_dict.items():
    print(f"{key}: {value}")
```

Dictionaries are widely used in Python for storing data that can be easily retrieved by a unique key, making them essential for tasks involving data manipulation, storage, and retrieval operations.

### Create a DataFrame from a dict

```python
classmethod DataFrame.from_dict(data, orient='columns', dtype=None, columns=None)
```

> Construct DataFrame from dict of array-like or dicts.
>
> Creates DataFrame object from dictionary by columns or by index allowing dtype specification.

Pandas provides the [`pd.DataFrame.from_dict()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.from_dict.html) method to create a DataFrame from a dictionary. The keys of the dictionary can serve as column names, and the values are used to populate the rows of the DataFrame.

Parameters:
- `data` - The dictionary object.
- `orient` - The orientation of the data. If 'columns', the keys are column names; if 'index', the keys are row indices.
- `dtype` - The data type to force. If None, infer.
- `columns` - The column labels to use if `orient='index'`. In this case, you can use this parameter to ensure the DataFrame's columns are in the desired order.

#### Example: Creating a DataFrame from a Dictionary

```python
import pandas as pd

# Dictionary
data = {
    'A': [1, 2, 3],
    'B': [4, 5, 6],
    'C': [7, 8, 9]
}

# Creating a DataFrame from the dictionary
df = pd.DataFrame.from_dict(data)

print(df)
```

This will create a DataFrame with columns 'A', 'B', and 'C', and the rows populated with the corresponding values from the lists in the dictionary.

If your query was about a different `from_dict` method or a different context, please provide more details for a more accurate explanation.

#### Create DataFrame based on rows

```python
data = { 'name': ['Cherry', 'Oak', 'Willow', 'Fir'], 
         'height': [7, 20, 12, 16], 
         'diameter': [12, 89, 30, 18], 
         'flowering': [True, False, True, False]}

forest = pd.DataFrame.from_dict(data)
```

#### Create DataFrame based on columns

```python
data = {0: ['Cherry', 7, 12, True],
        1: ['Oak', 20, 89, False],
        2: ['Willow', 12, 30, True],
        3: ['Fir', 16, 18, False]}
column_names = ['name', 'height', 'diameter', 'flowering']

forest = pd.DataFrame.from_dict(data, orient='index', columns= column_names)
```

## Series
A DataFrame is a "Two-dimensional tabular data structure with columns and axis labels."

When you index a column from a DataFrame using two brakcets `cereal[['mfr']]` you get a new DataFrame in return.

When you index a column from a DataFrame using single brackets `cereal['mfr']` you get a __Series__.

A _Series_ is a one-dimensional array of values with an axis label, sort of like a list with a name attached to it.

> A DataFrame is really a dict of Series objects.

A Series has the properties `Name`, `Length`, and `dtype`.

Columns in DataFrames have a `dtype`:

Numeric dtypes:
- `float64` - contains `float` type values.
- `int64` - contains `int` type values.

Non-numeric types:
- `obj` - _default_ - contains `str` type values or a mix of other values.
- `bool` - contains Boolean values (`True`/`False`)
- `datetime64` - out of scope
- `timedelta[ns]` - out of scope

Get the `dtype` for every column using `.dtypes` method on the DataFrame:

```python
cereal.dtypes
```

Get the `dtype` of a column using the `.dtypes` method on the column:

```python
cereal['calories'].dtypes
```

### Breakdown

A Series in Pandas is a one-dimensional array-like object that can hold data of any type (integer, string, float, Python objects, etc.), and it is capable of holding both a data sequence and associated labels, known as the index. The basic structure of a Series can be thought of as a cross between a list and a dictionary in Python, where the index serves a role similar to keys in a dictionary, allowing for fast lookups and operations.

#### Key Features of Pandas Series:

- **Homogeneous Data**: Although a Series can hold any data type, each Series object holds data of one type.
- **Size Immutable**: You cannot change the size of a Series once it is created, but its values are mutable.
- **Data Alignment**: An important feature of Series is automatic alignment of data based on the index labels, which is very useful for working with data from different sources.

#### Creating a Series:

You can create a Series from a list, array, or dictionary using the `pd.Series()` constructor.

```python
import pandas as pd

# Creating a Series from a list
s1 = pd.Series([1, 3, 5, 7, 9])

# Creating a Series from a dictionary
s2 = pd.Series({'a': 100, 'b': 200, 'c': 300})

print(s1)
print(s2)
```

#### Accessing Elements:

Elements in a Series can be accessed using index labels if defined, or by using integer location.

```python
# Accessing elements by index
print(s2['a'])  # Output: 100

# Accessing elements by integer location
print(s1[2])  # Output: 5
```

#### Operations

Pandas Series support various operations, including mathematical operations, aggregations, slicing, and more, which can be performed while automatically aligning data based on index labels.

```python
# Mathematical operation
s3 = s1 * 2
print(s3)

# Aggregation
print(s1.mean())
```

Series are foundational to Pandas and data manipulation in Python, often used for representing individual columns in a DataFrame or for performing data analysis tasks directly.

## Python Operators
Operators are used to do math things.

Standard operators apply:
- `+` - Addition
- `-` - Subtraction
- `*` - Multiplication
- `/` - Division
- `**` - Exponent

`bool` values can mix with `int` and `float` because `True` = 1 and `False` = 0.

Mixing strings and numbers with operators creates interesting things:

```python
'The monster under my bed' + ' is named Mike'  # Returns 'The monster under my bed is named Mike'

'The monster under my bed' + 1200  # Returns an error

'The monster under my bed' + str(1200)  # Returns 'The monster under my bed1200'

'The monster under my bed' * 3. # Returns 'The monster under my bedThe monster under my bedThe monster under my bed'
```

You can use `+` to concatenate lists:

```python
list1 = [1, 2.0, 3, 4.5] + ['nine', 'ten', 'eleven', 'twelve']
```

### Boolean operators

```python
# Comparison Operators

x, y = 5, 10

# Checks if the value of x is equal to the value of y.
print(x == y)  # False, because x (5) is not equal to y (10).

# Checks if the value of x is not equal to the value of y.
print(x != y)  # True, because x (5) is indeed not equal to y (10).

# Checks if x is greater than y.
print(x > y)   # False, because x (5) is not greater than y (10).

# Checks if x is greater than or equal to y.
print(x >= y)  # False, because x (5) is neither greater than nor equal to y (10).

# Checks if x is less than y.
print(x < y)   # True, because x (5) is less than y (10).

# Checks if x is less than or equal to y.
print(x <= y)  # True, because x (5) is less than y (10), satisfying the condition.

# Identity Operator

# Checks if x is the same object as y.
print(x is y)  # False, because x and y are not the same object (though this primarily compares identity, not value).

# Logical Operators

a, b = True, False

# Checks if both a and b are True.
print(a and b)  # False, because b is False.

# Checks if at least one of a or b is True.
print(a or b)   # True, because a is True.

# Checks if a is False (not a).
print(not a)    # False, because a is True.
```

## Column operators

If we have a DataFrame where some columns have a `dtype` that is not a number, those columns are excluded when running `DataFrame.describe()`.

If we use `.sum()` on a column with `dtype` `obj` we get a concatenation of each value rather than the actual sum (same as if each value was a string and a `+` operator was added between. Literal sum of strings.)

Solve using `.astype('type')` method to reassign `dtype`:

```python
cereal = cereal.assign(calories=cereal['calories'].astype('int'))
```

`.mean()` also runs into issues when applied to a column with `dtype` `obj`, `bool`, etc.

Use the `axis` argument to do operations on rows:

```python
# Get the sum
cereal.loc[:, 'protein': 'carbo'].sum(axis=1)

# Add the sum as a new column
cereal = cereal.assign(total_pffc=cereal.loc[:, 'protein': 'carbo'].sum(axis=1))
```

## Splitting a column
```python
Series.str.split(pat=None, *, n=-1, expand=False, regex=None)
```

> Split strings around given separator/delimiter.
>
> Splits the string in the Series/Index from the beginning, at the specified delimiter string.

The `.split()` method is used to split a string. The [`.str.split()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.str.split.html) method can be used to split a column.

```python
new = cereal_amended['mfr_type'].str.split('-', expand=True)
```

This produces two new columns named `0` and `1`. Use `.rename()` to help make sense of things:

```python
new = new.rename(columns = {0:'mfr', 1: 'type'})
```

Finally, add the `new` DF onto the old one:

```python
cereal = cereal_amended.assign(mfr=new['mfr'], type=new['type'])
```

If you set the `.str.split()` separator to `False`, it returns a Series data type with a list containing both columns for each row. 