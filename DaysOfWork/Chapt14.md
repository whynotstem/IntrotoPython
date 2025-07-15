## Functions and modules

Before explaining the content of this lesson, let us first study a math problem. Please tell me how many sets of positive integer solutions the following equation has.

$$
x_{1} + x_{2} + x_{3} + x_{4} = 8
$$

You may have figured out that this question is actually equivalent to how many ways there are to divide 8 apples into four groups with at least one apple in each group, or how many ways there are to divide the apples into four groups by putting three partitions between the 7 gaps separating the 8 apples. So the answer is $\small{C_{7}^{3} = 35}$ , where $\small{C_{7}^{3}}$ represents the number of combinations of 7 to 3, and its calculation formula is as follows.

$$
C_m^n = \frac {m!} {n!(mn)!}
$$

Based on what we have learned before, we can use a loop to perform cumulative multiplication to calculate $\small{m!}$, $\small{n!}$, and $\small{(mn)!}$, and then use division to get the number of combinations $\small{C_{m}^{n}}$. The code is shown below.

```python
"""
Input m and n and calculate the value of the combination number C(m,n)

Version: 1.0
Author: Luo Hao
"""

m = int(input('m = '))
n = int(input('n = '))
# Calculate the factorial of m
fm = 1
for num in range(1, m + 1):
    fm *= number
# Calculate the factorial of n
fn = 1
for num in range(1, n + 1):
    fn *= number
# Calculate the factorial of mn
fk = 1
for num in range(1, m - n + 1):
    fk *= num
# Calculate the value of C(M,N)
print(fm // fn // fk)
```

enter:

```
m = 7
n = 3
```

Output:

```
35
```

I wonder if you have noticed that we have performed factorial operations three times in the above code. Although the values of $\small{m}$, $\small{n}$, and $\small{m - n}$ are different, there is no substantial difference between the three pieces of code, and they are repeated codes. Martin Fowler, a world-class programming master, once said: "**Code has many bad smells, and repetition is the worst!**". To write high-quality code, we must first solve the problem of repeated code. For the above code, we can encapsulate the function of factorial calculation into a code block called "function". Wherever we need to calculate the factorial, we only need to "call the function" to reuse the factorial calculation function.

### Define functions

Mathematical functions usually take the form of $\small{y = f(x)}$ or $\small{z = g(x, y)}$. In $\small{y = f(x)}$, $\small{f}$ is the name of the function, $\small{x}$ is the independent variable of the function, and $\small{y}$ is the dependent variable of the function; while in $\small{z = g(x, y)}$, $\small{g}$ is the name of the function, $\small{x}$ and $\small{y}$ are the independent variables of the function, and $\small{z}$ is the dependent variable of the function. Functions in Python are consistent with this structure. Each function has its own name, independent variables, and dependent variables. We usually call the independent variables of Python functions the parameters of the function, and the dependent variables are called the return values of the function.

In Python, you can use the `def` keyword to define functions. Like variables, each function should also have a beautiful name, and the naming rules are the same as the naming rules of variables (please think about the naming rules of variables we talked about before). The parameters of the function can be set in the parentheses after the function name, which is what we just called the independent variables of the function. After the function is executed, we will use the `return` keyword to return the execution result of the function, which is what we just called the dependent variable of the function. If there is no `return` statement in the function, the function will return `None`, which represents a null value. In addition, a function can also have no independent variables (parameters), but the parentheses after the function name are required. What a function has to do (the code to be executed) is placed after the function definition line by indenting the code, similar to the code blocks of the previous branch and loop structures, as shown in the figure below.

<img src="res/day14/function_definition.png" style="zoom:45%;">

Next, we put the factorial operation in the previous code into a function and refactor the code in this way. **Refactoring is to adjust the structure of the code without affecting the code execution result**. The refactored code is shown below.

```python
"""
Input m and n and calculate the value of the combination number C(m,n)

Version: 1.1
Author: Luo Hao
"""


# Define the factorial function using the keyword def
# The independent variable (parameter) num is a non-negative integer
# The dependent variable (return value) is the factorial of num
def do(num):
    result = 1
    for n in range(2, num + 1):
        result *= n
    return result


m = int(input('m = '))
n = int(input('n = '))
# When calculating factorial, you don't need to write repeated code but call the function directly
# The syntax for calling a function is to follow the function name with parentheses and pass in the parameters
print(do(m) // do(n) // do(m - n))
```

You can feel whether the above code is simpler and more elegant than the previous version. More importantly, the factorial function `fac` we defined can also be reused in other codes that need factorial. Therefore, **using functions can help us encapsulate relatively independent and reusable codes**. When we need these codes, instead of rewriting the repeated codes, **we can reuse the existing codes by calling functions**. In fact, in the `math` module of the Python standard library, there is already a function called `factorial` that implements the function of factorial. We can directly import the `math` module with `import math`, and then use `math.factorial` to call the factorial function; we can also use `from math import factorial` to directly import the `factorial` function to use it. The code is shown below.

```python
"""
Input m and n and calculate the value of the combination number C(m,n)

Version: 1.2
Author: Luo Hao
"""
from math import factorial

m = int(input('m = '))
n = int(input('n = '))
print(factorial(m) // factorial(n) // factorial(m - n))
```

In the future, the functions we use will either be custom functions or functions provided by the Python standard library or third-party libraries. If there are already available functions, there is no need to define them ourselves. "**Reinventing the wheel**" is a very bad thing. For the above code, if you think the name `factorial` is too long and not very convenient to write code, we can also use the `as` keyword to give it an alias when importing the function. When calling a function, we can use the function's alias instead of its previous name. The code is as follows.

```python
"""
Input m and n and calculate the value of the combination number C(m,n)

Version: 1.3
Author: Luo Hao
"""
from math import factorial as f

m = int(input('m = '))
n = int(input('n = '))
print(f(m) // f(n) // f(m - n))
```

### Function parameters

#### Positional and keyword arguments

Let's write another function to determine whether a triangle can be formed based on the lengths of the three given sides. If a triangle can be formed, it returns `True`, otherwise it returns `False`. The code is shown below.

```python
def make_judgement(a, b, c):
    """Judge whether the lengths of the three sides can form a triangle"""
    return a + b > c and b + c > a and a + c > b
```

The `make_judgement` function above has three parameters. These parameters are called positional parameters. When calling a function, they are usually passed in from left to right, and the number of parameters passed in must be the same as the number of parameters when defining the function, as shown below.

```python
print(make_judgement(1, 2, 3))  # False
print(make_judgement(4, 5, 6))  # True
```

If you do not want to give the values of the three parameters `a`, `b`, and `c` in order from left to right, you can also use keyword parameters to pass parameters to the function in the form of "parameter name = parameter value", as shown below.

```python
print(make_judgement(b=2, c=3, a=1))  # False
print(make_judgement(c=6, b=4, a=5))  # True
```

When defining a function, we can use `/` to set **positional-only arguments** in the parameter list, and `*` to set **named keyword parameters**. The so-called mandatory positional parameters are parameters that can only receive parameter values according to the parameter position when calling a function; while named keyword parameters can only pass and receive parameters in the "parameter name = parameter value" way. You can see the following example.

```python
# /The preceding parameter is a mandatory positional parameter
def make_judgement(a, b, c, /):
    """Judge whether the lengths of the three sides can form a triangle"""
    return a + b > c and b + c > a and a + c > b


# The following code will generate a TypeError error, and the error message prompts "Mandatory positional parameters are not allowed to give parameter names"
# TypeError: make_judgement() got some positional-only arguments passed as keyword arguments
# print(make_judgement(b=2, c=3, a=1))
```

> **Note**: Mandatory positional parameters are a new feature introduced in Python 3.8. Please pay attention when using an earlier version of the Python interpreter.

```python
# * The parameters following are named keyword parameters
def make_judgement(*, a, b, c):
    """Judge whether the lengths of the three sides can form a triangle"""
    return a + b > c and b + c > a and a + c > b


# The following code will generate a TypeError error, and the error message prompts "The function has no positional parameters but 3 positional parameters are given"
# TypeError: make_judgement() takes 0 positional arguments but 3 were given
# print(make_judgement(1, 2, 3))
```

#### Default values for parameters

Python allows function parameters to have default values. We can encapsulate the function of getting points by rolling the dice in the previously mentioned example "CRAPS Gambling Game" (Lesson 07: Application of Branch and Loop Structures) into a function. The code is shown below.

```python
from random import randrange


# Define the function of shaking the dice
# The function's independent variable (parameter) n represents the number of dice, the default value is 2
# The dependent variable (return value) of the function represents the number of points obtained by shaking n dice
def roll_dice(n=2):
    total = 0
    for _ in range(n):
        total += randrange(1, 7)
    return total


# If no parameter is specified, n uses the default value 2, which means rolling two dice
print(roll_dice())
# Pass parameter 3, and variable n is assigned a value of 3, indicating that three dice are shaken to get points
print(roll_dice(3))
```

Let's look at a simpler example.

```python
def add(a=0, b=0, c=0):
    """Sum of three numbers"""
    return a + b + c


# Call the add function without passing in parameters, so a, b, and c all use the default value 0
print(add())         # 0
# Call the add function and pass in a parameter, which is assigned to variable a. Variables b and c use the default value 0.
print(add(1))        # 1
# Call the add function, pass in two parameters, assign them to variables a and b respectively, and use the default value 0 for variable c
print(add(1, 2))     # 3
# Call the add function, pass in three parameters, and assign them to variables a, b, and c respectively
print(add(1, 2, 3))  # 6
```

It should be noted that **parameters with default values must be placed after parameters without default values**, otherwise a `SyntaxError` error will be generated. The error message is: `non-default argument follows default argument`, which means "parameters without default values are placed after parameters with default values" in Chinese.

#### Variable parameters

In Python, you can use the asterisk expression syntax to allow functions to support variable parameters. The so-called variable parameters mean that when calling a function, you can pass `0` or any number of parameters to the function. In the future, when we develop commercial projects in a team-based manner, it is very likely that we will design functions for others to use, but sometimes we don’t know how many parameters the caller of the function will pass to the function, and at this time, variable parameters can come in handy.

The following code demonstrates how to use variable positional parameters to implement the `add` function that sums any number of numbers. The parameters passed in when calling the function are saved in a tuple. By traversing the tuple, you can get the parameters passed in to the function.

```python
# Use the asterisk expression to indicate that args can receive 0 or any number of parameters
# The n parameters passed in when calling the function will be assembled into an n-tuple and assigned to args
# If no arguments are passed, args will be an empty tuple
def add(*args):
    total = 0
    # Loop through the tuples that hold variable parameters
    for val in args:
        # Type checking of parameters (only numeric types can be summed)
        if type(val) in (int, float):
            total += val
    return total


# When calling the add function, you can pass in 0 or any number of parameters
print(add())         # 0
print(add(1))        # 1
print(add(1, 2, 3))  # 6
print(add(1, 2, 'hello', 3.45, 6))  # 12.45
```

If we want to pass in several parameters in the form of "parameter name = parameter value", and the exact number of parameters is uncertain, we can also add variable keyword parameters to the function and assemble the passed keyword parameters into a dictionary. The code is as follows.

```python
# **kwargs in the parameter list can receive 0 or more keyword parameters
# The keyword arguments passed in when calling the function will be assembled into a dictionary (the parameter name is the key in the dictionary, and the parameter value is the value in the dictionary)
# If no keyword arguments are passed, kwargs will be an empty dictionary
def foo(*args, **kwargs):
    print(args)
    print(kwargs)


foo(3, 2.1, True, name='骆昊', age=43, gpa=4.95)
```

Output:

```
(3, 2.1, True)
{'name': 'Luo Hao', 'age': 43, 'gpa': 4.95}
```

### Use module management functions

No matter what programming language you use to write code, naming variables and functions is a headache because we will encounter embarrassing situations such as **naming conflicts**. The simplest scenario is to define two functions with the same name in the same `.py` file, as shown below.

```python
set there():
    print('hello, world!')


set there():
    print('goodbye, world!')

    
foo() # Guess what the output will be when calling the foo function
```

Of course, we can easily avoid the above situation, but if the project is a team collaboration, multiple programmers in the team may define a function named `foo`. How to solve the naming conflict in this case? The answer is actually very simple. In Python, each file represents a module. We can have functions with the same name in different modules. When using a function, we import the specified module through the `import` keyword and then use the **fully qualified name** (`module name.function name`) calling method to distinguish which module's `foo` function is to be used. The code is shown below.

`module1.py`

```python
set there():
    print('hello, world!')
```

`module2.py`

```python
set there():
    print('goodbye, world!')
```

`test.py`

```python
import module1
import module2

# Call the function using the "module name.function name" format (fully qualified name).
module1.foo()  # hello, world!
module2.foo()  # goodbye, world!
```

When importing a module, you can also use the `as` keyword to alias the module so that we can use a shorter fully qualified name.

`test.py`

```python
import module1 as m1
import module2 as m2

m1.foo()  # hello, world!
m2.foo()  # goodbye, world!
```

In the above two code snippets, we import the module that defines the function. We can also use the `from...import...` syntax to directly import the required function from the module. The code is as follows.

`test.py`

```python
from module1 import foo

foo()  # hello, world!

from module2 import foo

foo()  # goodbye, world!
```

However, if we import functions with the same name from two different modules, the function imported later will replace the previous one, just like the following code, calling `foo` will output `goodbye, world!`, because we first imported `foo` from `module1`, and then imported `foo` from `module2`. If the two `from...import...` are written in reverse, it will be a different story.

`test.py`

```python
from module1 import foo
from module2 import foo

foo()  # goodbye, world!
```

If you want to use the `foo` function from both modules in the above code, there is still a way. As you may have guessed, you can still use the `as` keyword to alias the imported function. The code is as follows.

`test.py`

```python
from module1 import foo as f1
from module2 import foo as f2

f1()  # hello, world!
f2()  # goodbye, world!
```

### Modules and functions in the standard library

The Python standard library provides a large number of modules and functions to simplify our development work. The `random` module we used before provides us with functions for generating random numbers and performing random sampling; the `time` module provides functions related to time operations; the `math` module we used before also includes a series of mathematical functions such as calculating sine, cosine, exponential, logarithm, etc. As we learn the Python language in depth, we will use more modules and functions.

There is another type of function in the Python standard library that can be used directly without `import`. We call it **built-in functions**. These built-in functions are not only useful but also commonly used. The following table lists some of the built-in functions.

| Function | Description |
| ------- | ------------------------------------------------------------ |
| `abs` | Returns the absolute value of a number, for example: `abs(-1.3)` returns `1.3`. |
| `bin` | Convert an integer to a binary string starting with `'0b'`, e.g. `bin(123)` will return `'0b1111011'`. |
| `chr` | Convert Unicode code to the corresponding character, for example: `chr(8364)` will return `'€'`. |
| `hex` | Convert an integer to a hexadecimal string starting with `'0x'`, for example: `hex(123)` will return `'0x7b'`. |
| `input` | Read a line from input and return the string read. |
| `len` | Get the length of a string, list, etc. |
| `max` | Returns the maximum value among multiple arguments or an iterable object, for example: `max(12, 95, 37)` returns `95`. |
| `min` | Returns the minimum value among multiple arguments or an iterable object, for example: `min(12, 95, 37)` returns `12`. |
| `oct` | Convert an integer to an octal string starting with `'0o'`, e.g. `oct(123)` will return `'0o173'`. |
| `open` | Opens a file and returns a file object. |
| `ord` | Convert a character to its Unicode encoding, for example: `ord('€')` will return `8364`. |
| `pow` | Exponentiation, for example: `pow(2, 3)` returns `8`; `pow(2, 0.5)` returns `1.4142135623730951`. |
| `print` | Prints the output. |
| `range` | Constructs a range sequence, for example: `range(100)` will produce a sequence of integers from `0` to `99`. |
| `round` | Rounds a number to the specified precision, for example: `round(1.23456, 4)` returns `1.2346`. |
| `sum` | Sum the items in a sequence from left to right, for example: `sum(range(1, 101))` returns `5050`. |
| `type` | Returns the type of the object, for example: `type(10)` returns `int`; and `type('hello')` returns `str`. |

### Summarize

**A function is an encapsulation of relatively independent and reusable code**. Learning how to define and use functions will allow you to write better code. Of course, the Python language standard library has provided us with a large number of modules and commonly used functions. Using these modules and functions well can do more with less code; if these modules and functions cannot meet our requirements, we may need to customize functions, and then manage these customized functions through the concept of modules.