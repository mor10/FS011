# Making Choices and Repeating Iterations

## Learning Goals (ToC)
- [Conditional `if`/`elif`/`else` statements](#conditional-statements)
- [The ternary operator](#ternary-inline-condition)
- [Explain the DRY principle and how it can be useful.](#dry-principle)
- [Write `for` loops to repeatedly run code.](#for-loops)
- [Looping with `range()`](#looping-with-range)
- [Nested loops](#nested-loops)
- [Stop an iteration](#stop-an-iteration)
- [How to create functions](#functions)

## Conditional Statements
Conditional statements test a condition (if something is true or false) and act accordingly. This is self-explanatory:

`if` condition `a` is met :
    do this
`elif` (else-if) condition `b` is met :
    do that
`else` : 
    do the other thing

The condition can be anything that returns a boolean value.

### Example
```python
my_name = 'Hayley' 

if my_name.lower() == 'hayley':
    print('My name is Hayley too!')
elif my_name.lower() == 'totoro':
    print('Interesting, I loved that movie!')
else:
    print("That's a great name.")

print('Nice to meet you!')
```

Python conditional statements contains 2 important things:

- A strict structure.
- The keyword `if` and optional keywords `else` and `elif`.

### Python has Opinions!
Python handles nesting of operations through indentation (!??!?), so in the case of an `if` statement that ends with a `:`, the nested action below is indented by 4 spaces. If it ain't indented it ain't happenin'! Put away your curly braces. No minificaiton here!

The `elif` statement must follow an initial `if` (obvs: you can't say "else-if" without saying "if" first) and can be repeated multiple times to create a decision tree of sorts.

### Conditional statements are basic logic
Structure your conditional statements logically or they won't work. 

Trivial example:

```python
item = 25 

if item > 10:
    magnitude = 'Between 10 and 20'
elif item > 20:
    magnitude = 'Greater than 20'
else:
    magnitude = '10 or less'

magnitude
```

### Ternary (inline) condition
For short if/else statements you can do a ternary condition. The structure for ternary conditions in Python is:

```python
variable = value_if_true if condition else value_if_false
```

More detailed example:

```python
# Long version:
item = 13
if  item > 10:
    magnitude = 'Greater than 10'
else:
    magnitude = '10 or less'

magnitude

# Short version:
magnitude = "Greater than 10" if item > 10 else "10 or less"

```

### Boolean Conditions
Boolean conditions in Python are expressions that evaluate to either `True` or `False`. These conditions are fundamental to controlling the flow of a program, especially when used with conditional statements like `if`, `elif`, and `else`. Boolean conditions can involve comparisons, logical operations, and even membership tests. Here's a breakdown of how these conditions work:

### Comparisons
Comparison operators are used to compare two values. Here are the basic comparison operators in Python:

- `==` (equal to)
- `!=` (not equal to)
- `>` (greater than)
- `<` (less than)
- `>=` (greater than or equal to)
- `<=` (less than or equal to)

#### Logical Operators

Logical operators are used to combine conditional statements:

- `and` returns True if both conditions are true.
- `or` returns True if at least one of the conditions is true.
- `not` reverses the result of the condition (i.e., True becomes False and vice versa).

#### Membership Operators

Membership operators are used to test if a sequence is presented in an object:

- `in` returns True if a sequence with the specified value is present in the object.
- `not in` returns True if a sequence with the specified value is not present in the object.

#### Examxle

```python
x = 5
y = 10

# Comparison
print(x > 3)  # True
print(y == 10)  # True

# Logical
print(x < 3 and y == 10)  # False
print(x < 3 or y == 10)  # True

# Not
print(not x > 3)  # False

# Membership
my_list = [1, 2, 3, 4, 5]
print(x in my_list)  # True
print(6 not in my_list)  # True
```

#### Contextual Example

Imagine you are working on a data analysis project and need to filter a dataset based on multiple conditions. Here's how you might use boolean conditions to accomplish this task:

```python
# Sample dataset: List of dictionaries representing people's information
people = [
    {"name": "Alice", "age": 30, "city": "New York"},
    {"name": "Bob", "age": 25, "city": "Paris"},
    {"name": "Charlie", "age": 35, "city": "London"},
    {"name": "Diana", "age": 32, "city": "New York"}
]

# Goal: Find people who are older than 30 and live in New York
filtered_people = [person for person in people if person["age"] > 30 and person["city"] == "New York"]

print(filtered_people)
```

This example demonstrates how to use boolean conditions to filter a list of dictionaries based on specific criteria (in this case, age and city). The use of `and` in the conditional expression allows for checking multiple conditions simultaneously, illustrating the power of boolean conditions in data filtering and manipulation tasks.

## DRY Principle
DRY = "Don't Repeat Yourself" as opposed to WET "Write Every Time".

DRY is a core design principle for coding. Any time you need to repeat an action, create a function for it so you only have to write it once and then pass the data to it rather than repeat the same function over and over.

## `for` Loops
Loops allow you to repeat actions a set number of times, iterate through things like lists, etc. 

A `for` loop in Python is used to iterate over a sequence (such as a list, tuple, dictionary, set, or string) or other iterable objects. Iterating over a sequence is called traversal. `for` loops are often used to execute a block of code for each item in a sequence.

Here’s the basic syntax of a `for` loop:

```python
for item in sequence:
    # Block of code to execute for each item
```

### Example

Let's say we have a list of fruits, and we want to print each fruit in the list:

```python
fruits = ['apple', 'banana', 'cherry']

for fruit in fruits:
    print(fruit)
```

This loop will iterate over the list `fruits`, assigning each element of the list to the variable `fruit` in turn, and then print each one.

### Contextual Example

Now, let's look at a more practical example where `for` loops can be really useful in data analysis:

```python
# List of numbers
numbers = [1, 2, 3, 4, 5]

# Calculate the square of each number
squares = []

for number in numbers:
    squares.append(number ** 2)

print(squares)
```

In this example, we iterate over a list of numbers, calculate the square of each number using the expression `number ** 2`, and then append the result to the list `squares`. This demonstrates how `for` loops can be used to perform operations on the elements of a sequence and collect the results.

### Using `range()` in `for` Loops

The `range()` function generates a sequence of numbers, which can be used to iterate over a block of code a specific number of times. Here’s how you can use `range()` in a `for` loop:

```python
# Print numbers from 0 to 4
for i in range(5):
    print(i)
```

`range(5)` generates numbers from 0 to 4, and the `for` loop iterates over each number, printing it.

`for` loops are a powerful tool in Python, enabling efficient iteration and manipulation of data structures, which is a cornerstone of data analysis and manipulation tasks.

## Looping with `range()`
The `range()` function is used to generate output over ranges:

```python
range(start, end, skip)```

The `range()` function in Python returns a sequence of numbers. It is often used for looping a specific number of times in `for` loops. `range()` can take one, two, or three arguments, making it versatile for various use cases.

### Syntax

- `range(stop)` - Generates a sequence of numbers from 0 to `stop-1`.
- `range(start, stop)` - Generates a sequence of numbers from `start` to `stop-1`.
- `range(start, stop, step)` - Generates a sequence of numbers from `start` to `stop-1`, incrementing by `step`.

### Examples

1. **Using `range()` with one argument:**

```python
for i in range(5):
    print(i)
```

This will print numbers from 0 to 4.

2. **Using `range()` with two arguments:**

```python
for i in range(2, 6):
    print(i)
```

This will print numbers from 2 to 5.

3. **Using `range()` with three arguments:**

```python
for i in range(1, 10, 2):
    print(i)
```

This will print odd numbers from 1 to 9.

### Contextual Example

Imagine you're working with Python to analyze data and need to iterate over a sequence of dates, perform operations based on indices, or simulate data. Here's how you might use `range()` in a practical scenario:

```python
# Simulating temperature measurements over a 10-day period
temperatures = [22, 24, 19, 23, 25, 21, 20, 22, 18, 24]

# Calculate the average temperature
total_temp = sum(temperatures)
num_days = len(temperatures)
average_temp = total_temp / num_days

print(f"Average temperature over 10 days is: {average_temp}°C")

# Identifying days with temperatures above average
print("Days with above average temperatures:")
for i in range(num_days):
    if temperatures[i] > average_temp:
        print(f"Day {i+1} with {temperatures[i]}°C")
```

In this example, `range()` is used to iterate over the indices of the `temperatures` list. This allows us to access each temperature measurement, compare it against the average, and print the days where the temperature is above average. This demonstrates the utility of `range()` in looping through indices in data analysis tasks.

### Full-fledged example:
Load in 4 dataframes using a `for` loop and `range()`, then concatenate them:

```python
# Starting with an empty dataframe

full_dataframe = None

# This code creates loop that reads in each dataframe and concatenates them together

for number in range(1,5):
    string = 'data/pkm' + str(number) + '.csv'
    data = pd.read_csv(string)
    full_dataframe = pd.concat([full_dataframe,data])

# Display the final dataframe

full_dataframe
```
## Looping in a dictionary
The `for` loop can be used to loop through items in a dictionary. This is done using the `items()` method. Here we have to identify both the dictionary `key` and `value`:

```python
cereals = {'Special K': 4, 'Lucky Charms': 7, 'Cheerios': 2, 'Wheaties': 3}

for cereal, stock in cereals.items():
    print( cereal  + " has " + str(stock) + " available")
```

## Comprehensions: build lists/sets/dictionaries with `for` loops
In the example below the dictionary key is labled `number`:

```python
numbers = [2, 3, 5]

word_length = {number : number ** 2 for number in numbers}
word_length # returns: {2: 4, 3: 9, 5: 25}
```

Comprehensions in Python provide a concise way to create lists, dictionaries, sets, and even generators by iterating over sequences and applying an expression to each element in the sequence. They are a powerful feature that enables clean, efficient, and readable code for creating new data structures.

### Types of Comprehensions

1. **List Comprehensions**: Create a new list by applying an expression to each item in a sequence.
2. **Dictionary Comprehensions**: Create a new dictionary by generating key-value pairs.
3. **Set Comprehensions**: Similar to list comprehensions but for sets, removing duplicate values.
4. **Generator Expressions**: Similar to list comprehensions but instead of returning a list, they return a generator object which can be iterated over.

### List Comprehensions

**Syntax**:
```python
[expression for item in iterable if condition]
```

**Example**:
```python
# Squaring numbers in a list
numbers = [1, 2, 3, 4, 5]
squared = [number ** 2 for number in numbers]
print(squared)  # Output: [1, 4, 9, 16, 25]
```

### Dictionary Comprehensions

**Syntax**:
```python
{key_expression: value_expression for item in iterable if condition}
```

**Example**:
```python
# Create a dictionary with number-square pairs
squares = {number: number ** 2 for number in range(1, 6)}
print(squares)  # Output: {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
```

### Set Comprehensions

**Syntax**:
```python
{expression for item in iterable if condition}
```

**Example**:
```python
# Create a set of even numbers
evens = {number for number in range(10) if number % 2 == 0}
print(evens)  # Output: {0, 2, 4, 6, 8}
```

### Generator Expressions

**Syntax**:
```python
(expression for item in iterable if condition)
```

**Example**:
```python
# Create a generator for even numbers
even_gen = (number for number in range(10) if number % 2 == 0)
print(list(even_gen))  # Convert to list to print all at once
```

Comprehensions are not only syntactically concise but also tend to be faster than using equivalent `for` loops, as they are optimized for their respective operations. They are widely used in Python for data manipulation, filtering, and transformation tasks, making code more expressive and readable.

## Nested loops
Nested loops are just loops inside other loops.

```python
suits = ["❤︎","♦︎"]
faces = ['Jack', 'Queen', 'King']

cards = list()
for suit in suits:
    for face in faces: 
        cards.append(face + ' of ' + suit)
cards
```

## Stop an iteration
Use the `break` keyword. It stops the iteration. Nuff said.

## Iterating on a number
Instead of

```python
some_number = some_number + 1
```

we can write

```python
some_number += 1
```

Same with negative.

## Functions
`functions` are `methods` by another name, essentially.

The syntax for defining a function in Python involves the `def` keyword, followed by the function name with parentheses, and a colon. Inside the parentheses, you can optionally include parameters (also known as arguments) that the function can accept to process its task. After the colon, the indented block of code beneath the function definition is executed each time the function is called. Here's a breakdown of the syntax:

```python
def function_name(parameters):
    """Docstring to explain the function's purpose."""
    # Function body
    return value
```

- **`def`**: This keyword starts the function definition.
- **`function_name`**: This is the identifier you're assigning to the function. It should be descriptive and follow standard Python naming conventions.
- **`parameters`**: These are variables passed into the function. They are optional; a function might not need any parameters.
- **`"""Docstring"""`**: This is an optional documentation string that describes what the function does. It's not executed but is highly recommended for better code readability and maintainability.
- **Function body**: This is the block of code that performs the function's main task. It's indented under the function definition.
- **`return`**: This keyword is used to exit a function and optionally pass back an expression to the caller. The `return` statement is optional; if omitted, the function will return `None` by default.

### Example: A Simple Function

Here's a simple function that takes two parameters, adds them together, and returns the result:

```python
def add_numbers(a, b):
    """Add two numbers and return the sum."""
    return a + b

# Calling the function
result = add_numbers(5, 3)
print(result)  # Output: 8
```

In this example, `add_numbers` is the function name, `a` and `b` are parameters, and the function body consists of a single line that returns the sum of `a` and `b`. The `print(result)` line demonstrates how to call the function with arguments `5` and `3`, and then print the result.

### Parameters and Arguments
For clarity (because this is confusing even in the module docs):

- __Parameter__ - A variable listed inside the parentheses in the function definition. It represents the data that the function expects to receive when it is called.
- __Argument__ - The actual value or data passed to the function when it is called.