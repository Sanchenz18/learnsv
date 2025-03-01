---
{"dg-publish":true,"permalink":"/Python/Python 面向对象编程（OOP）/"}
---

面向对象编程（Object-Oriented Programming, OOP）是一种编程的范式，它通过“对象”来组织代码。
Python 是支持 OOP 的语言，使用类和对象来实现。

| OOP 三大特性 | 描述                   | Python 实现方式              |
|-------------|-----------------------|----------------------------|
| 封装         | 隐藏内部实现细节         | 使用类封装属性和方法             |
| 继承         | 复用现有代码           | class 子类(父类) 语法          |
| 多态         | 同一方法不同表现        | 通过方法重写实现               |

## 一、类和对象
### 1. 类的定义
```python
class Dog:
    """一个简单的狗类"""
    
    # 类属性
    species = "Canis familiaris"
    
    def __init__(self, name, age):
        """初始化方法（构造函数）"""
        self.name = name  # 实例属性
        self.age = age
    
    def description(self):
        """返回狗的描述信息"""
        return f"{self.name} is {self.age} years old."
    
    def speak(self, sound):
        """让狗发出声音"""
        return f"{self.name} says {sound}"
```

### 2. 创建对象
```python
# 创建 Dog 对象
my_dog = Dog("Buddy", 3)

# 访问实例属性
print(my_dog.name)  # 输出：Buddy
print(my_dog.age)   # 输出：3

# 调用实例方法
print(my_dog.description())  # 输出：Buddy is 3 years old.
print(my_dog.speak("Woof Woof"))  # 输出：Buddy says Woof Woof
```

## 二、继承
### 1. 基本继承
```python
class GoldenRetriever(Dog):
    """金毛寻回犬类，继承自Dog类"""
    
    def fetch(self, item):
        """金毛特有的行为：取物"""
        return f"{self.name} fetched the {item}."

# 创建 GoldenRetriever 对象
my_golden = GoldenRetriever("Max", 4)
print(my_golden.description())  # 输出：Max is 4 years old.
print(my_golden.fetch("ball"))  # 输出：Max fetched the ball.
```

### 2. 方法重写
```python
class Chihuahua(Dog):
    """吉娃娃类，继承自Dog类，并重写speak方法"""
    
    def speak(self, sound="Yip"):
        """吉娃娃特有的叫声"""
        return f"{self.name} says {sound}"

# 创建 Chihuahua 对象
my_chihuahua = Chihuahua("Tiny", 2)
print(my_chihuahua.speak())  # 输出：Tiny says Yip
```

## 三、封装
### 1. 私有属性和方法
```python
class BankAccount:
    """银行账户类，使用私有属性和方法进行封装"""
    
    def __init__(self, owner, balance=0):
        self.owner = owner
        self.__balance = balance  # 私有属性
    
    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
            print(f"Deposited {amount}. New balance: {self.__balance}")
        else:
            print("Invalid deposit amount.")
    
    def withdraw(self, amount):
        if 0 < amount <= self.__balance:
            self.__balance -= amount
            print(f"Withdrew {amount}. New balance: {self.__balance}")
        else:
            print("Invalid withdrawal amount.")
    
    def get_balance(self):
        return self.__balance

# 创建 BankAccount 对象
my_account = BankAccount("Alice", 1000)
my_account.deposit(500)  # 输出：Deposited 500. New balance: 1500
my_account.withdraw(200)  # 输出：Withdrew 200. New balance: 1300
print(my_account.get_balance())  # 输出：1300
```

## 四、多态
### 1. 多态示例
```python
class Animal:
    def speak(self):
        pass

class Cat(Animal):
    def speak(self):
        return "Meow"

class Dog(Animal):
    def speak(self):
        return "Woof"

def animal_sound(animal: Animal):
    """多态示例：根据传入的不同动物对象调用相应的方法"""
    print(animal.speak())

# 创建不同类型的动物对象
my_cat = Cat()
my_dog = Dog()

# 使用多态
animal_sound(my_cat)  # 输出：Meow
animal_sound(my_dog)  # 输出：Woof
```

## 五、特殊方法（魔术方法）
### 1. `__str__` 和 `__repr__`
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __str__(self):
        return f"Point({self.x}, {self.y})"
    
    def __repr__(self):
        return f"Point(x={self.x}, y={self.y})"

p = Point(3, 4)
print(p)  # 输出：Point(3, 4)  (调用__str__)
print(repr(p))  # 输出：Point(x=3, y=4)  (调用__repr__)
```

### 2. `__add__` 和 `__len__`
```python
class Vector:
    def __init__(self, *components):
        self.components = components
    
    def __add__(self, other):
        if len(self.components) != len(other.components):
            raise ValueError("Vectors must be of the same dimension")
        return Vector(*[x + y for x, y in zip(self.components, other.components)])
    
    def __len__(self):
        return len(self.components)
    
    def __str__(self):
        return f"Vector{self.components}"

v1 = Vector(1, 2, 3)
v2 = Vector(4, 5, 6)
v3 = v1 + v2  # 调用__add__
print(v3)  # 输出：Vector(5, 7, 9)
print(len(v1))  # 输出：3  (调用__len__)
```

## 六、相关概念

| 概念   | 描述             | 示例                                      |
| ---- | -------------- | --------------------------------------- |
| 类    | 定义对象的模板        | `class Dog: ...`                        |
| 对象   | 类的实例           | `my_dog = Dog("Buddy", 3)`              |
| 属性   | 对象的状态          | `self.name`, `self.age`                 |
| 方法   | 对象的行为          | `def speak(self, sound): ...`           |
| 继承   | 子类继承父类的属性和方法   | `class GoldenRetriever(Dog): ...`       |
| 封装   | 保护对象内部状态       | `self.__balance`                        |
| 多态   | 不同对象对同一方法的不同响应 | `def animal_sound(animal: Animal): ...` |
| 特殊方法 | 自定义类的行为        | `__str__`, `__add__`                    |
