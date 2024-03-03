# Module 7: Importing Files and the Coding Style Guide

## Learning Goals (ToC)
- [Importing Python libraries, functions, and files](#importing-python-libraries)
- [Testing your functons with Pytest](#testing-your-functions-with-pytest)
- [Automatic style formatting](#automatic-style-formatting)
- [Guidelines for writing things](#guidelines-for-writing-things)
- [The Python Debugger](#the-python-debugger)
- Describe what Python libraries are, as well as explain when and why they are useful.
- Identify where code can be improved concerning variable names, magic numbers, comments and whitespace.
- Write code that is human readable and follows the black style guide.
- Import files from other directories.
- Use pytest to check a function's tests.
- When running pytest, explain how pytest finds the associated test functions.
- Explain how the Python debugger can help rectify your code.

## Importing Python Libraries
Libraries are prepared clusters of code you can import and use. They are imported using the `import` keyword and are usually renamed with a shorter name using the `as` keyword like this:

```python
import pandas as pd
```

You can also import specific functions from within a library by calling it directly like this:

```python
from numpy import sqrt
```

__NOTE:__ Doing this, you get only `sqrt` functions, not the whole `numpy` library.

### Working with other files
Once you have a function defined in a `file_name.py` file, you can import it the same way as above:

```python
from file_name.py import some_function
```

## Testing your functions with Pytest
To create a test file,

1. Create a new `test_this_function.py` file.
2. Create a `test_this_function()` function in that file.
3. Put the assert tests or whatever in that function (without a `return` statement).
4. Import the function you want to test into the test file.
5. In terminal, navigated to the relevant folder, run `pytest`.

`pytest` will pick up any file prefixed with `test_` and run the function within. To specify what file to run, invoke it using `pytest test_this_function.py`.

## Automatic style formatting
Code style and formatting enforces a specific code style on code so it's easier to read and work with in teams. In this modern day of development we have tools to do this formatting for us so we don't have to do it ourselves, and we have many code style guides we can choose to follow.

__PEP8__ is a style guide that recommends formatting such as:
- Indenting using 4 spaces
- Having whitespace around operators, e.g. x = 1 not x=1
- Avoiding extra whitespace such as f(1), not f (1)
- Single and double quotes for strings, but only using “””triple double quotes”””, not ‘’’triple single quotes’’’
- Variable and function names using underscores_between_words

We can use the `flake8` library to check if our code adheres to the `PEP8` style guide.

Doing this manually is annoying, so we don't do that. Instead we use automatic formatters. 

The formater called `black` enforces PEP8 with a few subtle differences. As a result, `flake8` will still complain about your code even when you're using `black`. Tough cookies.

To check `.py` files with `flake8`, just call the file in terminal

```pycon
flake8 this_function.py
```

To check Jupyter Notebook `.ipynb` files, do this:

```pycon
flake8-nb example.ipynb
```

You can also run `flake8` in a code cell _within_ a notebook like this:

```pycon
!flake8 exponent_a_list.py
```

We can use the same approach to have `black` clean that mess up for us:

```pycon
!black exponent_a_list.py
```

Do the same from terminal:

```pycon
Black exponent_a_list.py
```

## Guidelines for writing things

### Commenting Guidelines according to PEP8 and python.org:
- Comments should start with a # followed by a single space.
- They should be complete sentences. The first word should be capitalized unless it is an identifier that begins with a lower case letter.
- Comments should be clear and easily understandable to other speakers of the language you are writing in.
- For block comments, each line of a block comment should start with a # followed by a single space and should be indented to the same level as the code it precedes.

### Common variable naming recommendations:
- Variable names should use underscores
- Variable names should be lowercase
- Don't name variables for keywords, ie `list`, `str`, etc. That shit ain't gonna work.
- Give your variables meaningful yet concice names.

## The Python Debugger
Fun historical esoterica: The term "debugging" is credited to computer pioneer Grace Hopper. The anecdote from which the term emerges is that a computer program was malfunctioning and upon inspection it was discovered it was because a moth had gotten stuck in a punch card and was blocking several holes. To fix the problem, they literally de-bugged the punch card. 

Most computer jargon is this silly. Don't look up the origin of the name "JavaScript". You'll wish you didn't.

Anyway...

When debugging Python, you can use the `breakpoint()` function to add a breakpoint where you want the Python debugger to kick in. When you run the code and it hits the breakpoint, the code stops and you can now interact with the memory of the current function in its current state by for example printing out a variable.

To move to the next line in the code, type `n`.

Hit `q` to quit.

Once you're done debugging, remove `breakpoint()` or the debugging will keep bugging you.

This is just the tip of the debugging iceberg. The [documentation for the Python Debugger](https://docs.python.org/3/library/pdb.html) has lots more info.

