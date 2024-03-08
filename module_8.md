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

- Describe the shape, dimension and size of an array.
- Identify null values in a dataframe and manage them by removing them using `.dropna()` or replacing them using `.fillna()`.
- Manipulate non-standard date/time formats into standard Pandas datetime using `pd.to_datetime()`.
- Find, and replace text from a dataframe using verbs such as `.replace()` and `.contains()`.

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
The `linspace(start, stop, steps)` function produces `steps` number of evenly spaced items between `start` and `stop`. The items default to `float`.

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

