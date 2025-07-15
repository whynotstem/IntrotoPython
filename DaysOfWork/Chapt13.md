## Common data structure dictionary

So far, we have introduced three container data types in Python (list, tuple, set), but these data types are still not enough to help us solve all problems. For example, we need a variable to save a person's multiple information, including: name, age, height, weight, home address, personal mobile phone number, emergency contact mobile phone number, then you will find that the list, tuple and set types we have learned before are not good enough.

```python
person1 = ['王大锤', 55, 168, 60, 'Room 101, Building 1, No. 62, Kehua North Road, Wuhou District, Chengdu', '13122334455', '13800998877']
person2 = ('王大锤', 55, 168, 60, 'Room 101, Building 1, No. 62, Kehua North Road, Wuhou District, Chengdu', '13122334455', '13800998877')
person3 = {'王大锤', 55, 168, 60, 'Room 101, Building 1, No. 62, Kehua North Road, Wuhou District, Chengdu', '13122334455', '13800998877'}
```

Sets are definitely the least suitable, because there cannot be duplicate elements in a set. If a person's age and weight are exactly the same, then there will be one less piece of information in the set; similarly, if the person's mobile phone number is the same as the emergency contact's mobile phone number, then there will be another piece of information missing in the set. On the other hand, although lists and tuples can save all of a person's information, when you want to get the person's mobile phone number or home address, you must first know which element of the list or tuple his mobile phone number is. In short, when encountering the above scenario, lists, tuples, and sets are not the most appropriate choices. At this time, we need a dictionary type. This data type is most suitable for assembling related information together, which can help us solve the problem of modeling real things in Python programs.

When it comes to the word dictionary, everyone must be familiar with it. When we were in elementary school, basically everyone had a "Xinhua Dictionary" in their hands, as shown in the picture below.

<img src="res/day13/xinhua_dictionary.jpg" style="zoom:80%;">

The dictionary in Python program is very similar to the dictionary in real life. It organizes data together in the form of key-value pairs (a combination of key and value). We can find the corresponding value through the key and perform operations. Just like in the Xinhua Dictionary, each word (key) has its corresponding explanation (value), each word and its explanation together is an entry in the dictionary, and the dictionary usually contains many such entries.

### Creating and using dictionaries

In Python, you can use the `{}` literal syntax to create a dictionary, which is the same as the set we learned in the previous lesson. However, the elements in the `{}` of the dictionary exist in the form of key-value pairs, and each element consists of two values separated by `:`. The key is before `:`, and the value is after `:`. The code is as follows.

```python
xinhua = {
    '麓': 'at the foot of the mountain',
    '路': 'Road, a place for people to travel; aspect, region: southern goods, foreign goods; type: they are the same person',
    '蕗': 'Another name for licorice',
    'Lu': 'Lushui, a river name, is now the Zhuozhang River in Shanxi Province; Lujiang, a river name, is now the Nujiang River in Yunnan Province'
}
print(xinhua)
person = {
    'name': 'Wang Dachui',
    'age': 55,
    'height': 168,
    'weight': 60,
    'addr': 'Room 101, Building 1, No. 62, Kehua North Road, Wuhou District, Chengdu',
    'tel': '13122334455',
    'emergence contact': '13800998877'
}
print(person)
```

From the above code, I believe you have already seen that using a dictionary to save a person's information is far better than using a list or tuple, because we can use the key before the `:` to indicate the meaning of the entry, and the key after the `:` is the value corresponding to the entry.

Of course, if we prefer, we can also use the built-in function `dict` or the dictionary's generative syntax to create a dictionary, as shown below.

```python
# Each set of parameters in the dict function (constructor) is a set of key-value pairs in the dictionary
person = dict(name='王大锤', age=55, height=168, weight=60, addr='Room 101, Building 1, No. 62, Kehua North Road, Wuhou District, Chengdu')
print(person) # {'name': '王大锤', 'age': 55, 'height': 168, 'weight': 60, 'addr': 'Room 101, Building 1, No. 62, Kehua North Road, Wuhou District, Chengdu'}

# You can use the Python built-in function zip to compress two sequences and create a dictionary
items1 = dict(zip('ABCDE', '12345'))
print(items1)  # {'A': '1', 'B': '2', 'C': '3', 'D': '4', 'E': '5'}
items2 = dict(zip('ABCDE', range(1, 10)))
print(items2)  # {'A': 1, 'B': 2, 'C': 3, 'D': 4, 'E': 5}

# Create a dictionary using dictionary production grammar
items3 = {x: x ** 3 for x in range(1, 6)}
print(items3)  # {1: 1, 2: 8, 3: 27, 4: 64, 5: 125}
```

If you want to know how many key-value pairs there are in the dictionary, you still use the `len` function; if you want to traverse the dictionary, you can use the `for` loop, but you need to note that the `for` loop only traverses the keys of the dictionary. But it doesn’t matter. After learning the index operation of the dictionary, we can access the corresponding value through the dictionary key.

```python
person = {
    'name': 'Wang Dachui',
    'age': 55,
    'height': 168,
    'weight': 60,
    'addr': 'Room 101, Building 1, No. 62, Kehua North Road, Wuhou District, Chengdu'
}
print(len(person))  # 5
for key in person:
    print(key)
```

### Dictionary operations

For dictionary types, member operations and index operations are definitely very important. The former can determine whether the specified key is in the dictionary, and the latter can access the corresponding value through the key or add a new key-value pair to the dictionary. It is worth noting that the index of a dictionary is different from the index of a list. Because the elements in the list have their own serial numbers, the index of the list is an integer; because the dictionary stores key-value pairs, the dictionary needs to use the key to index the corresponding value. It is necessary to **specially remind** everyone that **the keys in the dictionary must be immutable types**, such as integers (`int`), floating-point numbers (`float`), strings (`str`), tuples (`tuple`) and other types. This is the same as the requirements for elements of the set type; obviously, the lists (`list`) and sets (`set`) we talked about before cannot be used as keys in the dictionary, and the dictionary type itself can no longer be used as a key in the dictionary, because the dictionary is also a mutable type, but lists, sets, and dictionaries can all be used as values in the dictionary, for example:

```python
person = {
    'name': 'Wang Dachui',
    'age': 55,
    'height': 168,
    'weight': 60,
    'addr': ['Room 101, Building 1, No. 62, Kehua North Road, Wuhou District, Chengdu', 'No. 1, Baiwanzhuang Street, Xicheng District, Beijing'],
    'car': {
        'brand': 'BMW X7',
        'maxSpeed': '250',
        'length': 5170,
        'width': 2000,
        'height': 1835,
        'displacement': 3.0
    }
}
print(person)
```

You can take a look at the following code to understand the member operations and index operations of the dictionary.

```python
person = {'name': '王大锤', 'age': 55, 'height': 168, 'weight': 60, 'addr': 'Room 101, Building 1, No. 62, Kehua North Road, Wuhou District, Chengdu'}

# Membership Operation
print('name' in person)  # True
print('tel' in person)   # False

# Indexing
print(person['name'])
print(person['addr'])
person['age'] = 25
person['height'] = 178
person['tel'] = '13122334455'
person['signature'] = 'Your boyfriend is a piece of trash. He will ride on a colorful cloud to marry your bestie.'
print(person)

# Loop through
for key in person:
    print(f'{key}:\t{person[key]}')
```

Note that when getting a value from a dictionary through indexing, if the specified key is not in the dictionary, a KeyError exception will be raised.

### Dictionary Methods

The methods of dictionary type are basically related to the key-value pair operations of the dictionary, among which the `get` method can get the corresponding value through the key. Unlike the index operation, the `get` method will not generate an exception when there is no specified key in the dictionary, but will return `None` or the specified default value. The code is as follows.

```python
person = {'name': '王大锤', 'age': 25, 'height': 178, 'addr': 'Room 101, Building 1, No. 62, Kehua North Road, Wuhou District, Chengdu'}
print(person.get('name')) # Wang Dachui
print(person.get('sex'))        # None
print(person.get('sex', True))  # True
```

If you need to get all the keys in a dictionary, you can use the `keys` method; if you need to get all the values in a dictionary, you can use the `values` method. The dictionary also has a method called `items`, which assembles keys and values into tuples. This method is also very convenient for traversing the elements in the dictionary.

```python
person = {'name': '王大锤', 'age': 25, 'height': 178}
print(person.keys())    # dict_keys(['name', 'age', 'height'])
print(person.values())  # dict_values(['王大锤', 25, 178])
print(person.items())   # dict_items([('name', '王大锤'), ('age', 25), ('height', 178)])
for key, value in person.items():
    print(f'{key}:\t{value}')
```

The dictionary's `update` method implements the merge operation of two dictionaries. For example, there are two dictionaries `x` and `y`. When the `x.update(y)` operation is executed, the values corresponding to the same keys in `x` and `y` will be updated by the values in `y`, and the key-value pairs in `y` but not in `x` will be directly added to `x`. The code is as follows.

```python
person1 = {'name': '王大锤', 'age': 55, 'height': 178}
person2 = {'age': 25, 'addr': 'Room 101, Building 1, No. 62, Kehua North Road, Wuhou District, Chengdu'}
person1.update(person2)
print(person1) # {'name': '王大锤', 'age': 25, 'height': 178, 'addr': 'Room 101, Building 1, No. 62, Kehua North Road, Wuhou District, Chengdu'}
```

If you are using Python 3.9 and above, you can also use the `|` operator to accomplish the same operation as shown below.

```python
person1 = {'name': '王大锤', 'age': 55, 'height': 178}
person2 = {'age': 25, 'addr': 'Room 101, Building 1, No. 62, Kehua North Road, Wuhou District, Chengdu'}
person1 |= person2
print(person1) # {'name': '王大锤', 'age': 25, 'height': 178, 'addr': 'Room 101, Building 1, No. 62, Kehua North Road, Wuhou District, Chengdu'}
```

You can delete elements from a dictionary using the `pop` or `popitem` method. The former returns (gets) the value corresponding to the key, but if the specified key does not exist in the dictionary, a `KeyError` error will be raised; the latter returns (gets) a tuple consisting of the key and value when deleting an element. The `clear` method of a dictionary will clear all key-value pairs in the dictionary, as shown below.

```python
person = {'name': '王大锤', 'age': 25, 'height': 178, 'addr': 'Room 101, Building 1, No. 62, Kehua North Road, Wuhou District, Chengdu'}
print(person.pop('age'))  # 25
print(person) # {'name': '王大锤', 'height': 178, 'addr': 'Room 101, Building 1, No. 62, Kehua North Road, Wuhou District, Chengdu'}
print(person.popitem()) # ('addr', 'Room 101, Building 1, No. 62, Kehua North Road, Wuhou District, Chengdu')
print(person) # {'name': '王大锤', 'height': 178}
person.clear()
print(person)             # {}
```

Just like lists, you can also use the `del` keyword to delete elements from a dictionary. When deleting an element, if the specified key index does not match the corresponding value, a `KeyError` error will be raised. The specific steps are as follows.

```python
person = {'name': '王大锤', 'age': 25, 'height': 178, 'addr': 'Room 101, Building 1, No. 62, Kehua North Road, Wuhou District, Chengdu'}
of the character['age']
del person['addr']
print(person) # {'name': '王大锤', 'height': 178}
```

### Application of dictionary

Let's take a look at some simple examples to see how to use the dictionary type to solve some practical problems.

**Example 1**: Input a paragraph, count the number of times each English letter appears, and output them in descending order.

```python
sentence = input('Please enter a sentence: ')
counter = {}
for ch in sentence:
    if 'A' <= ch <= 'Z' or 'a' <= ch <= 'z':
        counter[ch] = counter.get(ch, 0) + 1
sorted_keys = sorted(counter, key=counter.get, reverse=True)
for key in sorted_keys:
    print(f'{key} appears {counter[key]} times.')
```

enter:

```
Man is distinguished, not only by his reason, but by this singular passion from other animals, which is a lust of the mind, that by a perseverance of delight in the continued and indefatigable generation of knowledge, exceeds the short vehemence of any carnal pleasure.
```

Output:

```
e appears 27 times.
n appears 21 times.
a appears 18 times.
i appears 18 times.
s appears 16 times.
t appears 16 times.
o appears 14 times.
h appears 13 times.
r appears 10 times.
d appears 9 times.
l appears 9 times.
g appears 6 times.
u appears 6 times.
f appears 6 times.
c appears 6 times.
y appears 5 times.
b appears 5 times.
m appears 4 times.
p appears 3 times.
w appears 2 times.
v appears 2 times.
M appears 1 time.
k appears 1 time.
x appears 1 time.
```

**Example 2**: The stock codes and prices are saved in a dictionary. Find the stocks with a price greater than 100 yuan and create a new dictionary.

> **Note**: You can use the dictionary's generative grammar to create this new dictionary.

```python
stocks = {
    'AAPL': 191.88,
    'GOOG': 1186.96,
    'IBM': 149.24,
    'ORCL': 48.44,
    'ACN': 166.89,
    'FB': 208.09,
    'SYMC': 21.29
}
stocks2 = {key: value for key, value in stocks.items() if value > 100}
print(stocks2)
```

Output:

```
{'AAPL': 191.88, 'GOOG': 1186.96, 'IBM': 149.24, 'ACN': 166.89, 'FB': 208.09}
```

### Summarize

Dictionaries in Python programs are very similar to dictionaries in real life, allowing us to **store data in the form of key-value pairs** and then **access the corresponding values through the keys**. Dictionaries are a data type that is very **convenient for data retrieval**, but we need to remind you again that **the keys in dictionaries must be immutable types**, and lists, sets, dictionaries and other types of data cannot be used as dictionary keys.