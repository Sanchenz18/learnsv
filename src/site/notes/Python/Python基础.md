---
{"dg-publish":true,"permalink":"/Python/Python基础/"}
---

## 注释
### 单行注释
使用`#`进行注释
```python
#print(hello!)
print("Hello!")  # "Hello, World!"
```

### 多行注释
使用三个双引号`"""`进行注释
```python
"""
多行
注释
"""
print("Hello, World!")
```

## 变量
### 创建变量
创建时自动匹配数据类型，甚至可以在创建后再赋值进行更改数据类型
```python
x = 5 # x is of type int
x = "Steve" # x is now of type str
print(x)
```

### 向多个变量赋值
允许在一行中向多个变量赋值
```python
x, y, z = "Orange", "Banana", "Cherry"
```

可也一在一行中向多个变量进行赋相同值
```python
x = y = z = "Orange"
```

### 输出变量（ print() ）
当需要结合文本和变量时则需要`+`进行组合
- 不能进行数字和字符串的相加，`string + 123`将会报错
```python
x = "awesome"
print("Python is " + x)
```

### 全局变量
在函数外部创建变量，并在函数内部使用它
- 要在函数内部创建全局变量，您可以使用 global 关键字
- 要在函数内部更改全局变量的值，请使用 global 关键字引用该变量
```python
x = "awesome"

def myfunc():
  print("Python is " + x)

def myfunc1():
  global x
  x = "global"

myfunc1()
print("x is" + x)
```

## 数字类型
### 复数
使用`j`来表示虚部
```python
z = 1j # complex
```

### 随机数
需要`import`模块来生成随机数
```python
import random

print(random.randrange(1,10))
```

## 字符串
`'hello'` 等同于 `"hello"`
### 字符串是数组
```python
a = "Hello"
print(a[1])     #结果是e
```

### 字符串长度
使用`len()`返回长度
```python
print(len(a))
```

## 运算符
### 算数运算

| 运算符 | 名称  |
| :-: | :-: |
|  %  | 取模  |
| **  |  幂  |
| //  | 整除  |

### 身份运算符
用于比较对象，不是比较是否相等

|  运算符   |          描述          |     实例     |
| :----: | :------------------: | :--------: |
|   is   | 如果两个变量是同一个对象，则返回True |   x is y   |
| is not |  如果不是同一个对象，则返回True   | x is not y |

### 成员运算符
用于测试序列是否在对象中出现

|  运算符   |         描述          |     实例     |
| :----: | :-----------------: | :--------: |
|   in   | 如果对象中存在指定序列，则返回True |   x in y   |
| not in | 如果不存在指定值的序列，则返回True | x not in y |

