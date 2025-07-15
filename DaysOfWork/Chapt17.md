## Advanced application of functions

In the previous chapter, we explored higher-order functions in Python. I believe that everyone has a deeper understanding of the definition and application of functions. In this chapter, we will continue to explain the knowledge related to functions, one is the characteristic syntax decorator in Python, and the other is the recursive call of functions.

### Decorators

In Python, decorator is a grammatical phenomenon of "**decorating another function with a function and providing it with additional capabilities**". The decorator itself is a function, its parameter is the decorated function, and its return value is a function with decoration function. From the previous description, I believe everyone has heard that the decorator is a high-order function, and its parameters and return values are both functions. However, the concept of decorator is still a headache for beginners of programming languages. Let's first use a simple example to illustrate the role of decorators. Suppose there are two functions named `downlaod` and `upload`, which are used for uploading and downloading files respectively, as shown below.

```python
import random
import time


def download(filename):
    """Download file"""
    print(f'Start downloading {filename}.')
    time.sleep(random.random() * 6)
    print(f'{filename} download completed.')

    
def upload(filename):
    """Upload file"""
    print(f'Start uploading {filename}.')
    time.sleep(random.random() * 8)
    print(f'{filename} upload completed.')

    
download('MySQL from deleting the database to running away.avi')
upload('Python from entry to hospitalization.pdf')
```

> **Note**: The above code simulates the time it takes to download and upload files by sleeping for a random period of time, and does not actually upload or download files online. It is also very simple to use Python to upload and download files online, and we will talk about related knowledge later.

Now there is a new requirement. We want to know how long it takes to call the `download` and `upload` functions to upload or download files. How can this be achieved? I believe many friends have already thought of it. We can record a time when the function starts executing and a time after the function call ends. The two times can be subtracted to calculate the download or upload time. The code is as follows.

```python
start = time.time()
download('MySQL from deleting the database to running away.avi')
end = time.time()
print(f'Time taken: {end - start:.2f} seconds')
start = time.time()
upload('Python from entry to hospitalization.pdf')
end = time.time()
print(f'Time taken: {end - start:.2f} seconds')
```

Through the above code, we can record the time spent when downloading and uploading files, but I wonder if you have noticed that the codes for recording time, calculating and displaying execution time are all repeated codes. People with programming experience know that **repeated code is the root of all evil**, so is there a way to record the execution time of functions in a simple and elegant way without writing repeated code? In Python language, decorators are the best choice to solve this kind of problem. Through the decorator syntax, we can encapsulate the timing function code that has nothing to do with the original business (upload and download) into a function. If the `upload` and `download` functions need to record time, we can directly apply the decorator to these two functions. Since it is mentioned above that the decorator is a high-order function, its parameters and return values are both functions. We will tentatively name the decorator for recording time as `record_time`, then its overall structure should be as shown in the following code.

```python
def record_time(func):
    
    def wrapper(*args, **kwargs):
        
        result = func(*args, **kwargs)
        
        return result
    
    return wrapper
```

I believe you have noticed that the parameter `func` of the `record_time` function represents a decorated function. The `wrapper` function defined in the function is a decorated function. It will execute the decorated function `func` and return the return value of the function execution at the end. I wonder if you have noticed that I left two blank lines in the 4th and 6th lines of the above code, which means that we can add code in these places to implement additional functions. The `record_time` function will eventually return the decorated function `wrapper` and replace the original function `func` with it. When the original function `func` is decorated by the `record_time` function, we actually call the `wrapper` function when calling it, so it has additional capabilities. The parameters of the `wrapper` function are special. Since we want to use `wrapper` to replace the original function `func`, but we don’t know what parameters the original function `func` will accept, we accept them all through variable parameters and keyword parameters, and then give them all to it when calling `func`. It should also be emphasized here that Python supports nested definition of functions. As above, we can define the `wrapper` function in the `record_time` function, which is not supported in many programming languages.

After understanding this structure, we can write the function of recording time into this decorator. The code is as follows.

```python
import time


def record_time(func):

    def wrapper(*args, **kwargs):
        # Record the start time before executing the decorated function
        start = time.time()
        # Execute the decorated function and get the return value
        result = func(*args, **kwargs)
        # Record the end time after executing the decorated function
        end = time.time()
        # Calculate and display the execution time of the decorated function
        print(f'{func.__name__} execution time: {end - start:.2f} seconds')
        # Return the return value of the decorated function
        return result
    
    return wrapper
```

Although writing a decorator is quite troublesome, it is a one-time operation. In the future, when there is a need to record the execution time of a function, we only need to add the above decorator. There are two ways to use the above decorator function. The first way is to directly call the decorator function, pass in the decorated function and get the return value. We can use this return value to directly replace the original function, so when calling, we have obtained the additional ability provided by the decorator (recording execution time). You will understand it by trying the following code.

```python
download = record_time(download)
upload = record_time(upload)
download('MySQL from deleting the database to running away.avi')
upload('Python from entry to hospitalization.pdf')
```

In Python, decorators are very convenient for adding syntax sugar (a syntax added to a programming language that has no effect on the language's functionality, but is more convenient to use and makes the code more readable. We call it "syntax sugar" or "sugar-coated syntax"). You can use `@decorator function` to place the decorator function directly on the decorated function, and the effect is the same as the above code. We list the complete code for you, and you can see how we define and use decorators.

```python
import random
import time


def record_time(func):

    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f'{func.__name__} execution time: {end - start:.2f} seconds')
        return result

    return wrapper


@record_time
def download(filename):
    print(f'Start downloading {filename}.')
    time.sleep(random.random() * 6)
    print(f'{filename} download completed.')


@record_time
def upload(filename):
    print(f'Start uploading {filename}.')
    time.sleep(random.random() * 8)
    print(f'{filename} upload completed.')


download('MySQL from deleting the database to running away.avi')
upload('Python from entry to hospitalization.pdf')
```

In the above code, we added decorators to the `download` and `upload` functions through decorator syntax sugar. The decorated `download` and `upload` functions are actually the `wrapper` functions we returned in the decorator. Calling them is actually calling the `wrapper` function, so there is a function to record the execution time of the function.

If we want to remove the effect of the decorator and execute the original function in some places of the code, we need to do a little extra work when defining the decorator function. The `wraps` function of the `functools` module of the Python standard library is also a decorator. We put it on the `wrapper` function. This decorator can help us retain the function before being decorated, so that when we need to cancel the decorator, we can get the function before being decorated through the `__wrapped__` attribute of the decorated function.

```python
import random
import time

from functools import wraps


def record_time(func):

    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f'{func.__name__} execution time: {end - start:.2f} seconds')
        return result

    return wrapper


@record_time
def download(filename):
    print(f'Start downloading {filename}.')
    time.sleep(random.random() * 6)
    print(f'{filename} download completed.')


@record_time
def upload(filename):
    print(f'Start uploading {filename}.')
    time.sleep(random.random() * 8)
    print(f'{filename} upload completed.')


# Calling the decorated function will record the execution time
download('MySQL from deleting the database to running away.avi')
upload('Python from entry to hospitalization.pdf')
# Cancel the decorator and do not record the execution time
download.__wrapped__('MySQL Must Know.pdf')
upload.__wrapped__('Python from Novice to Master.pdf')
```

**The decorator function itself can also be parameterized**. Simply put, the decorator can also be customized through the parameters passed in by the caller. We will explain this knowledge point later when we use it.

### Recursive call

Python allows nested definitions of functions, and also allows functions to call each other, and a function can call itself directly or indirectly. A function calling itself is called a recursive call, so what is the use of recursive calls? In reality, the definition of many problems is a recursive definition itself, such as the factorial we talked about before. The factorial of a non-negative integer `N` is the factorial of `N` multiplied by `N-1`, that is, $\small{N! = N \times (N-1)!}$. The concept of factorial appears on both the left and right sides of the definition, so this is a recursive definition. In this case, we can use recursive calls to write a function to calculate the factorial. The code is shown below.

```python
def do(num):
    if num in (0, 1):
        return 1
    return num * do(num - 1)
```

In the above code, the `fac` function is called again in the `fac` function, which is called a recursive call. The `if` condition in line 2 of the code is called the convergence condition of the recursion. Simply put, it is when to end the recursive call of the function. When calculating the factorial, if the factorial of `0` or `1` is calculated, the recursive call stops and `1` is returned directly; `num * fac(num - 1)` in line 4 of the code is a recursive formula, which is also the recursive definition of the factorial. Next, we briefly analyze what the whole process will be like if we use `fac(5)` to calculate the factorial of `5`.

```python
# Recursively call functions onto the stack
# 5 * do(4)
# 5 * (4 * do(3))
# 5 * (4 * (3 * do(2)))
# 5 * (4 * (3 * (2 * do(1))))
# Stop recursive function popping
# 5 * (4 * (3 * (2 * 1)))
# 5 * (4 * (3 * 2))
# 5 * (4 * 6)
# 5 * 24
# 120
print(do(5)) # 120
```

Note that function calls save the current code execution site through a data structure called a "stack" in memory. After the function call ends, the previous execution site will be restored through this stack structure. The stack is a first-in, last-out data structure, which means that the earliest function pushed into the stack will return last, and the last function pushed into the stack will return first. For example, if a function named `a` is called, the execution body of function `a` calls function `b`, and the execution body of function `b` calls function `c`, then the first function pushed into the stack is `a`, and the first function popped out of the stack is `c`. Each time a function call is entered, the stack will increase a stack frame, which is the structure we just mentioned to save the current code execution site; each time a function call ends, the stack will reduce a stack frame. Usually, the stack space in the memory is very small, so if the number of recursive calls is too many, it will cause a stack overflow, so **recursive calls must ensure that they can converge quickly**. We can try to execute `fac(5000)` to see if a `RecursionError` error is prompted. The error message is: `maximum recursion depth exceeded in comparison`, which means a stack overflow has occurred.

If we use the official Python interpreter (CPython), the maximum depth of the stack structure of function calls is set to 1000 by default. If this depth is exceeded, the above-mentioned RecursionError will occur. Of course, we can use the setrecursionlimit function of the sys module to change the maximum depth of recursive calls, but we do not recommend doing so, because making recursion converge quickly is what we should do, otherwise we should consider using loop recursion instead of recursion.

Let's take another example of generating the Fibonacci sequence that we talked about before. Since the first two numbers in the Fibonacci sequence are both 1, starting from the third number, each number is the sum of the previous two numbers, which can be written as f(n) = f(n - 1) + f(n - 2). Obviously, this is another recursive definition, so we can use the following recursive call function to calculate the ​n`th Fibonacci number.

```python
def fib1(n):
    if n in (1, 2):
        return 1
    return fib1(n - 1) + fib1(n - 2)


for i in range(1, 21):
    print(fib1(i))
```

I need to remind you that although the code for calculating Fibonacci numbers above looks very simple and clear, its execution performance is relatively poor. You can try to change the second parameter of the `range` function in the `for` loop in the above code to `51`, that is, output the first 50 Fibonacci numbers, and see how long it takes. You are also welcome to leave your code execution time in the comment area. As for why it is so slow, you can think about the reason yourself. Obviously, it is a better choice to directly use the loop recursion method to obtain the Fibonacci sequence. The code is shown below.

```python
def fib2(n):
    a, b = 0, 1
    for _ in range(n):
        a, b = b, a + b
    return a
```

In addition, we can also use the `lru_cache` function of the `functools` module in the Python standard library to optimize the above recursive code. The `lru_cache` function is a decorator function. We put it on the above function `fib1`. It can cache the execution results of the function to avoid a large number of repeated operations during the recursive call process, so that the execution performance of the code will be improved "flying". You can try to output the first 50 Fibonacci numbers and see how long the code takes to execute after adding the decorator. See the comment section!

```python
from functools import lru_cache


@lru_cache()
def fib1(n):
    if n in (1, 2):
        return 1
    return fib1(n - 1) + fib1(n - 2)


for i in range(1, 51):
    print(i, fib1(i))
```

> **Tip**: The `lru_cache` function is a decorator with parameters, so when using decorator syntax sugar in the fourth line of code above, `lru_cache` must be followed by parentheses. The `lru_cache` function has a very important parameter called `maxsize`, which can be used to define the size of the cache space. The default value is 128.

### Summarize

Decorators are a special syntax in Python. **Decorators can be used to enhance existing functions**, which is a very useful programming technique. On the other hand, through recursive function calls, some complex problems can be simplified at the code level, but **recursive calls must pay attention to convergence conditions and recursive formulas**. Only when the recursive formula is found can recursive calls be used, and the convergence conditions ensure that the recursive calls can stop. Function calls use the stack space in the memory to save and restore the scene. The stack space is usually very small, so **if the recursion cannot converge quickly, it is likely to cause a stack overflow error, resulting in the crash of the program**.