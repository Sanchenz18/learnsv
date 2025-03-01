---
{"dg-publish":true,"permalink":"/Python/二、Python数据结构基础/"}
---

## 一、列表（List）
列表是一种有序的、可变的数据结构。可以存储不同类型的元素。

```python
# 创建列表（有序、可变）
fruits = ["apple", "banana", "cherry"]

# 索引访问（注意从0开始）
print(fruits[1])  # 输出：banana

# 切片操作 [start:end:step]
print(fruits[0:2])  # 输出：['apple', 'banana']

# 常用方法
fruits.append("orange")   # 添加元素
fruits.insert(1, "mango") # 指定位置插入
fruits.remove("banana")   # 删除元素
```

## 二、元组（Tuple）
元组是一种有序的、不可变的数据结构。一旦创建，不能修改。

```python
# 创建元组（有序、不可变）
colors = ("red", "green", "blue")

# 访问元素（与列表相同）
print(colors[-1])  # 输出：blue 

# 错误示范（元组不可修改）
# colors[0] = "yellow"  # 会报错
```

## 三、字典（Dict）
字典是一种无序的、可变的数据结构，以键值对的形式存储数据。

```python
# 创建字典（键值对集合）
student = {
    "name": "小明",
    "age": 18,
    "courses": ["数学", "英语"]
}

# 访问元素
print(student.get("age"))    # 输出：18
print(student["courses"])    # 输出：['数学', '英语']

# 新增/修改元素
student["gender"] = "男"     # 新增
student["age"] = 19          # 修改
```

## 四、集合（Set）
```python
# 创建集合（无序、元素唯一）
unique_nums = {1, 2, 2, 3, 4}

# 自动去重后结果：{1, 2, 3, 4}

# 集合运算
A = {1,2,3}
B = {3,4,5}
print(A | B)  # 并集 {1,2,3,4,5}
print(A & B)  # 交集 {3}

# 检查成员
if 4 in unique_nums:
    print("4 is in the set")

# 集合运算
set1 = {1, 2, 3}
set2 = {3, 4, 5}
print(set1.union(set2))  # 并集: {1, 2, 3, 4, 5}
print(set1.intersection(set2))  # 交集: {3}
print(set1.difference(set2))  # 差集: {1, 2}
```

| 数据结构  | 可变性 | 有序性 | 特点               |
|----------|--------|--------|--------------------|
| 列表     | ✔️     | ✔️     | 可重复，元素可修改  |
| 元组     | ❌     | ✔️     | 创建后不可修改      |
| 字典     | ✔️     | ❌     | 键不可重复，快速查找 |
| 集合     | ✔️     | ❌     | 元素唯一，数学运算  |
