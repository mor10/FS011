# Module 6: Functions Fundamentals and Best Practices

## Learning Goals (ToC)
- [Global vs local scope](#scope-global-and-local)
- [Side effects](#side-effects)
- [Function arguments and parameters](#function-arguments-and-parameters)
- [Docstrings](#docstrings)
- [Defensive programming and exceptions](#defensive-programming-and-exceptions)
- [Test-driven development](#test-driven-development)
- [Good function design](#good-function-design)

 
## Scope: global and local
- __Global__ scope: a variable or other entity that is available to the entire application - typically at the root.
_ __Local__ scope - a variable or other entity that is available only within the function or other entity it is defined.

If you try calling a local variable outside its scope (eg outside the function it was defined in) you get an error.

__NOTE:__ If you create a globally scoped variable, and then change its value inside a local scope, that change happens to the global variable. _Don't change global variables inside functions._ 

In programming, particularly in Python, the concepts of global and local scope are crucial for understanding how variables are recognized and used within your code. These scopes dictate where a variable can be accessed and modified in your program.

### Global Scope
- **Definition**: A variable created in the main body of a Python script is in the global scope, meaning it can be accessed from anywhere in the script. However, modifying global variables inside a function requires using the `global` keyword, otherwise Python will treat it as a new local variable in the function's scope.
- **Purpose**: Global variables are useful for storing constants or data that needs to be accessed by multiple functions throughout a script.

### Local Scope
- **Definition**: A variable created inside a function belongs to the local scope of that function, and can only be used inside that function. The local scope is created at function call and destroyed when the function returns.
- **Purpose**: Local variables are used for temporary storage that is only relevant within the context of that function, keeping the global namespace clean and reducing the risk of unintended modifications to data.

### Prototype Example

```python
x = "global"  # Global scope

def my_func():
    y = "local"  # Local scope
    print(x)  # Prints global variable x
    print(y)  # Prints local variable y

my_func()
print(x)  # Works fine, prints global x
# print(y)  # This would raise an error, y is not accessible here
```

### Contextual Example

Let's illustrate the difference with a more detailed example, including modifying global variables within a function:

```python
counter = 0  # A global variable

def increment_counter():
    global counter  # Tell Python we want to use the global variable
    counter += 1  # Modify the global variable
    print(f"Counter inside function: {counter}")

increment_counter()  # Increment counter and print it
print(f"Counter outside function: {counter}")  # Access the modified global variable

def local_example():
    local_counter = 10  # A local variable
    local_counter += 1  # Modify the local variable
    print(f"Local counter inside function: {local_counter}")

local_example()
# print(local_counter)  # This would raise an error as local_counter is not accessible here
```

In this contextual example, we see how a global variable (`counter`) is accessed and modified across the global and local scopes, using the `global` keyword to modify it within a function. Conversely, `local_counter` demonstrates a variable that is confined to the local scope of its function, showcasing how local variables are not accessible outside their defining scope. This example highlights the importance of scope management for variable access and modification in Python programming.

## Side effects
In programming, a "side effect" refers to any change that a function or expression causes in the state of the program or system, outside of its own scope or return value. This can include modifying a global variable, changing the value of an input argument, writing data to a file, or altering the state of an external system. Side effects are contrasted with a function's "return value," which is the main effect or output the function is designed to produce.

Side effects are not inherently bad and are often necessary for a program to perform useful work. For example, a function that writes logs to a file or updates a database is relying on side effects. However, excessive or unexpected side effects can make a program difficult to understand, test, and debug, as the behavior of the program depends not just on its input values but also on the state of the system.

Here's a simple example to illustrate the concept of a side effect:

```python
counter = 0  # This is a global variable

def increment():
    global counter
    counter += 1  # This modifies the global variable, which is a side effect

increment()
print(counter)  # Prints: 1
```

In this example, the `increment` function modifies the global variable `counter`, which is a side effect. The primary purpose of the function might seem to increment the value of `counter`, but because it alters the state outside its own local scope, it's considered a side effect.

Understanding and managing side effects is crucial for writing clean, maintainable, and predictable code, especially in functional programming paradigms where functions are encouraged to be pure (having no side effects).

# Function Arguments and Parameters
_Arguments_ allow you to pass information to a function's _parameters_:

```python
def do_some_math(a,b,c): # a,b,c are the function parameters
    result = a + b + c
    return result

print(do_some_math(3,4,5)) # 3,4,5 are the function call arguments
```
The order of your arguments must match the order of the parameters.

You can set default values for any of the parameters:

```python
def tip_calculator(charge, tip=20):
    tip = (charge * tip) / 100
    total = charge + tip
    return [tip,total]

print(tip_calculator(50))
```
__NOTE:__ Any _optional arguments_ with default values must be placed _after_ the required arguments, otherwise things go sideways.

Also, a Python ideosyncracy: You can declare the parameter names in the arguments of your function call and scramble their order. This looks like a crime, but is somehow OK.

```python
def tip_calculator(charge, tip=20):
    tip = (charge * tip) / 100
    total = charge + tip
    return [tip,total]

print(tip_calculator(tip = 15, charge=20))
```

## Docstrings
Docstrings are comments containing documentation about what a function does. 

NumPy style docstrings are marked up starting and ending with three double quotes: `""" stuff and things """`.

The NumPy format includes 4 main sections:
- A brief description of the function
- Explaining the input Parameters
- What the function Returns
- Examples

### Prototype
```python
def function_name(param1, param2):
    """The first line is a short description of the function.

    A paragraph describing in a bit more detail what the function
    does and what algorithms it uses and common use cases.

    Parameters
    ----------
    param1 : datatype
        A description of param1.
    param2 : datatype
        A longer description because maybe this requires
        more explanation, and we can use several lines.

    Returns
    -------
    datatype
        A description of the output, data types, and behaviors.
        Describe special cases and anything the user needs to know 
        to use the function.

    Examples
    --------
    >>> function_name(3, 8, -5)
    2.0
    """
```

### An example from the slides:
```python
def squares_a_list(numerical_list):
    """
    Squared every element in a list.

    Parameters
    ----------
    numerical_list : list
        The list from which to calculate squared values 

    Returns
    -------
    list
        A new list containing the squared value of each of the elements from the input list 

    Examples
    --------
    >>> squares_a_list([1, 2, 3, 4])
    [1, 4, 9, 16]
    """
    new_squared_list = list()
    for number in numerical_list:
        new_squared_list.append(number ** 2)
    return new_squared_list
```

To read a docstring outside the code (eg from a Jupyter Notebook ), use the syntax `?function_name`.

## Defensive programming and exceptions
__Defensive programming__: Code written in such a way that if errors do occur, they are handled in a graceful, fast and informative manner.

__Exceptions__: Used in Defensive programming to disrupts the normal flow of instructions. When Python encounters code that it cannot execute, it will throw an exception.

### Basic example
From the slides:

```python
def exponent_a_list(numerical_list, exponent=2):

    if type(numerical_list) is not list:
        raise Exception("You are not using a list for the numerical_list input.")

    new_exponent_list = list()
    for number in numerical_list:
        new_exponent_list.append(number ** exponent)
    return new_exponent_list
```

Raising an exception in Python is a way to signal an error or abnormal condition in your program. Exceptions are Python's way of interrupting the normal flow of a program when an error occurs. When you raise an exception, you can specify an error message or a custom exception type, allowing your program to handle expected and unexpected issues gracefully.

[Here's a full list of built-in exceptions.](https://docs.python.org/3/library/exceptions.html) They come in many types lik `TypeError`, `IndexError`, `ArithmeticError`, `KeyError`, etc.

Exceptions are documented in the docstring like this:

```python
def exponent_a_list(numerical_list, exponent=2):
    """
    Creates a new list containing specified exponential values of the input list. 

    Parameters
    ----------
    numerical_list : list
        The list from which to calculate exponential values from
    exponent : int or float, optional
        The exponent value (the default is 2, which implies the square).

    Returns
    -------
    new_exponent_list : list
        A new list containing the exponential value specified of each 
        of the elements from the input list 

    Raises
    ------
    TypeError
        If the input argument numerical_list is not of type list

    Examples
    --------
    >>> exponent_a_list([1, 2, 3, 4])
    [1, 4, 9, 16]
```

## Unit testing
Unit testing means throwing a complete sample of all possible input types (units) at the code to see if it is able to correctly process them or raise the correct exceptions.

One way of doing unit testing in Python is using the `assert` statement.

Works like this:
1. `assert` a statement.
2. If the statement is true, continue.
3. If the statement is false, display the associated error message and stop.

```python
assert 1 == 2 , "1 is not equal to 2."
```

### Breakdown
The `assert` statement in Python is a debugging aid that tests a condition. If the condition is `True`, nothing happens, and your program continues to execute as normal. However, if the condition is `False`, an `AssertionError` exception is raised, interrupting the program flow. This makes `assert` a critical tool for developers to quickly check for conditions that they expect to be true at a certain point in the program. It is primarily used during development to catch bugs early; however, it's not recommended to rely on `assert` statements for runtime error handling in a production environment.

Here's a basic example of using `assert`:

```python
def divide_numbers(numerator, denominator):
    assert denominator != 0, "Denominator cannot be zero."
    return numerator / denominator

# This will pass without any issues.
result = divide_numbers(10, 2)
print(result)

# This will raise an AssertionError.
result = divide_numbers(10, 0)
```

In this example, the `assert` statement checks to ensure that the `denominator` is not zero before attempting to divide two numbers. If the `denominator` is zero, it raises an `AssertionError` with the message "Denominator cannot be zero." This prevents the division-by-zero error, which would otherwise occur, by catching the issue early.

Remember, `assert` statements can be globally disabled with the `-O` (optimize) switch when running Python, so they are not suitable for handling conditions that might change during execution or for use in production code where input validation is critical.


## Test-driven development
Tl;Dr: Write the tests first, then write a program that passes the tests one after the other.

Example from the slides:

```python
def exponent_a_list(numerical_list, exponent=2):
    new_exponent_list = list()

    for number in numerical_list:
        new_exponent_list.append(number ** exponent)

    return new_exponent_list

# Tests
assert exponent_a_list([1, 2, 4, 7], 2) == [1, 4, 16, 49], "incorrect output for exponent = 2"
assert exponent_a_list([1, 2, 3], 3) == [1, 8, 27], "incorrect output for exponent = 3"
assert type(exponent_a_list([1,2,4], 2)) == list, "output type not a list"
```

Remember to check for __corner cases__ - weird scenarios like passing an empty list to a function that processes a list. 

When we work with larger datasets, test-driven development means building out a small test set to run the actual tests on.

Example from the slides:

```python
def column_stats(df, column):
   stats_dict = {'max': df[column].max(),
                 'min': df[column].min(),
                 'mean': round(df[column].mean()),
                 'range': df[column].max() - df[column].min()}
   return stats_dict

data = {'name': ['Cherry', 'Oak', 'Willow', 'Fir', 'Oak'], 
        'height': [15, 20, 10, 5, 10], 
        'diameter': [2, 5, 3, 10, 5], 
        'age': [0, 0, 0, 0, 0], 
        'flowering': [True, False, True, False, False]}

forest = pd.DataFrame.from_dict(data)
forest

assert column_stats(forest, 'height') == {'max': 20, 'min': 5, 'mean': 12.0, 'range': 15}
assert column_stats(forest, 'diameter') == {'max': 10, 'min': 2, 'mean': 5.0, 'range': 8}
assert column_stats(forest, 'age') == {'max': 0, 'min': 0, 'mean': 0, 'range': 0}
```

### Systemic approach
1. Write the function stub: a function that does nothing but accepts all input parameters and returns the correct datatype.
2. Write tests to satisfy the design specifications.
3. Outline the program with pseudo-code (comments).
4. Write code and test frequently.
5. Write documentation.

## Good function design

1. Avoid hard-coding values
    - Pass values as arguments instead
2. Less is more
    - Don't make ginormous functions - split them into discrete tasks instead
3. Return a single object
    - You _can_ technically return multiple things, but it's bad practice in most cases (but not all). Context and purpose matter here.
4. Keep global variables in their global environment.