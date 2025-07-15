## Function application practice

### Example 1: Random verification code

Design a function to generate a random verification code. The verification code consists of numbers and uppercase and lowercase English letters. The length can be set by parameters.

```python
import random
import string

ALL_CHARS = string.digits + string.ascii_letters


def generate_code(*, code_len=4):
    """
    Generate a verification code of specified length
    :param code_len: length of verification code (4 characters by default)
    :return: A random verification code string consisting of uppercase and lowercase English letters and numbers
    """
    return ''.join(random.choices(ALL_CHARS, k=code_len))
```

> **Note 1**: `digits` of the `string` module represents the string `'0123456789'` consisting of the numbers from 0 to 9, and `ascii_letters` of the `string` module represents the string `'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'` consisting of uppercase and lowercase English letters.
>
> **Note 2**: Both the `sample` and `choices` functions of the `random` module can implement random sampling. `sample` implements sampling without replacement, which means that the elements sampled are not repeated; `choices` implements sampling with replacement, which means that some elements may be repeatedly selected. The first parameter of these two functions represents the sampling population, and the parameter `k` represents the sample size. It should be noted that the parameter `k` of the `choices` function is a named keyword parameter, and the parameter name must be specified when passing parameters.

You can use the following code to generate 5 sets of random verification codes to test the above function.

```python
for _ in range(5):
    print(generate_code()) 
```

Output:

```
59tZ
QKU5
izq8
IBBb
jIfX
```

or

```python
for _ in range(5):
    print(generate_code(code_len=6))
```

Output:

```
FxJucw
HS4H9G
0yyXfz
x7fohf
ReO22w
```

> **Note**: The parameter of the `generate_code` function we designed is a named keyword parameter. Since it has a default value, you don't need to pass it a value and use the default value 4. If you need to pass a parameter to the function, you must specify the parameter name `code_len`.

### Example 2: Determining prime numbers

Design a function to determine whether a given positive integer greater than 1 is prime. A prime number is a positive integer (greater than 1) that can only be divided by 1 and itself. If a positive integer $\small{N}$ greater than 1 is prime, it means that it has no factors between 2 and $\small{N-1}$.

```python
def is_prime(num: int) -> bool:
    """
    Determine whether a positive integer is prime
    :param num: a positive integer greater than 1
    :return: Returns True if num is a prime number, otherwise returns False
    """
    for i in range(2, int(num ** 0.5) + 1):
        if num % i == 0:
            return False
    return True
```

> **Note 1**: The `: int` after the parameter `num` of the `is_prime` function above is used to mark the type of the parameter. Although it has no effect on the execution result of the code, it greatly enhances the readability of the code. Similarly, the `-> bool` after the parameter list is used to mark the type of the function return value. It also has no effect on the execution result of the code, but it makes us know clearly that calling the function will get a Boolean value, either `True` or `False`.
>
> **Note 2**: The above loop does not need to loop from 2 to $\small{N-1}$, because if the factors of $\small{N}$ have not been found when the loop reaches $\small{\sqrt{N}}$, then the factors of $\small{N}$ will not appear after $\small{\sqrt{N}}$. You can think about why this is the case.

### Example 3: Greatest common divisor and least common multiple

Design a function to calculate the greatest common divisor and least common multiple of two positive integers. The greatest common divisor of $\small{x}$ and $\small{y}$ is the largest integer that can divide both $\small{x}$ and $\small{y}$. If $\small{x}$ and $\small{y}$ are relatively prime, then their greatest common divisor is 1; The least common multiple of $\small{x}$ and $\small{y}$ is the smallest positive integer that can be divided by both $\small{x}$ and $\small{y}$. If $\small{x}$ and $\small{y}$ are relatively prime, then their least common multiple is $\small{x \times y}$. It should be noted that calculating the greatest common divisor and the least common multiple are two different functions and should be designed as two functions instead of putting the two functions into the same function.

```python
def lcm(x: int, y: int) -> int:
    """Find the least common multiple"""
    return x * y // gcd(x, y)


def gcd(x: int, y: int) -> int:
    """Find the greatest common divisor"""
    while y % x != 0:
        x, y = y % x, x
    return x
```

> **Note**: Functions can call each other. The `lcm` function for finding the least common multiple above calls the `gcd` function for finding the greatest common divisor, and the least common multiple is calculated by $\frac{x \times y}{ gcd(x, y)}$.

### Example 4: Data statistics

Assuming that the sample data is saved in a list, design a function to calculate the descriptive statistics of the sample data. Descriptive statistics usually include: arithmetic mean, median, range (difference between maximum and minimum values), variance, standard deviation, coefficient of variation, etc. The calculation formula is as follows.

Sample mean:

$$
\bar{x} = \frac{\sum_{i=1}^{n}x_{i}}{n} = \frac{x_{1}+x_{2}+\cdots +x_{n}}{n}
$$

Sample variance:

$$
s^2 = \frac {\sum_{i=1}^{n}(x_i - \bar{x})^2} {n-1}
$$

Sample standard deviation:

$$
s = \sqrt{\frac{\sum_{i=1}^{n}(x_i - \bar{x})^2}{n-1}}
$$

Coefficient of variation (coefficient of sample variation):

$$
CV = \frac{s}{\bar{x}}
$$

```python
def ptp(data):
    """Range (full range)"""
    return max(data) - min(data)


def mean(data):
    """Arithmetic mean"""
    return sum(data) / len(data)


def median(data):
    """median"""
    temp, size = sorted(data), len(data)
    if size % 2 != 0:
        return temp[size // 2]
    else:
        return mean(temp[size // 2 - 1:size // 2 + 1])


def var(data, come=1):
    """variance"""
    x_bar = mean(data)
    temp = [(num - x_bar) ** 2 for num in data]
    return sum(temp) / (len(temp) - dof)


def std(data, dof=1):
    """Standard deviation"""
    return var(data, dof) ** 0.5


def cv(data, dof=1):
    """Coefficient of variation"""
    return std(data, ddof) / mean(data)


def describe(data):
    """Output descriptive statistics"""
    print(f'mean: {mean(data)}')
    print(f'median: {median(data)}')
    print(f'Range: {ptp(data)}')
    print(f'Variance: {var(data)}')
    print(f'Standard deviation: {std(data)}')
    print(f'Coefficient of variation: {cv(data)}')
```

> **Note 1**: The median is the number in the middle after the data is arranged in ascending or descending order. It describes the average level of the data. The calculation of the median is divided into two cases: when the data volume $n$ is an odd number, the median is the element at the $\frac{n + 1}{2}$ position; when the data volume $\small{n}$ is an even number, the median is the mean of the elements at the $\frac{n}{2}$ and $\frac{n}{2} + 1$ positions.
>
> **Note 2**: There is a parameter called `ddof` in the function for calculating variance and standard deviation, which represents the adjustable degrees of freedom, and the default value is 1. When calculating sample variance and sample standard deviation, degrees of freedom correction is required; if you want to calculate population variance and population standard deviation, you can assign the `ddof` parameter a value of 0, that is, no degrees of freedom correction is required.
>
> **Note 3**: The `describe` function assembles the above encapsulated statistical functions together to output the descriptive statistical information of the data. In fact, there is a module called `statistics` in the Python standard library, which has encapsulated the functions for obtaining descriptive statistical information. Interested readers can learn more about it.

### Example 5: Randomly select numbers for the Double Color Ball

We use functions to reconstruct the example of random number selection of the Double Color Ball that we talked about before (Lesson 09: List of Common Data Structures-2), encapsulate the functions of generating random numbers and outputting a group of numbers into two functions respectively, and then implement the function of randomly selecting `N` numbers by calling the functions.

```python
"""
Double Color Ball Random Number Selection Program

Author: Luo Hao
Version: 1.3
"""
import random

RED_BALLS = [i for i in range(1, 34)]
BLUE_BALLS = [i for i in range(1, 17)]


def choose():
    """
    Generate a set of random numbers
    :return: Save the list of random numbers
    """
    selected_balls = random.sample(RED_BALLS, 6)
    selected_balls.sort()
    selected_balls.append(random.choice(BLUE_BALLS))
    return selected_balls


def display(balls):
    """
    Output a set of numbers in the format
    :param balls: holds a list of random numbers
    """
    for ball in balls[:-1]:
        print(f'\033[031m{ball:0>2d}\033[0m', end=' ')
    print(f'\033[034m{balls[-1]:0>2d}\033[0m')


n = int(input('Generate several numbers: '))
for _ in range(n):
    display(choose())
```

> **Explanation**: Let's take a look at the line of code `display(choose())`. Here we first get a set of random numbers through the `choose` function, and then use the return value of the `choose` function as the parameter of the `display` function to display the selected random numbers through the `display` function. The code logic after refactoring is very clear and the code is more readable. If someone encapsulates these two functions for you, you are just the caller of the function. In fact, you don't need to care about the internal implementation of the `choose` function and the `display` function. You only need to know that calling the `choose` function can generate a set of random numbers, and calling the `display` function to pass in a list can output this set of numbers. In the future, when we use various Python third-party libraries, we will not pay attention to their underlying implementation at all. All we need to know is which function to call to solve the problem.

### Summarize

When writing code, especially when developing commercial projects, you must consciously encapsulate relatively independent and reusable functions into functions, so that both you and other team members can use these functions by calling functions, reducing repetitive and tedious work.