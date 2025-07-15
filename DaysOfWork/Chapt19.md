## Advanced Object-Oriented Programming

Earlier we explained some basic knowledge of Python object-oriented programming. In this section, we will continue to discuss content related to object-oriented programming.

### Visibility and property decorators

In many object-oriented programming languages, object attributes are usually set as private or protected members, which simply means that direct access to these attributes is not allowed; object methods are usually public, because public methods are messages that objects can accept and are also the calling interface exposed by objects to the outside world, which is called access visibility. In Python, you can specify the access visibility of attributes by prefixing the object attribute name with an underscore. For example, you can use `__name` to represent a private attribute and `_name` to represent a protected attribute. The code is as follows.

```python
class Student:

    def __init__(self, name, age):
        self.__name = name
        self.__age = age

    def study(self, course_name):
        print(f'{self.__name} is learning {course_name}.')


stu = Student('王大锤', 20)
stu.study('Python Programming')
print(stu.__name)  # AttributeError: 'Student' object has no attribute '__name'
```

The last line of the above code will trigger an AttributeError exception with the message: 'Student' object has no attribute '__name'. It can be seen that the attribute __name starting with __ is equivalent to private and cannot be directly accessed outside the class, but the attribute can be accessed through self.__name in the study method in the class. It should be noted that most people who use the Python language usually do not choose to make the attributes of objects private or protected when defining classes. As a famous saying goes: "We are all consenting adults here", adults can be responsible for their own behavior without restricting access visibility through the Python language itself. In fact, most programmers believe that openness is better than closedness, and privatizing the attributes of objects is not essential, so the Python language does not make the strictest semantic restrictions, that is, if you want, you can still access the private attribute __name in the above code using stu._Student__name. Interested readers can try it themselves.

### Dynamic properties

Python is a dynamic language. Wikipedia defines dynamic languages as "languages whose structure can be changed at runtime, such as new functions, objects, or even code can be introduced, existing functions can be deleted, or other structural changes can occur". Dynamic languages are very flexible. Currently popular languages such as Python and JavaScript are both dynamic languages. In addition, languages such as PHP and Ruby are also dynamic languages, while languages such as C and C++ are not dynamic languages.

In Python, we can dynamically add attributes to objects, which is a privilege of Python as a dynamically typed language. The code is as follows. It is important to remind you that object methods are essentially attributes of objects. If you send an unreceivable message to an object, the exception raised is still `AttributeError`.

```python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age


stu = Student('王大锤', 20)
stu.sex = '男' # Dynamically add sex attribute to student object
```

If you do not want to dynamically add attributes to an object when using it, you can use the `__slots__` magic in Python. For the `Student` class, you can specify `__slots__ = ('name', 'age')` in the class, so that objects of the `Student` class can only have `name` and `age` attributes. If you want to dynamically add other attributes, an exception will be raised. The code is as follows.

```python
class Student:
    __slots__ = ('name', 'age')

    def __init__(self, name, age):
        self.name = name
        self.age = age


stu = Student('王大锤', 20)
# AttributeError: 'Student' object has no attribute 'sex'
stu.sex = 'male'
```

### Static methods and class methods

Previously, all the methods we defined in a class were object methods. In other words, these methods are messages that can be received by an object. In addition to object methods, a class can also have static methods and class methods. These two types of methods are messages sent to a class, and there is no substantial difference between them. In the object-oriented world, everything is an object. Every class we define is actually an object, and static methods and class methods are messages sent to class objects. So, what kind of messages are sent directly to class objects?

For example, define a triangle class, construct a triangle by passing in the lengths of three sides, and provide methods for calculating the perimeter and area. There is no doubt that calculating the perimeter and area is a method of the triangle object. However, when creating a triangle object, the three sides passed in may not be able to construct a triangle. For this reason, we can first write a method to verify whether the given three sides can form a triangle. This method is obviously not an object method because the triangle object has not been created when this method is called. We can design this type of method as a static method or a class method, that is, this type of method is not a message sent to the triangle object, but a message sent to the triangle class. The code is as follows.

```python
class Triangle(object):
    """triangle"""

    def __init__(self, a, b, c):
        """Initialization method"""
        self.a = a
        self.b = b
        self.c = c

    @staticmethod
    def is_valid(a, b, c):
        """Judge whether the three sides can form a triangle (static method)"""
        return a + b > c and b + c > a and a + c > b

    # @classmethod
    # def is_valid(cls, a, b, c):
    # """Judge whether the three sides can form a triangle (class method)"""
    #     return a + b > c and b + c > a and a + c > b

    def perimeter(self):
        """Calculate the circumference"""
        return self.a + self.b + self.c

    def area(self):
        """Calculate area"""
        p = self.perimeter() / 2
        return (p * (p - self.a) * (p - self.b) * (p - self.c)) ** 0.5
```

The code above uses the `staticmethod` decorator to declare that the `is_valid` method is a static method of the `Triangle` class. If you want to declare a class method, you can use the `classmethod` decorator (as shown in lines 15 to 18 of the code above). You can directly use the `class name.method name` method to call static methods and class methods. The difference between the two is that the first parameter of the class method is the class object itself, while the static method does not have this parameter. To sum up briefly, **object methods, class methods, and static methods can all be called using the "class name.method name" method. The difference is whether the first parameter of the method is a normal object, a class object, or an object that does not receive the message**. Static methods can usually also be written directly as an independent function because they are not bound to a specific object.

As a supplementary explanation, we can add a `property` decorator (Python built-in type) to the above method for calculating the perimeter and area of a triangle. In this way, `perimeter` and `area` of the triangle class become two attributes. They are no longer accessed by calling methods, but are directly obtained by object access attributes. The modified code is shown below.

```python
class Triangle(object):
    """triangle"""

    def __init__(self, a, b, c):
        """Initialization method"""
        self.a = a
        self.b = b
        self.c = c

    @staticmethod
    def is_valid(a, b, c):
        """Judge whether the three sides can form a triangle (static method)"""
        return a + b > c and b + c > a and a + c > b

    @property
    def perimeter(self):
        """Calculate the circumference"""
        return self.a + self.b + self.c

    @property
    def area(self):
        """Calculate area"""
        p = self.perimeter / 2
        return (p * (p - self.a) * (p - self.b) * (p - self.c)) ** 0.5


t = Triangle(3, 4, 5)
print(f'Perimeter: {t.perimeter}')
print(f'Area: {t.area}')
```

### Inheritance and polymorphism

Object-oriented programming languages support the creation of new classes based on existing classes, thereby reducing the writing of repeated code. The class that provides inheritance information is called the parent class (superclass, base class), and the class that obtains inheritance information is called the child class (derived class, derived class). For example, if we define a student class and a teacher class, we will find that they have a lot of repeated code, and these repeated codes are the common attributes and behaviors of teachers and students as people, so in this case, we should first define humans, and then derive teacher and student classes from humans through inheritance. The code is shown below.

```python
class Person:
    """people"""

    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def eat(self):
        print(f'{self.name} is eating.')
    
    def sleep(self):
        print(f'{self.name} is sleeping.')


class Student(Person):
    """student"""
    
    def __init__(self, name, age):
        super().__init__(name, age)
    
    def study(self, course_name):
        print(f'{self.name} is learning {course_name}.')


class Teacher(Person):
    """teacher"""

    def __init__(self, name, age, title):
        super().__init__(name, age)
        self.title = title
    
    def teach(self, course_name):
        print(f'{self.name}{self.title} is teaching {course_name}.')



stu1 = Student('Bai Yuanfang', 21)
stu2 = Student('Di Renjie', 22)
tea1 = Teacher('Wu Zetian', 35, 'Associate Professor')
stu1.eat()
stu2.sleep()
tea1.eat()
stu1.study('Python Programming')
tea1.teach('Python Programming')
stu2.study('Introduction to Data Science')
```

The syntax of inheritance is to specify the parent class of the current class in parentheses after the class name when defining a class. If you do not specify who its parent class is when defining a class, the default parent class is the `object` class. The `object` class is the top-level class in Python, which means that all classes are its subclasses, either directly or indirectly inheriting it. The Python language allows multiple inheritance, which means that a class can have one or more parent classes. We will discuss the issue of multiple inheritance in more detail later. In the initialization method of the subclass, we can call the parent class initialization method through `super().__init__()`. The `super` function is a built-in function in Python specifically designed to obtain the parent class object of the current object. As can be seen from the above code, in addition to inheriting the attributes and methods provided by the parent class, the subclass can also define its own unique attributes and methods, so the subclass has more capabilities than the parent class. In actual development, we often replace a parent class object with a subclass object. This is a common behavior in object-oriented programming, also known as the "Liskov Substitution Principle".

After a subclass inherits a method from its parent class, it can also rewrite the method (re-implement the method). Different subclasses can give different implementations to the same method of the parent class. Such methods will exhibit polymorphic behavior when the program is running (calling the same method and doing different things). Polymorphism is the most essential part of object-oriented programming, and of course it is also the most difficult part for beginners to understand and use flexibly. We will use a special example to explain this knowledge point in the next chapter.

### Summarize

Python is a dynamically typed language. Objects in Python can dynamically add attributes. Object methods are actually attributes, but the corresponding attribute is a callable function. In the object-oriented world, **everything is an object**. The classes we define are also objects, so **classes can also receive messages**, and the corresponding methods are class methods or static methods. Through inheritance, we can **create new classes from existing classes** to reuse existing class codes.