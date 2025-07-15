## Collection of commonly used data structures

After learning about lists and tuples, let's learn about a container-type data type called a set. You must be familiar with the word set, as it is a concept in math textbooks. If we treat a certain range of definite and distinguishable things as a whole, then this whole is a set, and each thing in the set is called an element of the set. Usually, a set needs to meet the following requirements:

1. **Disorder**: In a set, the status of each element is the same, and the elements are disordered.
2. **Mutual Distinction**: In a set, any two elements are different, that is, an element can only appear once in a set.
3. **Determinism**: Given a set and an arbitrary element, the element either belongs to the set or does not belong to the set, one of the two must be true, and no ambiguity is allowed.

There is no essential difference between sets in Python programs and sets in mathematics. What needs to be emphasized is the disorder and heterogeneity mentioned above. Disorder means that the elements in a set do not have a certain order like the elements in a column. Any element can be accessed through index operations. **Sets do not support index operations**. In addition, the heterogeneity of sets determines that **there cannot be duplicate elements in a set**. This is also what distinguishes sets from lists. We cannot add duplicate elements to a set. Set types must support `in` and `not in` member operations, so that we can determine whether an element belongs to a set, which is the determinism of the set mentioned above. **The member operation of a set is better than the member operation of a list in performance**. This is determined by the underlying storage characteristics of the set. We will not discuss it here for the time being. Just remember this conclusion.

> **Note**: The underlying layer of the collection uses hash storage (hash storage). Readers who are not familiar with hash storage can first read the explanation of [hash table](https://www.hello-algo.com/chapter_hashing/) on the "Hello Algorithm" website. Thanks to the author for his open source spirit.

### Create a collection

In Python, you can use the `{}` literal syntax to create a set. `{}` needs to have at least one element, because `{}` without elements is not an empty set but an empty dictionary. We will introduce the dictionary type in the next lesson. Of course, you can also use the Python built-in function `set` to create a set. To be precise, `set` is not a function, but a constructor that creates a set object. This knowledge point will be introduced later when we explain object-oriented programming. We can use the `set` function to create an empty set, or we can use it to convert other sequences into sets. For example: `set('hello')` will get a set containing `4` characters (the repeated character `l` will only appear once in the set). In addition to these two methods, you can also use generative syntax to create sets, just like we used generative syntax to create lists before.

```python
set1 = {1, 2, 3, 3, 3, 2}
print(set1)

set2 = {'banana', 'pitaya', 'apple', 'apple', 'banana', 'grape'}
print(set2)

set3 = set('hello')
print(set3)

set4 = set([1, 2, 2, 3, 3, 3, 2, 1])
print(set4)

set5 = {num for num in range(1, 20) if num % 3 == 0 or num % 7 == 0}
print(set5)
```

It is necessary to remind everyone that the elements in the collection must be of `hashable` type. The so-called `hashable` type refers to the data type that can calculate the hash code. Usually, immutable types are `hashable` types, such as integers (`int`), floating-point decimals (`float`), Boolean values (`bool`), strings (`str`), tuples (`tuple`), etc. Mutable types are not `hashable` types, because mutable types cannot calculate a definite hash code, so they cannot be put into a collection. For example: we cannot use a list as an element in a collection; similarly, since the collection itself is also a mutable type, the collection cannot be used as an element in a collection. We can create nested lists (the elements of a list are also lists), but we cannot create nested collections. This must be paid attention to when using collections.

> **Warm Tip**: If you don’t understand the concepts of hash code and hash storage mentioned above, you can put it aside for now, because it does not affect your continued learning and use of Python. Of course, if you are a computer science major, it is difficult to be forgiven for not understanding hash storage, so you should hurry up and make up for it.

### Element traversal

We can use the `len` function to get the number of elements in a collection, but we cannot use index operations to traverse the elements in a collection because the elements in a collection do not have a specific order. Of course, to traverse the elements of a collection, we can still use the `for-in` loop, as shown below.

```python
set1 = {'Python', 'C++', 'Java', 'Kotlin', 'Swift'}
for elem in set1:
    print(item)
```

> **Tip**: Take a look at the results of the above code and experience the disorder of the set through the order of word output.

### Set operations

Python provides a very rich set of operations, including membership operations, intersection operations, union operations, difference operations, comparison operations (equality, subsets, supersets), etc.

#### Member operations

You can use the member operations `in` and `not in` to check whether an element is in a set. The code is as follows.

```python
set1 = {11, 12, 13, 14, 15}
print(10 in set1)      # False 
print(15 in set1)      # True
set2 = {'Python', 'Java', 'C++', 'Swift'}
print('Ruby' in set2)  # False
print('Java' in set2)  # True
```

#### Binary operations

Binary operations on sets mainly refer to operations such as intersection, union, difference, and symmetric difference of sets. These operations can be implemented through operators or through methods of set types. The code is shown below.

<img src="res/day12/set_operations.png" style="zoom:50%;">

```python
set1 = {1, 2, 3, 4, 5, 6, 7}
set2 = {2, 4, 6, 8, 10}

# Intersection
print(set1 & set2)                      # {2, 4, 6}
print(set1.intersection(set2))          # {2, 4, 6}

# Union
print(set1 | set2)                      # {1, 2, 3, 4, 5, 6, 7, 8, 10}
print(set1.union(set2))                 # {1, 2, 3, 4, 5, 6, 7, 8, 10}

# Collection
print(set1 - set2)                      # {1, 3, 5, 7}
print(set1.difference(set2))            # {1, 3, 5, 7}

# Symmetric difference
print(set1 ^ set2)                      # {1, 3, 5, 7, 8, 10}
print(set1.symmetric_difference(set2))  # {1, 3, 5, 7, 8, 10}
```

From the above code, we can see that for finding the intersection of two sets, the `&` operator and the `intersection` method have exactly the same effect. Using the operator is obviously more intuitive and the code is shorter. It should be noted that the binary operation of sets can also be combined with the assignment operation to form a compound assignment operation. For example: `set1 |= set2` is equivalent to `set1 = set1 | set2`, and the method with the same effect as `|=` is `update`; `set1 &= set2` is equivalent to `set1 = set1 & set2`, and the method with the same effect as `&=` is `intersection_update`, as shown in the following code.

```python
set1 = {1, 3, 5, 7}
set2 = {2, 4, 6}
set1 |= set2
# set1.update(set2)
print(set1)  # {1, 2, 3, 4, 5, 6, 7}
set3 = {3, 6, 9}
set1 &= set3
# set1.intersection_update(set3)
print(set1)  # {3, 6}
set2 -= set1
# set2.difference_update(set1)
print(set2)  # {2, 4}
```

#### Comparison operations

Two sets can be compared for equality using `==` and `!=`. If the elements in the two sets are exactly the same, then the result of the `==` comparison is `True`, otherwise it is `False`. If any element of set `A` is an element of set `B`, then set `A` is called a subset of set `B`, that is, for $\small{\forall{a} \in {A}}$, there is $\small{{a} \in {B}}$, then $\small{{A} \subseteq {B}}$, `A` is a subset of `B`, and conversely, `B` can be called a superset of `A`. If `A` is a subset of `B` and `A` is not equal to `B`, then `A` is a proper subset of `B`. Python provides operators for judging subsets and supersets for set types, which are actually the operators `<`, `<=`, `>`, and `>=` that we are very familiar with. Of course, we can also use the set type methods `issubset` and `issuperset` to determine the relationship between sets. The code is as follows.

```python
set1 = {1, 3, 5}
set2 = {1, 2, 3, 4, 5}
set3 = {5, 4, 3, 2, 1}

print(set1 < set2)   # True
print(set1 <= set2)  # True
print(set2 < set3)   # False
print(set2 <= set3)  # True
print(set2 > set1)   # True
print(set2 == set3)  # True

print(set1.issubset(set2))    # True
print(set2.issuperset(set1))  # True
```

> **Explanation**: In the above code, `set1 < set2` determines whether `set1` is a proper subset of `set2`, `set1 <= set2` determines whether `set1` is a subset of `set2`, and `set2 > set1` determines whether `set2` is a superset of `set1`. Of course, we can also use `set1.issubset(set2)` to determine whether `set1` is a subset of `set2`; and `set2.issuperset(set1)` to determine whether `set2` is a superset of `set1`.

### Collection Methods

We just said that sets in Python are mutable types, and we can add elements to or delete elements from sets through set methods.

```python
set1 = {1, 10, 100}

# Add elements
set1.add(1000)
set1.add(10000)
print(set1)  # {1, 100, 1000, 10, 10000}

# Deleting elements
set1.discard(10)
if 100 in set1:
    set1.remove(100)
print(set1)  # {1, 1000, 10000}

# Clear elements
set1.clear()
print(set1)  # set()
```

> **Note**: The `remove` method for deleting an element will cause a `KeyError` error if the element does not exist, so in the above code we first determine whether the element is in the set through member operations. The set type also has a `pop` method that can randomly delete an element from the set. This method will return (obtain) the deleted element while deleting the element, while the `remove` and `discard` methods only delete the element and will not return (obtain) the deleted element.

The collection type also has a method called `isdisjoint` that can determine whether two collections have the same elements. If there are no identical elements, the method returns `True`, otherwise the method returns `False`. The code is as follows.

```python
set1 = {'Java', 'Python', 'C++', 'Kotlin'}
set2 = {'Kotlin', 'Swift', 'Java', 'Dart'}
set3 = {'HTML', 'CSS', 'JavaScript'}
print(set1.isdisjoint(set2))  # False
print(set1.isdisjoint(set3))  # True
```

### Immutable Collections

There is another immutable type of collection in Python, called `frozenset`. The difference between `set` and `frozenset` is like the difference between `list` and `tuple`. `frozenset` is an immutable type and can calculate the hash code, so it can be used as an element in `set`. Except that elements cannot be added or deleted, `frozenset` is the same as `set` in other aspects. The following code simply shows the usage of `frozenset`.

```python
fset1 = frozenset({1, 3, 5, 7})
fset2 = frozenset(range(1, 6))
print(fset1)          # frozenset({1, 3, 5, 7})
print(fset2)          # frozenset({1, 2, 3, 4, 5})
print(fset1 & fset2)  # frozenset({1, 3, 5})
print(fset1 | fset2)  # frozenset({1, 2, 3, 4, 5, 7})
print(fset1 - fset2)  # frozenset({7})
print(fset1 < fset2) # False
```

### Summarize

The **set type in Python is an unordered container**, **no repeated operations are allowed**. Since the underlying hash storage is used, the elements in the set must be `hashable`. The biggest difference between a set and a list is that **the elements in the set have no order**, so **elements cannot be accessed through index operations**, but the set can perform binary operations such as intersection, union, and difference, and can also use relational operators to check whether two sets have superset, subset, and other relationships.