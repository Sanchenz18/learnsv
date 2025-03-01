---
{"dg-publish":true,"permalink":"/Python/四、Python函数/"}
---

## 一、函数定义与调用
使用 `def` 关键字来定义一个函数。函数可以有参数和返回值。

**基本语法：**
```python
def function_name(parameters):
    # 函数体
    return value  # 可选
```

**实例：**
```python
# 基础函数定义（def 关键字）
def greet(name):
    """打印问候语（文档字符串）"""
    print(f"Hello, {name}!")

print(greet("Alice"))  # 输出: Hello, Alice!
```

## 二、参数传递
函数可以接受多种类型的参数，包括位置参数、关键字参数、默认参数和可变参数。
### 1. 位置参数
位置参数是最常见的参数类型，调用时必须按顺序传递。

```python
def calculate(x, y):
    return x * 2 + y

print(calculate(3, 5))  # 3→x, 5→y → 输出：11
```

### 2. 默认参数
关键字参数允许在调用函数时指定参数名，这样就不需要按顺序传递。

```python
def order(food, count=1):  # 默认购买1份
    print(f"{count}份{food}")

order("拉面")     # → 1份拉面
order("饺子", 3)  # → 3份饺子
```

### 3. 关键字参数（明确指定参数）
默认参数允许为函数参数设置默认值，如果调用时未提供该参数，则使用默认值。

```python
def create_user(name, age, city):
    print(f"{name} {age}岁 来自{city}")

create_user(age=20, city="北京", name="小明")
```

### 4.可变参数
可变参数允许函数接受任意数量的位置参数或关键字参数。

- 使用 `*args` 接受任意数量的位置参数。
- 使用 `**kwargs` 接受任意数量的关键字参数。

**示例：**
```python
def sum_all(*args):
    return sum(args)

print(sum_all(1, 2, 3, 4))  # 输出: 10

def print_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="Alice", age=25, city="New York")
# 输出:
# name: Alice
# age: 25
# city: New York

## 三、返回值
```python
# 返回单值
def add(a, b):
    return a + b

# 返回多值（实际上是返回元组）
def calculator(x, y):
    return x+y, x-y

sum_result, sub_result = calculator(8, 3)
```

## 四、变量作用域
```python
global_var = "全局"

def test_scope():
    local_var = "局部" 
    print(global_var)       # 可访问全局变量
    print(local_var)        # 可访问局部变量

test_scope()
print(local_var)  # ❌ 报错，外部无法访问局部变量
```

## 五、Lambda函数
lambda 表达式是一种简洁的方式来创建匿名函数。常用于需要简单函数的地方。

**基本语法：**
```python
lambda arguments: expression
```

**实例：**
```python
# 匿名函数（适合简单操作）
square = lambda x: x ** 2
print(square(5))  # 输出：25

# 配合排序使用
points = [(1,2), (3,1), (5,0)]
points.sort(key=lambda p: p[1])  # 按y坐标排序 → [(5,0), (3,1), (1,2)]
```

## 六、函数特性与注意

| 函数特性       | 描述                       | 应用场景               |
|--------------|--------------------------|----------------------|
| 代码复用       | 避免重复代码                | 多次执行的相同操作         |
| 参数灵活性      | 支持多种参数传递方式          | 配置不同场景的函数行为       |
| 模块化        | 分解复杂问题为小任务          | 大型项目开发             |
| 作用域隔离      | 局部变量不影响全局变量         | 避免变量污染             |

**注意：**
1. 函数不写`return`语句时默认返回`None`
2. 函数参数传递的是对象引用（可变参数在函数内部修改会影响外部）
3. 建议函数长度不超过50行（保持单一职责）