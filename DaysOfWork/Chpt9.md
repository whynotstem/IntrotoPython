## Common data structures: strings

World War II led to the birth of modern electronic computers. The world's first general-purpose electronic computer was called ENIAC (Electronic Numerical Integrator and Computer), which was built at the University of Pennsylvania in the United States. It covers an area of 167 square meters, weighs about 27 tons, and can perform about 5,000 floating-point operations per second, as shown in the figure below. After its birth, ENIAC was used to calculate missile trajectories, and numerical calculation is also one of the most important functions of modern electronic computers.

<img src="res/day11/eniac.jpg" style="zoom:50%;">

As time goes by, although numerical operations are still the most important part of daily computer work, today's computers also need to process a lot of information in the form of text. If we want to operate this text information through Python programs, we must first understand the string data type and its related operations and methods.

### String definition

The so-called **string** is a **finite sequence of zero or more characters**, generally written as:

$$
s = a_1a_2 \cdots a_n\,\,\,\,\,(0 \le n \le \infty)
$$

In Python programs, we can express a string by enclosing a single or multiple characters in single or double quotes. The characters in a string can be special symbols, English letters, Chinese characters, Japanese hiragana or katakana, Greek letters, Emoji characters (such as: 💩, 🐷, 🀄️), etc.

```python
s1 = 'hello, world!'
s2 = "Hello, world!❤️"
s3 = '''hello,
wonderful
world!'''
print(s1)
print(s2)
print(s3)
```

#### Escape character

We can use `\` (backslash) in a string to represent an escape, which means that the character after `\` no longer has its original meaning. For example, `\n` does not represent the character `\` and the character `n`, but represents a newline; `\t` does not represent the character `\` and the character `t`, but represents a tab. So if the string itself contains special characters such as `'`, `"`, and `\`, they must be escaped through `\`. For example, to output a string with single quotes or backslashes, you need to use the following method.

```python
s1 = '\'hello, world!\''
s2 = '\\hello, world!\\'
print(s1)
print(s2)
```

#### Raw string

In Python, there is a string that starts with `r` or `R`. This string is called a raw string, which means that each character in the string has its original meaning, and there is no so-called escape character. For example, in the string `'hello\n'`, `\n` means a newline; while in `r'hello\n'`, `\n` no longer means a newline, but the characters `\` and `n`. You can run the following code to see what will be output.

```python
s1 = '\it \is \time \to \read \now'
s2 = r'\it \is \time \to \read \now'
print(s1)
print(s2)
```

> **Note**: In the variable `s1` above, `\t`, `\r` and `\n` are escape characters. `\t` is a tab character, `\n` is a new line character, and `\r` is a carriage return character, which is equivalent to returning the output to the beginning of the line. Compare the output of the two `print` functions to see what the difference is!

#### Special representation of characters

Python also allows an octal or hexadecimal number to represent a character after `\`. For example, `\141` and `\x61` both represent the lowercase letter `a`. The former is an octal representation, and the latter is a hexadecimal representation. Another way to represent a character is to follow the Unicode character code after `\u`. For example, `\u9a86\u660a` represents the Chinese character "洛昊". Run the following code and see what is output.

```python
s1 = '\141\142\143\x61\x62\x63'
s2 = '\u9a86\u660a'
print(s1)
print(s2)
```

### String operations

Python provides a rich set of operators for string types, many of which are similar to list operators. For example, we can use the + operator to concatenate strings, the * operator to repeat the contents of a string, in and not in to determine whether a string contains another string, and we can use the [] and [:] operators to extract a character or some characters from a string.

#### Concatenation and repetition

The following example demonstrates the use of the `+` and `*` operators to implement string concatenation and repetition operations.

```python
s1 = 'hello' + ', ' + 'world'
print(s1)    # hello, world
s2 = '!' * 3
print(s2)    # !!!
s1 += s2
print(s1)    # hello, world!!!
s1 *= 2
print(s1)    # hello, world!!!hello, world!!!
```

Using `*` to repeat a string is a very interesting operator. In many programming languages, to represent a string with 10 `a`, you can only write `'aaaaaaaaaa'`, but in Python, you can write `'a' * 10`. You may think that there is nothing inconvenient in writing `'aaaaaaaaaa'`, but think about it, what if the character `a` is repeated 100 times or 1000 times?

#### Comparison operations

For two string type variables, you can directly use comparison operators to determine the equality of the two strings or compare the size. It should be noted that because the string also exists in binary form in the computer memory, the size of the string is compared by the size of the code corresponding to each character. For example, the code of `A` is `65`, and the code of `a` is `97`, so the result of `'A' < 'a'` is equivalent to the result of `65 < 97`, which is obviously `True` here; and `'boy' < 'bad'`, because the first character is `'b'`, the size cannot be compared, so the actual comparison is the size of the second character, obviously `'o' < 'a'` The result is `False`, so the result of `'boy' < 'bad'` is `False`. If you don't know what the code corresponding to the two characters is, you can use the `ord` function to get it, which we mentioned before. For example, the value of `ord('A')` is `65`, and the value of `ord('昊')` is `26122`. The following code shows the comparison operation of strings. Please take a closer look.

```python
s1 = 'a whole new world'
s2 = 'hello world'
print(s1 == s2) # False
print(s1 < s2)              # True
print(s1 == 'hello world')  # False
print(s2 == 'hello world')  # True
print(s2 != 'Hello world')  # True
s3 = 'Luo Hao'
print(ord('Luo')) # 39558
print(ord('昊')) # 26122
s4 = 'Wang Dachui'
print(word('王')) # 29579
print(word('大')) # 22823
print(ord('hammer')) # 38180
print(s3 >= s4)             # True
print(s3 != s4)             # True
```

#### Member operations

In Python, you can use `in` and `not in` to determine whether a string contains another character or string. Like the list type, `in` and `not in` are called member operators, which will produce Boolean values `True` or `False`. The code is as follows.

```python
s1 = 'hello, world'
s2 = 'goodbye, world'
print('wo' in s1)      # True
print('wo' not in s2)  # False
print(s2 in s1)        # False
```

#### Get the string length

Getting the length of a string is the same as getting the number of list elements. Use the built-in function `len` as shown below.

```python
s = 'hello, world'
print(len(s)) # 12
print(len('goodbye, world'))  # 14
```

#### Indexing and slicing

The indexing and slicing operations of strings are almost the same as those of lists and tuples, because strings are also ordered sequences, and their elements can be accessed through forward or reverse integer indices. However, one thing to note is that because **strings are immutable types**, **characters in strings cannot be modified through index operations**.

```python
s = 'abc123456'
n = len(s)
print(s[0], s[-n])    # a a
print(s[n-1], s[-1])  # 6 6
print(s[2], s[-7])    # c c
print(s[5], s[-4])    # 3 3
print(s[2:5])         # c12
print(s[-7:-4])       # c12
print(s[2:])          # c123456
print(s[:2])          # ab
print(s[::2])         # ac246
print(s[::-1])        # 654321cba
```

It is important to remind you again that when performing index operations, if the index is out of bounds, an `IndexError` exception will be thrown, and the error message will be: `string index out of range`.

### Character traversal

If you want to iterate over each character in a string, you can use a for-in loop. There are two ways to do this as shown below.

Method 1:

```python
s = 'hello'
for i in range(len(s)):
    print(s[i])
```

Method 2:

```python
s = 'hello'
for elem in s:
    print(item)
```

### String methods

In Python, we can operate and process strings through the methods that come with the string type. Suppose we have a string named `foo` and the string has a method named `bar`, then the syntax for using the string method is: `foo.bar()`, which is a syntax for calling an object method through an object reference, which is the same as the syntax for using the list method mentioned above.

#### Case-related operations

The following code demonstrates methods related to string case conversion.

```python
s1 = 'hello, world!'
# Capitalize the first letter of a string
print(s1.capitalize())  # Hello, world!
# Capitalize the first letter of each word in the string
print(s1.title())       # Hello, World!
# Make the string uppercase
print(s1.upper())       # HELLO, WORLD!
s2 = 'GOODBYE'
# Make the string lowercase
print(s2.lower())       # goodbye
# Check the values of s1 and s2
print(s1)               # hello, world
print(s2)               # GOODBYE
```

> **Note**: Since strings are immutable, using string methods to operate on strings will generate new strings, but the values of the original variables will not change. So in the above code, when we finally check the values of the two variables `s1` and `s2`, the values of `s1` and `s2` have not changed.

#### Find Operation

If you want to find another string from the beginning to the end in a string, you can use the string's `find` or `index` method. When using the `find` and `index` methods, you can also specify the search range through the method's parameters, that is, the search does not have to start from the index `0`.

```python
s = 'hello, world!'
print(s.find('or'))      # 8
print(s.find('or', 9))   # -1
print(s.find('of'))      # -1
print(s.index('or'))     # 8
print(s.index('or', 9))  # ValueError: substring not found
```

>**Note**: The `find` method will return `-1` if it cannot find the specified string, and the `index` method will raise a `ValueError` error if it cannot find the specified string.

The `find` and `index` methods also have reverse search (search from back to front) versions, namely `rfind` and `rindex`, the code is shown below.

```python
s = 'hello world!'
print(s.find('o'))       # 4
print(s.rfind('o'))      # 7
print(s.rindex('o'))     # 7
# print(s.rindex('o', 8))  # ValueError: substring not found
```

#### Nature judgment

You can use the string's `startswith` and `endswith` to determine whether a string starts and ends with a certain string; you can also use methods starting with `is` to determine the characteristics of the string. These methods all return Boolean values. The code is shown below.

```python
s1 = 'hello, world!'
print(s1.startswith('He'))   # False
print(s1.startswith('hel'))  # True
print(s1.endswith('!'))      # True
s2 = 'abc123456'
print(s2.isdigit())  # False
print(s2.isalpha())  # False
print(s2.isalnum())  # True
```

> **Explanation**: The `isdigit` above is used to determine whether a string is composed entirely of numbers, `isalpha` is used to determine whether a string is composed entirely of letters, where letters refer to Unicode characters but do not include Emoji characters, and `isalnum` is used to determine whether a string is composed of letters and numbers.

#### Formatting

In Python, string types can be centered, left-aligned, or right-aligned using the `center`, `ljust`, and `rjust` methods. If you want to pad the left side of a string with zeros, you can also use the `zfill` method.

```python
s = 'hello, world'
print(s.center(20, '*'))  # ****hello, world****
print(s.rjust(20))        #         hello, world
print(s.bright(20, '~')) # hello, world~~~~~~~~
print('33'.zfill(5))      # 00033
print('-33'.zfill(5))     # -0033
```

We have mentioned before that when using the `print` function to output a string, the string can be formatted in the following way.

```python
a = 321
b = 123
print('%d * %d = %d' % (a, b, a * b))
```

Of course, we can also use the `format` method of the string to complete the string format. The code is as follows.

```python
a = 321
b = 123
print('{0} * {1} = {2}'.format(a, b, a * b))
```

Starting from Python 3.6, there is a more concise way to write formatted strings, which is to add `f` before the string to format the string. In this string starting with `f`, `{variable name}` is a placeholder and will be replaced by the value corresponding to the variable. The code is as follows.

```python
a = 321
b = 123
print(f'{a} * {b} = {a * b}')
```

If you need to further control the form of the variable value in the formatting syntax, you can refer to the table below to perform string formatting operations.

| variable value | placeholder | format result | description |
| ----------- | ---------- | ------------- | ---- |
| `3.1415926` | `{:.2f}` | `'3.14'` | round to two decimal places |
| `3.1415926` | `{:+.2f}` | `'+3.14'` | Signed to two decimal places |
| `-1` | `{:+.2f}` | `'-1.00'` | Signed to two decimal places |
| `3.1415926` | `{:.0f}` | `'3'` | without decimal |
| `123` | `{:0>10d}` | `'0000000123'` | Add `0` on the left to make it 10 digits |
| `123` | `{:x<10d}` | `'123xxxxxxx'` | Add `x` to the right to make it 10 digits |
| `123` | `{:>10d}` | `' 123'` | Add spaces on the left to make it 10 digits |
| `123` | `{:<10d}` | `'123 '` | Add spaces on the right to make sure there are 10 digits |
| `123456789` | `{:,}` | `'123,456,789'` | comma-separated format |
| `0.123` | `{:.2%}` | `'12.30%'` | Percentage format |
| `123456789` | `{:.2e}` | `'1.23e+08'` | scientific notation format |

#### Trim Operation

The `strip` method of a string can help us get a string after trimming the specified characters on both ends of the original string. By default, it trims the space characters. This method is very practical and can be used to remove the leading and trailing spaces that the user accidentally types when inputting. The `strip` method also has two versions, `lstrip` and `rstrip`. I believe you have guessed what these two methods are used for from the names.

```python
s1 = ' jackfrued@126.com '
print(s1.strip())      # jackfrued@126.com
s2 = '~Hello, World~'
print(s2.lstrip('~')) # Hello, world~
print(s2.rstrip('~')) # ~Hello, World
```

#### Replace Operation

If you want to replace the specified content in the string with new content, you can use the `replace` method. The code is as follows. The first parameter of the `replace` method is the content to be replaced, the second parameter is the content after replacement, and the third parameter can also be used to specify the number of replacements.

```python
s = 'hello, good world'
print(s.replace('o', '@'))     # hell@, g@@d w@rld
print(s.replace('o', '@', 1))  # hell@, good world
```

#### Split and merge

You can use the string's `split` method to split a string into multiple strings (put them in a list), or you can use the string's `join` method to join multiple strings in a list into one string. The code is as follows.

```python
s = 'I love you'
words = s.split()
print(words)            # ['I', 'love', 'you']
print('~'.join(words))  # I~love~you
```

It should be noted that the `split` method uses spaces for splitting by default. We can also specify other characters to split the string, and we can also specify the maximum number of splits to control the splitting effect. The code is shown below.

```python
s = 'I#love#you#so#much'
words = s.split('#')
print(words)  # ['I', 'love', 'you', 'so', 'much']
words = s.split('#', 2)
print(words)  # ['I', 'love', 'you#so#much']
```

#### Encoding and Decoding

In addition to the string `str` type, Python also has a byte string type (`bytes`) that represents binary data. A byte string is a finite sequence of zero or more bytes. Through the `encode` method of a string, we can encode a string into a byte string in a certain encoding method. We can also use the `decode` method of a byte string to decode a byte string into a string. The code is as follows.

```python
a = 'Luo Hao'
b = a.encode('utf-8')
c = a.encode('gbk')
print(b) # b'\xe9\xaa\x86\xe6\x98\x8a'
print(c)                  # b'\xc2\xe6\xea\xbb'
print(b.decode('utf-8'))  # 骆昊
print(c.decode('gbk')) # Luo Hao
```

Note that if the encoding and decoding methods are inconsistent, it will cause garbled characters (the original content cannot be reproduced) or cause a `UnicodeDecodeError` error, causing the program to crash.

#### Other methods

For string types, another common operation is to check whether the string meets a certain pattern. For example, a website's check of the user name and email address in the user registration information is a pattern matching check. The tool for implementing pattern matching checks is called regular expressions. Python language provides support for regular expressions through the `re` module in the standard library. We will explain this knowledge point to you in subsequent courses.

### Summarize

Knowing how to represent and manipulate strings is very important for programmers, because we often need to process text information. In Python, you can use operators such as concatenation, indexing, and slicing to manipulate strings, or you can use the very rich methods provided by the string type.
