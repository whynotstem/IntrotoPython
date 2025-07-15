## Object-Oriented Programming Application

Object-oriented programming is not difficult for beginners to understand but difficult to apply. Although we have summarized the three-step method of object-oriented programming (defining classes, creating objects, and sending messages to objects), it is easier said than done. **A lot of programming practice** and **reading high-quality code** may be the two things that can help you the most at this stage. Next, we will analyze the knowledge of object-oriented programming through classic cases, and also connect the Python knowledge we have learned before through these cases.

### Example 1: Poker game.

> **Description**: For simplicity, our poker deck has only 52 cards (without jokers). The game requires dealing 52 cards to 4 players. Each player has 13 cards, arranged in the order of spades, hearts, clubs, and diamonds, from small to large. No other functions are implemented for the time being.

Using object-oriented programming methods, we first need to find objects from the requirements of the problem and abstract the corresponding classes. In addition, we need to find the properties and behaviors of the objects. Of course, this is not particularly difficult. We can find nouns and verbs from the description of the requirements. Nouns are usually objects or the properties of objects, while verbs are usually the behaviors of objects. There should be at least three types of objects in the poker game, namely cards, poker and players. The three classes of cards, poker and players are not isolated. The relationship between classes can be roughly divided into **is-a relationship (inheritance)**, **has-a relationship (association)** and **use-a relationship (dependency)**. Obviously, poker and cards are has-a relationships, because a deck of poker has (has-a) 52 cards; there is not only an association relationship but also a dependency relationship between players and cards, because the player has (has-a) cards in his hand and the player uses (use-a) cards.

The attributes of cards are obvious, including suit and points. We can use four numbers from 0 to 3 to represent four different suits, but the readability of such code will be very poor, because we do not know the correspondence between spades, hearts, clubs, diamonds and numbers from 0 to 3. If a variable has only a finite number of options, we can use enumeration. Unlike languages such as C and Java, Python does not have a keyword for declaring enumeration types, but you can create enumeration types by inheriting the `Enum` class of the `enum` module, as shown below.

```python
from enum import Enum


class Suite(Enum):
    """Color (enumeration)"""
    SPADE, HEART, CLUB, DIAMOND = range(4)
```

From the above code, we can see that defining an enumeration type is actually defining symbolic constants, such as `SPADE`, `HEART`, etc. Each symbolic constant has a corresponding value, so to represent spades, you don't need to use the number 0, but `Suite.SPADE`; similarly, to represent diamonds, you don't need to use the number 3, but `Suite.DIAMOND`. Note that using symbolic constants is definitely better than using literal constants, because if you can read English, you can understand the meaning of symbolic constants, and the readability of the code will be greatly improved. The enumeration type in Python is an iterable type. Simply put, you can put the enumeration type in a `for-in` loop, and take out each symbolic constant and its corresponding value in turn, as shown below.

```python
for suite in Suite:
    print(f'{suite}: {suite.value}')
```

Next we can define the card classes.

```python
class Card:
    """Card"""

    def __init__(self, suite, face):
        self.suite = suite
        self.face = face

    def __repr__(self):
        suites = '♠♥♣♦'
        faces = ['', 'A', '2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K']
        return f'{suites[self.suite.value]}{faces[self.face]}' # Returns the suit and points of the card
```

You can test the `Card` class with the following code.

```python
card1 = Card(Suite.SPADE, 5)
card2 = Card(Suite.HEART, 13)
print(card1)  # ♠5 
print(card2)  # ♥K
```

Next we define the Poker class.

```python
import random


class Poker:
    """poker"""

    def __init__(self):
        self.cards = [Card(suite, face) 
                      for suite in Suite
                      for face in range(1, 14)] # List of 52 cards
        self.current = 0 # Record the properties of the dealing position

    def shuffle(self):
        """Shuffle"""
        self.current = 0
        random.shuffle(self.cards) # Randomly shuffle the cards using the shuffle function of the random module

    def deal(self):
        """Dealing licenses"""
        card = self.cards[self.current]
        self.current += 1
        return card

    @property
    def has_next(self):
        """Are there any more cards to be dealt?"""
        return self.current < len(self.cards)
```

You can test the Poker class using the following code.

```python
poker = Poker()
print(poker.cards) # Cards before shuffling
poker.shuffle()
print(poker.cards) # shuffled cards
```

Define the player class.

```python
class Player:
    """Player"""

    def __init__(self, name):
        self.name = name
        self.cards = [] # Cards in the player's hand

    def get_one(self, card):
        """Draw a card"""
        self.cards.append(card)

    def arrange(self):
        """Arrange the cards in your hand"""
        self.cards.sort()
```

Four players are created and the cards are dealt to the players' hands.

```python
poker = Poker()
poker.shuffle()
players = [Player('Eastern Evil'), Player('Western Poison'), Player('Southern Emperor'), Player('Northern Beggar')]
# Deal the cards to each player in turn, 13 cards per person
for _ in range(13):
    for player in players:
        player.get_one(poker.deal())
# The player sorts out the cards in his hand and outputs his name and hand
for player in players:
    player.arrange()
    print(f'{player.name}: ', end='')
    print(player.cards)
```

Executing the above code will cause an exception at `player.arrange()`, because the `arrange` method of `Player` uses the `sort` of the list to sort the cards in the player's hand. Sorting requires comparing the sizes of two `Card` objects, and the `<` operator cannot directly act on the `Card` type, so a `TypeError` exception occurs, and the exception message is: `'<' not supported between instances of 'Card' and 'Card'`.

To solve this problem, we can slightly modify the code of the `Card` class so that two `Card` objects can be directly compared in size using `<`. The technology used here is called **operator overloading**. To implement overloading of the `<` operator in Python, you need to add a magic method called `__lt__` to the class. Obviously, the `lt` in the magic method `__lt__` is the abbreviation of the English word "less than", and so on, the magic method `__gt__` corresponds to the `>` operator, the magic method `__le__` corresponds to the `<=` operator, `__ge__` corresponds to the `>=` operator, `__eq__` corresponds to the `==` operator, and `__ne__` corresponds to the `!=` operator.

The modified `Card` class code is as follows.

```python
class Card:
    """Card"""

    def __init__(self, suite, face):
        self.suite = suite
        self.face = face

    def __repr__(self):
        suites = '♠♥♣♦'
        faces = ['', 'A', '2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K']
        return f'{suites[self.suite.value]}{faces[self.face]}'
    
    def __lt__(self, other):
        if self.suite == other.suite:
            return self.face < other.face # Compare the points of the same color
        return self.suite.value < other.suite.value # Compare the values corresponding to different suits
```

>**Description:** You can try to write a simple poker game based on the above code, such as Black Jack. You can find the rules of the game on the Internet.

### Example 2: Salary settlement system.

> **Requirement**: A company has three types of employees, namely department managers, programmers and salesmen. A salary settlement system needs to be designed to calculate the monthly salary of employees based on the employee information provided. Among them, the monthly salary of the department manager is fixed at 15,000 yuan; the monthly salary of the programmer is paid according to the working time (in hours), 200 yuan per hour; the monthly salary of the salesman consists of a base salary of 1,800 yuan plus a commission of 5% of the sales volume.

Through the analysis of the above requirements, we can see that department managers, programmers, and salesmen are all employees, with the same attributes and behaviors, so we can first design a parent class named `Employee`, and then derive three subclasses of department managers, programmers, and salesmen from this parent class through inheritance. Obviously, the subsequent code will not create objects of the `Employee` class, because we need specific employee objects, so this class can be designed as an abstract class specifically for inheritance. There is no keyword for defining abstract classes in the Python language, but abstract classes can be defined through a metaclass named `ABCMeta` in the `abc` module. The concept of metaclasses will not be explained here, of course, you don't have to worry about it, just do it.

```python
from abc import ABCMeta, abstractmethod


class Employee(metaclass=ABCMeta):
    """staff"""

    def __init__(self, name):
        self.name = name

    @abstractmethod
    def get_salary(self):
        """Settlement of monthly salary"""
        pass
```

In the employee class above, there is a method called `get_salary` for settling monthly salary, but since it has not yet been determined which type of employee it is, there is no way to implement this method, even though it is a common behavior for employees. For methods that cannot be implemented temporarily, we can use the `abstractmethod` decorator to declare them as abstract methods. The so-called **abstract method is a method that is only declared but not implemented**, and **declaring this method is to allow subclasses to override this method**. The following code shows how to derive three subclasses of department manager, programmer, and salesperson from the employee class, and how the subclasses override the abstract method of the parent class.

```python
class Manager(Employee):
    """Department Manager"""

    def get_salary(self):
        return 15000.0


class Programmer(Employee):
    """programmer"""

    def __init__(self, name, working_hour=0):
        super().__init__(name)
        self.working_hour = working_hour

    def get_salary(self):
        return 200 * self.working_hour


class Salesman(Employee):
    """Salesman"""

    def __init__(self, name, sales=0):
        super().__init__(name)
        self.sales = sales

    def get_salary(self):
        return 1800 + self.sales * 0.05
```

The three classes above, `Manager`, `Programmer`, and `Salesman`, all inherit from `Employee`, and all three classes override the `get_salary` method. **Overriding means that the subclass reimplements the existing method of the parent class**. I believe you have noticed that the `get_salary` in the three subclasses is different, so this method will produce **polymorphic behavior** when the program is running. Polymorphism simply means **calling the same method**, **different subclass objects do different things**.

We use the following code to complete this salary settlement system. Since programmers and salesmen need to enter the working hours and sales of this month respectively, we use Python's built-in `isinstance` function to determine the type of employee objects in the following code. The `type` function we talked about before can also identify the type of an object, but the `isinstance` function is more powerful because it can determine whether an object is a subtype under a certain inheritance structure. You can simply understand that the `type` function is an accurate match of the object type, while the `isinstance` function is a fuzzy match of the object type.

```python
emps = [Manager('Liu Bei'), Programmer('Zhuge Liang'), Manager('Cao Cao'), Programmer('Xun Yu'), Salesman('Zhang Liao')]
for emp in emps:
    if isinstance(emp, Programmer):
        emp.working_hour = int(input(f'Please enter {emp.name} working hours this month: '))
    elif isinstance(emp, Salesman):
        emp.sales = float(input(f'Please enter {emp.name} sales this month: '))
    print(f'{emp.name}'s salary this month is: ￥{emp.get_salary():.2f} yuan')
```

### Summarize

The idea of object-oriented programming is very good and conforms to the normal thinking habits of human beings. However, in order to flexibly apply the abstraction, encapsulation, inheritance, and polymorphism in object-oriented programming, it requires a long period of accumulation and precipitation. This cannot be achieved overnight, because the accumulation of knowledge is a process of dripping into a river.2