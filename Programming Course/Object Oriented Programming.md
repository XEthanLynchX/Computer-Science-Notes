# The Four Pillars of Object-Oriented Programming

Object-Oriented Programming (OOP) is built on four fundamental principles that help create maintainable, scalable, and organized code. These principles are: **Encapsulation**, **Abstraction**, **Inheritance**, and **Polymorphism**.

---

## 1. Encapsulation

**Encapsulation** is the practice of bundling data (attributes) and methods (functions) that operate on that data within a single unit or class, while restricting direct access to some of the object's components.

### Key Concepts

- **Data Hiding**: Internal object details are hidden from the outside world
- **Access Modifiers**: Use of public, private, and protected keywords to control access
- **Getters and Setters**: Controlled access to private data through methods

### Benefits

- **Security**: Protects data from unauthorized access and modification
- **Flexibility**: Internal implementation can be changed without affecting external code
- **Maintainability**: Reduces complexity by hiding implementation details

### Example (Python)

```python
class BankAccount:
    def __init__(self, account_number, balance):
        self.__account_number = account_number  # Private attribute
        self.__balance = balance  # Private attribute
    
    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
            return True
        return False
    
    def get_balance(self):  # Getter method
        return self.__balance
    
    def withdraw(self, amount):
        if 0 < amount <= self.__balance:
            self.__balance -= amount
            return True
        return False

# Usage
account = BankAccount("123456", 1000)
account.deposit(500)
print(account.get_balance())  # 1500
# print(account.__balance)  # This would raise an error
```

---

## 2. Abstraction

**Abstraction** is the concept of hiding complex implementation details and showing only the necessary features of an object. It focuses on what an object does rather than how it does it.

### Key Concepts

- **Abstract Classes**: Classes that cannot be instantiated and serve as blueprints
- **Interfaces**: Contracts that define what methods a class must implement
- **Implementation Hiding**: Complex logic is hidden behind simple interfaces

### Benefits

- **Simplicity**: Users interact with simple interfaces without worrying about complexity
- **Code Reusability**: Abstract classes provide common functionality
- **Flexibility**: Implementation can change without affecting the interface

### Example (Python)

```python
from abc import ABC, abstractmethod

class Vehicle(ABC):
    @abstractmethod
    def start_engine(self):
        pass
    
    @abstractmethod
    def stop_engine(self):
        pass
    
    def honk(self):  # Concrete method
        print("Beep beep!")

class Car(Vehicle):
    def start_engine(self):
        print("Car engine started with key ignition")
    
    def stop_engine(self):
        print("Car engine stopped")

class ElectricCar(Vehicle):
    def start_engine(self):
        print("Electric motor activated silently")
    
    def stop_engine(self):
        print("Electric motor deactivated")

# Usage
my_car = Car()
my_car.start_engine()  # Car engine started with key ignition
my_car.honk()  # Beep beep!
```

---

## 3. Inheritance

**Inheritance** is a mechanism where a new class (child/derived class) inherits properties and behaviors from an existing class (parent/base class). This promotes code reuse and establishes relationships between classes.

### Key Concepts

- **Parent/Base Class**: The class being inherited from
- **Child/Derived Class**: The class that inherits
- **Method Overriding**: Child class provides specific implementation of parent method
- **Super Keyword**: Used to call parent class methods

### Types of Inheritance

- **Single Inheritance**: One child class inherits from one parent class
- **Multiple Inheritance**: One child class inherits from multiple parent classes
- **Multilevel Inheritance**: A child class inherits from a parent class, which itself inherits from another class
- **Hierarchical Inheritance**: Multiple child classes inherit from one parent class

### Benefits

- **Code Reusability**: Avoid writing duplicate code
- **Extensibility**: Easy to add new features to existing code
- **Natural Hierarchy**: Models real-world relationships

### Example (Python)

```python
class Animal:
    def __init__(self, name, species):
        self.name = name
        self.species = species
    
    def make_sound(self):
        print("Some generic animal sound")
    
    def info(self):
        print(f"{self.name} is a {self.species}")

class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name, "Dog")
        self.breed = breed
    
    def make_sound(self):  # Method overriding
        print("Woof! Woof!")
    
    def fetch(self):
        print(f"{self.name} is fetching the ball!")

class Cat(Animal):
    def __init__(self, name, color):
        super().__init__(name, "Cat")
        self.color = color
    
    def make_sound(self):  # Method overriding
        print("Meow! Meow!")

# Usage
dog = Dog("Buddy", "Golden Retriever")
dog.info()  # Buddy is a Dog
dog.make_sound()  # Woof! Woof!
dog.fetch()  # Buddy is fetching the ball!

cat = Cat("Whiskers", "Orange")
cat.info()  # Whiskers is a Cat
cat.make_sound()  # Meow! Meow!
```

---

## 4. Polymorphism

**Polymorphism** means "many forms" and allows objects of different classes to be treated as objects of a common parent class. It enables a single interface to represent different underlying forms (data types or classes).

### Key Concepts

- **Method Overriding**: Same method name in parent and child classes with different implementations
- **Method Overloading**: Same method name with different parameters (not directly supported in Python)
- **Duck Typing**: "If it walks like a duck and quacks like a duck, it must be a duck"

### Types of Polymorphism

- **Compile-time Polymorphism** (Method Overloading): Resolved during compilation
- **Runtime Polymorphism** (Method Overriding): Resolved during execution

### Benefits

- **Flexibility**: Same interface for different data types
- **Code Simplicity**: Write generic code that works with multiple types
- **Extensibility**: Easy to add new classes without modifying existing code

### Example (Python)

```python
class Shape:
    def area(self):
        pass
    
    def perimeter(self):
        pass

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
    
    def perimeter(self):
        return 2 * (self.width + self.height)

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return 3.14159 * self.radius ** 2
    
    def perimeter(self):
        return 2 * 3.14159 * self.radius

class Triangle(Shape):
    def __init__(self, side1, side2, side3):
        self.side1 = side1
        self.side2 = side2
        self.side3 = side3
    
    def area(self):
        s = (self.side1 + self.side2 + self.side3) / 2
        return (s * (s - self.side1) * (s - self.side2) * (s - self.side3)) ** 0.5
    
    def perimeter(self):
        return self.side1 + self.side2 + self.side3

# Polymorphism in action
def print_shape_info(shape):
    print(f"Area: {shape.area()}")
    print(f"Perimeter: {shape.perimeter()}")
    print("---")

# Usage
shapes = [
    Rectangle(5, 10),
    Circle(7),
    Triangle(3, 4, 5)
]

for shape in shapes:
    print_shape_info(shape)  # Same function works with different shape types
```

---

## Summary

|Principle|Focus|Key Benefit|
|---|---|---|
|**Encapsulation**|Bundling data and methods; restricting access|Data protection and security|
|**Abstraction**|Hiding complexity; showing only essentials|Simplified interface and reduced complexity|
|**Inheritance**|Reusing code from parent classes|Code reusability and extensibility|
|**Polymorphism**|One interface, multiple implementations|Flexibility and maintainability|

---

## How They Work Together

These four principles don't exist in isolation—they work together to create robust object-oriented systems:

1. **Encapsulation** protects the data within objects
2. **Abstraction** provides clean interfaces to interact with complex systems
3. **Inheritance** allows sharing of common functionality across related classes
4. **Polymorphism** enables flexible code that can work with objects of different types through a common interface

By mastering these four pillars, you can design software systems that are easier to understand, maintain, and extend over time.