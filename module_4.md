# Basic Python

## Learning goals (ToC)
- [Python objects and data types](#python-objects-and-data-types)
- [Convert data types by casting](#convert-data-types-by-casting)
- [Split strings into lists with `.list()`](#split-strings-into-lists-with-split)
- [Understanding lists (arrays by another name)](#lists---arrays-by-another-name)
- [Turn lists into DataFrames](#turn-lists-into-dataframes)
- [Tuples - immutable lists](#tuples---lists-but-immutable)
- [Sets - piles of data without repeats](#sets---unordered-no-duplicates-piles-of-data)
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
