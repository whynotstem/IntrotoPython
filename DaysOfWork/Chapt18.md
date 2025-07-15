## Introduction to Object-Oriented Programming

Object-oriented programming is a very popular programming paradigm. The so-called programming paradigm is the methodology of program design. Simply put, it is the programmer's cognition and understanding of the program and the way they write code.

In the previous course, we said that "**a program is a collection of instructions**". When running a program, the statements in the program will become one or more instructions, and then be executed by the CPU (central processing unit). In order to simplify the design of the program, we talked about functions again. **Put relatively independent and frequently reused code into a function**, and call the function when you need to use this code. If the function of a function is too complex and bloated, we can further **split the function into multiple sub-functions** to reduce the complexity of the system.

I wonder if you have noticed that programming is actually the process of the programmer controlling the machine to complete tasks through code according to the working method of the computer. However, the working method of the computer is different from the normal thinking mode of human beings. If you want to program, you must abandon the normal thinking mode of human beings to cater to the computer, and the fun of programming will be much less. Here, what I want to say is not that we cannot write code according to the working method of the computer, but when we need to develop a complex system, this method will make the code too complicated, making the development and maintenance work difficult.

As software becomes more complex, writing correct and reliable code becomes an extremely difficult task, which is why many people firmly believe that "software development is the most complex activity of all human activities to transform the world." How to use programs to describe complex systems and solve complex problems has become a problem that all programmers must think about and face. The Smalltalk language, which was born in the 1970s, gave software developers hope because it introduced a new programming paradigm called object-oriented programming. In the world of object-oriented programming, the data and the functions that operate the data in the program are a logical whole, which we call objects. Objects can receive messages, and the way to solve problems is to create objects and send various messages to them; through message passing, multiple objects in the program can work together, so that complex systems can be constructed and real-world problems can be solved. Of course, the prototype of object-oriented programming can be traced back to the earlier Simula language, but this is not the focus of our discussion.

> **Note:** Many high-level programming languages we use today support object-oriented programming, but object-oriented programming is not a "silver bullet" to solve all problems in software development, or there is no so-called "silver bullet" in the software development industry. Regarding this issue, you can refer to the paper "No Silver Bullet: The Essential and Ancillary Work of Software Engineering" published by Frederick Brooks, the father of the IBM 360 system, or the classic book of software engineering, "The Mythical Man-Month".

### Classes and Objects

If I were to summarize object-oriented programming in one sentence, I think the following statement is quite insightful and accurate.

> **Object-oriented programming**: combine a set of data and methods for processing data into **objects**, group objects with the same behavior into **classes**, hide the internal details of objects through **encapsulation**, implement class specialization and generalization through **inheritance**, and implement dynamic dispatch based on object type through **polymorphism**.

This sentence may not be easy for beginners to understand, but I can circle a few key words for you: **object**, **class**, **encapsulation**, **inheritance**, **polymorphism**.

Let's first talk about the words class and object. In object-oriented programming, class is an abstract concept and object is a concrete concept. We extract the common features of objects of the same type to form a class. For example, human beings, which we often talk about, are an abstract concept, and each of us is a real existence under this abstract concept of human beings, that is, an object. In short, a class is the blueprint and template of an object, and an object is an instance of a class, an entity that can receive messages.

In the world of object-oriented programming, **everything is an object**, **objects have attributes and behaviors**, **each object is unique**, and **objects must belong to a class**. The attributes of an object are the static characteristics of the object, and the behavior of an object is the dynamic characteristics of the object. According to the above statement, if we extract the attributes and behaviors of objects with common characteristics, we can define a class.

<img src="res/day18/20210731182741.png" width="75%">

### Define the class

In Python, we can use the `class` keyword plus the class name to define a class. By indenting, we can determine the class code block, just like defining a function. In the class code block, we need to write some functions. We have said that a class is an abstract concept, so these functions are our extraction of the common dynamic features of a class of objects. The functions written in the class are usually called **methods**. Methods are the behavior of objects, that is, messages that objects can receive. The first parameter of a method is usually `self`, which represents the object itself that receives the message.

```python
class Student:

    def study(self, course_name):
        print(f'The student is studying {course_name}.')

    def play(self):
        print(f'Students are playing games.')
```

### Creating and using objects

After we define a class, we can use the constructor syntax to create an object. The code is as follows.

```python
stu1 = Student()
stu2 = Student()
print(stu1)    # <__main__.Student object at 0x10ad5ac50>
print(stu2)    # <__main__.Student object at 0x10ad5acd0> 
print(hex(id(stu1)), hex(id(stu2)))    # 0x10ad5ac50 0x10ad5acd0
```

The name of the class followed by parentheses is the so-called constructor syntax. The code above creates two student objects, one assigned to the variable `stu1` and the other assigned to the variable `stu2`. When we use the `print` function to print the two variables `stu1` and `stu2`, we will see the address of the object in memory (in hexadecimal form) output, which is the same as the value we get when we use the `id` function to view the object identifier. Now we can tell you that the variable we defined actually stores the logical address (position) of an object in memory. Through this logical address, we can find the object in memory. Therefore, the assignment statement `stu3 = stu2` does not create a new object, but only uses a new variable to store the address of the existing object.

Next, we try to send a message to an object, that is, call an object's method. In the `Student` class just now, we defined two methods, `study` and `play`. The first parameter `self` of the two methods represents the student object receiving the message, and the second parameter of the `study` method is the name of the course to be studied. In Python, there are two ways to send messages to objects. Please see the code below.

```python
# Calling methods via "class.method"
# The first parameter is the object that receives the message
# The second parameter is the name of the course to be studied
Student.study(stu1, 'Python Programming') # The student is studying Python programming.
# Calling methods via "object.method"
# The object in front of the dot is the object that receives the message
# Only need to pass in the second parameter course name
stu1.study('Python Programming') # The student is learning Python Programming.

Student.play(stu2) # The student is playing a game.
stu2.play() # The student is playing a game.
```

### Initialization method

You may have noticed that the student object we just created only has behaviors but no attributes. If we want to define attributes for the student object, we can modify the `Student` class and add a method called `__init__` to it. When we call the `Student` class constructor to create an object, we first obtain the memory space required to save the student object in the memory, and then automatically execute the `__init__` method to complete the initialization operation of the memory, that is, put the data into the memory space. Therefore, we can specify attributes for the student object by adding the `__init__` method to the `Student` class, and at the same time complete the operation of assigning initial values to the attributes. For this reason, the `__init__` method is often also called the initialization method.

We slightly modify the `Student` class above and add two attributes, `name` (name) and `age` (age), to the student object.

```python
class Student:
    """student"""

    def __init__(self, name, age):
        """Initialization method"""
        self.name = name
        self.age = age

    def study(self, course_name):
        """study"""
        print(f'{self.name} is learning {course_name}.')

    def play(self):
        """play"""
        print(f'{self.name} is playing a game.')
```

Modify the code that just created the object and sent messages to the object, execute it again, and see if there are any changes in the execution results of the program.

```python
# Call the constructor of the Student class to create an object and pass in initialization parameters
stu1 = Student('Luo Hao', 44)
stu2 = Student('王大锤', 25)
stu1.study('Python Programming') # Luo Hao is learning Python programming.
stu2.play() # Wang Dachui is playing a game.
```


### The Pillars of Object Orientation

Object-oriented programming has three pillars, which are the three words we circled when we highlighted the key points: **encapsulation**, **inheritance**, and **polymorphism**. The latter two concepts will be explained in detail in the next class. Here we first talk about what encapsulation is. My own understanding of encapsulation is: **hide all implementation details that can be hidden, and only expose a simple calling interface to the outside world**. The object method we define in the class is actually a kind of encapsulation. This encapsulation allows us to execute the code in the method by sending a message to the object after creating the object. In other words, we only know the name and parameters of the method (the external view of the method) without knowing the internal implementation details of the method (the internal view of the method).

For example, if you want to control a robot to help me pour a glass of water, if you don't use object-oriented programming and don't do any encapsulation, you need to send a series of instructions to the robot, such as stand up, turn left, walk forward 5 steps, pick up the water cup in front of you, turn back, walk forward 10 steps, bend down, put down the water cup, press the water button, wait 10 seconds, release the water button, pick up the water cup, turn right, walk forward 5 steps, put down the water cup, etc., to complete this simple operation, which is troublesome to think about. According to the idea of object-oriented programming, we can encapsulate the pouring operation into a method of the robot. When we need the robot to help us pour water, we only need to send a pouring message to the robot object. Isn't it better to do this?

In many scenarios, object-oriented programming is actually a three-step problem. The first step is to define the class, the second step is to create the object, and the third step is to send a message to the object. Of course, sometimes we don't need the first step, because the class we want to use may already exist. As we said before, Python's built-in `list`, `set`, and `dict` are actually classes. If we want to create list, set, and dictionary objects, we don't need to customize the class. Of course, some classes are not directly provided in the Python standard library. They may come from third-party code. How to install and use third-party code will be discussed in subsequent courses. In some special scenarios, we will use objects called "built-in objects". The so-called "built-in objects" means that the first and second steps of the above three steps are not needed, because the class already exists and the object has been created. Just send a message to the object directly. This is what we often call "out of the box".

### Object-Oriented Example

#### Example 1: Clock

> **Requirement**: Define a class to describe a digital clock, providing the functions of moving numbers and displaying time.

```python
import time


# Define the clock class
class Clock:
    """Digital clock"""

    def __init__(self, hour=0, minute=0, second=0):
        """Initialization method
        :param hour: hour
        :param minute: Minute
        :param second: Second
        """
        self.hour = hour
        self.min = minute
        self.sec = second

    def run(self):
        """Walk"""
        self.sec += 1
        if self.sec == 60:
            self.sec = 0
            self.min += 1
            if self.min == 60:
                self.min = 0
                self.hour += 1
                if self.hour == 24:
                    self.hour = 0

    def show(self):
        """Show time"""
        return f'{self.hour:0>2d}:{self.min:0>2d}:{self.sec:0>2d}'


# Create a clock object
clock = Clock(23, 59, 58)
while True:
    # Send a message to the clock object to read the time
    print(clock.show())
    # Sleep for 1 second
    time.sleep(1)
    # Send a message to the clock object to make it move
    clock.run()
```

#### Example 2: Points on a plane

> **Requirements**: Define a class to describe a point on a plane and provide a method to calculate the distance to another point.

```python
class Point:
    """Points on a plane"""

    def __init__(self, x=0, y=0):
        """Initialization method
        :param x: horizontal axis
        :param y: vertical coordinate
        """
        self.x, self.y = x, y

    def distance_to(self, other):
        """Calculate the distance to another point
        :param other: another point
        """
        dx = self.x - other.x
        dy = self.y - other.y
        return (dx * dx + dy * dy) ** 0.5

    def __str__(self):
        return f'({self.x}, {self.y})'


p1 = Point(3, 5)
p2 = Point(6, 9)
print(p1) # Call the object's __str__ magic method
print(p2)
print(p1.distance_to(p2))
```

### Summarize

Object-oriented programming is a very popular programming paradigm. In addition, there are other programming paradigms such as **imperative programming** and **functional programming**. Since the real world is made up of objects, and objects are entities that can receive messages, **object-oriented programming is more in line with normal human thinking habits**. Classes are abstract, and objects are concrete. With classes, objects can be created, and with objects, messages can be received. This is the basis of object-oriented programming. The process of defining a class is an abstract process. Finding the common attributes of objects belongs to data abstraction, and finding the common methods of objects belongs to behavioral abstraction. The process of abstraction is a process that is different for different people. Abstracting the same class of objects may produce different results, as shown in the figure below.

<img src="res/day18/20210731182914.png" width="75%">

> **Note:** The illustrations in this lesson are from the book "Object-Oriented Analysis and Design" written by Grady Booc et al. This book is a classic work on object-oriented programming. Interested readers can buy and read this book to learn more about object-oriented knowledge.
