---
{"dg-publish":true,"permalink":"/Python/三、Python流程控制/"}
---

## 一、条件控制（if/elif/else语句）
```python
# 基础格式（注意冒号和缩进）
age = 18
if age < 16:
    print("儿童")
elif 16 <= age < 18:
    print("青少年")
else:
    print("成年人")  # 输出：成年人

# 嵌套判断示例
score = 88
if score >= 60:
    if score >= 90:
        print("优秀！")
    else:
        print("合格")
else:
    print("不合格")
```

## 二、循环控制(for 和 while)
### 1. for循环
`for` 循环用于遍历序列（如列表、元组、字符串等）或其他可迭代对象。

**基本语法：**
```python
for item in iterable:
    # 执行代码块
```

**实例：**
```python
# 遍历列表
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(f"I love {fruit}")

# 配合range()使用
for i in range(3):      # 输出0,1,2
    print(i)

# 遍历字典
student = {"name": "小明", "age": 18}
for key in student:
    print(f"{key}: {student[key]}")
```

### 2. while循环
`while` 循环在给定的条件为真时重复执行代码块。

**实例：**
```python
# 基础示例
count = 0
while count < 3:
    print(f"计数: {count}")
    count += 1

# 带else的循环（正常结束循环时执行）
num = 3
while num > 0:
    print(num)
    num -= 1
else:
    print("发射！")
```

## 三、循环控制语句
- `break` 语句用于立即退出循环。
- `continue` 语句用于跳过当前循环的剩余部分，并继续下一次迭代。

**实例：**
```python
# break跳出整个循环
for i in range(10):
    if i == 5:
        break
    print(i)  # 输出0-4

# continue跳过当前迭代
for i in range(5):
    if i == 2:
        continue
    print(i)  # 输出0,1,3,4

# pass占位语句（保持结构完整性）
if True:
    pass  # 后续补充代码
```

## 四、循环与错误

| 循环类型   | 适用场景                     | 注意事项                 |
|----------|----------------------------|------------------------|
| for      | 已知迭代次数/需要遍历集合元素  | 注意range()的起始/结束值 |
| while    | 条件控制循环次数             | 避免无限循环，需有终止条件 |

**常见错误案例：**
```python
# 错误1：忘记冒号
if x > 5  # ❌ 缺少冒号

# 错误2：缩进不一致
if True:
    print("正确")
     print("错误缩进")  # ❌ IndentationError
```
