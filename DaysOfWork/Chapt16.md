## Advanced Function Usage

We continue to explore the relevant knowledge of defining and using functions. Through previous learning, we know that functions have independent variables (parameters) and dependent variables (return values). Independent variables can be of any data type, and dependent variables can also be of any data type. Then there is a small question here. Can we use functions as function parameters and use functions as function return values? Here we first state the conclusion: **Functions in Python are "first-class functions". The so-called "first-class functions" means that functions can be assigned to variables, functions can be used as function parameters, and functions can also be used as function return values. The use of a function as a parameter or return value of other functions is usually called a "higher-order function".

### Higher-order functions

Let's go back to an example we talked about before and design a function that takes any number of parameters and performs the sum operation on the elements of type `int` or `float`. We make some adjustments to the previous code to make the whole code more compact, as shown below.

```python
def calc(*args, **kwargs):
    items = list(args) + list(kwargs.values())
    result = 0
    for item in items:
        if type(item) in (int, float):
            result += item
    return result
```

If we want the above `calc` function to not only sum multiple parameters, but also implement more or even customized binary operations, how can we do it? The above code can only sum because the `+=` operator is used in the function, which makes the function coupled with the addition operation. If this coupling relationship can be removed, the versatility and flexibility of the function will be better. The way to remove the coupling is to turn the `+` operator into a function call and design it as a function parameter. The code is shown below.

```python
def calc(init_value, op_func, *args, **kwargs):
    items = list(args) + list(kwargs.values())
    result = init_value
    for item in items:
        if type(item) in (int, float):
            result = op_func(result, item)
    return result
```

Note that the above function adds two parameters, where `init_value` represents the initial value of the operation and `op_func` represents the binary operation function. In order to call the modified function, we first define functions for addition and multiplication operations. The code is as follows.

```python
def add(x, y):
    return x + y


def multi(x, y):
    return x * y
```

If we want to do the sum operation, we can call the `calc` function as follows.

```python
print(calc(0, add, 1, 2, 3, 4, 5))  # 15
```

If we want to do the product operation, we can call the `calc` function as follows.

```python
print(calc(1, mul, 1, 2, 3, 4, 5))  # 120 
```

The `calc` function above realizes the coupling with the addition operation by turning the operator into a function parameter. This is a very clever and practical programming technique, but it may be difficult for beginners to understand. It is recommended that you take a closer look. It should be noted that in the above code, there is a significant difference between passing a function as a parameter to another function and calling the function directly. **Calling a function requires parentheses after the function name, while passing a function as a parameter only requires the function name**.

If we have not defined the `add` and `mul` functions in advance, we can also use the `add` and `mul` functions provided by the `operator` module in the Python standard library. They represent binary operations of addition and multiplication respectively. We can use them directly. The code is shown below.

```python
import operator

print(calc(0, operator.add, 1, 2, 3, 4, 5))  # 15
print(calc(1, operator.mul, 1, 2, 3, 4, 5))  # 120
```

There are many high-order functions in Python's built-in functions. The `filter` and `map` functions we mentioned earlier are high-order functions. The former can filter elements in a sequence, and the latter can map elements in a sequence. For example, if we want to remove the odd numbers in a list of integers and square all the even numbers to get a new list, we can directly use these two functions to do this. The specific steps are as follows.

```python
def is_even(num):
    """Judge whether num is an even number"""
    return num % 2 == 0


def square(num):
    """Find the square"""
    return num ** 2


old_nums = [35, 12, 8, 99, 60, 52]
new_nums = list(map(square, filter(is_even, old_nums)))
print(new_nums)  # [144, 64, 3600, 2704]
```

Of course, to complete the functions of the above code, you can also use list generation, which is simpler and more elegant.

```python
old_nums = [35, 12, 8, 99, 60, 52]
new_nums = [num ** 2 for num in old_nums if num % 2 == 0]
print(new_nums)  # [144, 64, 3600, 2704]
```

Let's discuss a built-in function `sorted`, which can implement the sorting of elements of container data types (such as lists, dictionaries, etc.). We have previously talked about the `sort` method of the `list` type, which implements the sorting of list elements. The `sorted` function is functionally no different from the `sort` method of the list, but it returns the sorted list object instead of directly modifying the original list. We call this the **side-effect-free design of the function**, which means that calling the function will not have any other effect on the state of the program or the external environment except for generating the return value. When using the `sorted` function for sorting, you can customize the sorting rules in the form of higher-order functions, which we illustrate with the following example.

```python
old_strings = ['in', 'apple', 'zoo', 'waxberry', 'pear']
new_strings = sorted(old_strings)
print(new_strings)  # ['apple', 'in', 'pear', waxberry', 'zoo']
```

The above code is not unfamiliar to everyone, but if we want to sort the list elements according to the length of the string instead of alphabetical order, we can pass a parameter called key to the sorted function and assign the key parameter to the function len that gets the length of the string. We have talked about this function in the previous lesson. The code is shown below.

```python
old_strings = ['in', 'apple', 'zoo', 'waxberry', 'pear']
new_strings = sorted(old_strings, key=len)
print(new_strings)  # ['in', 'zoo', 'pear', 'apple', 'waxberry']
```

> **Note**: The `sort` method of the list type also has the same `key` parameter. Interested readers can try it by themselves.

Lambda Function

When using higher-order functions, if the function itself as a parameter or return value is very simple and can be completed with one line of code, and there is no need to consider the reuse of the function, then we can use the lambda function. The lambda function in Python is a function without a name, so many people also call it an anonymous function. The lambda function can only have one line of code, and the result of the operation generated by the expression in the code is the return value of the anonymous function. In the previous code, the `is_even` and `square` functions we wrote only have one line of code. We can consider replacing them with lambda functions. The code is shown below.

```python
old_nums = [35, 12, 8, 99, 60, 52]
new_nums = list(map(lambda x: x ** 2, filter(lambda x: x % 2 == 0, old_nums)))
print(new_nums)  # [144, 64, 3600, 2704]
```

From the above code, we can see that the keyword for defining a lambda function is `lambda`, followed by the function's parameters. If there are multiple parameters, they are separated by commas. The part after the colon is the function's execution body, which is usually an expression. The result of the expression is the return value of the lambda function, so there is no need to write the `return` keyword.

As we said before, functions in Python are "first-class functions" and can be directly assigned to variables. After learning lambda functions, some of the functions we wrote before can be implemented with one line of code. Let's see if you can understand the following functions for factorial and prime number determination.

```python
import functools
import operator

# Implement a function to calculate factorial with one line of code
do = lambda n: functools.reduce(operator.mul, range(2, n + 1), 1)

# Use one line of code to implement the function of judging prime numbers
is_prime = lambda x: all(map(lambda f: x % f, range(2, int(x ** 0.5) + 1)))

# Call the Lambda function
print(do(6)) # 720
print(is_prime(37))  # True
```

> **Tip 1**: The `reduce` function used above is a function in the `functools` module of the Python standard library. It can implement the reduction operation on a set of data, similar to the `calc` function we defined earlier. The first parameter is the function representing the operation, the second parameter is the data to be operated, and the third parameter is the initial value of the operation. Obviously, the `reduce` function is also a high-order function. It, together with the `filter` function and the `map` function, constitutes three very critical actions in processing data: **filter**, **mapping** and **reduction**.
>
> **Tip 2**: The lambda function used to determine prime numbers above constructs a range from 2 to $\small{\sqrt{x}}$ through the `range` function, and checks whether this range has factors of `x`. The `all` function is also a built-in Python function. If all Boolean values in the passed sequence are `True`, the `all` function returns `True`, otherwise the `all` function returns `False`.

### Partial Function

A partial function is a function that generates a new function by fixing some parameters of a function, so that the same parameters do not need to be passed each time the function is called. In Python, we can use the `partial` function of the `functools` module to create partial functions. For example, the `int` function can treat strings as decimal integers for type conversion by default. If we modify its `base` parameter, we can define three new functions for converting binary, octal, and hexadecimal strings to integers, as shown below.

```python
import functools

int2 = functools.partial(int, base=2)
int8 = functools.partial(int, base=8)
int16 = functools.partial(int, base=16)

print(int('1001'))    # 1001

print(int2('1001'))   # 9
print(int8('1001'))   # 513
print(int16('1001'))  # 4097
```

Have you noticed that the first parameter and return value of the `partial` function are both functions? It processes the passed function into a new function and returns it. By constructing partial functions, we can combine the actual usage scenarios to turn the original function into a new function that is more convenient to use. I wonder if you find this operation interesting.

### Summarize

Functions in Python are first-class functions, which can be assigned to variables, or used as function parameters and return values, which means that we can use higher-order functions in Python. The concept of higher-order functions is not friendly to novices, but it brings flexibility in function design. If the function we want to define is very simple, with only one line of code, and does not require a function name to reuse it, we can use a lambda function.
