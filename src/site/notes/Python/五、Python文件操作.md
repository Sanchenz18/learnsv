---
{"dg-publish":true,"permalink":"/Python/五、Python文件操作/"}
---

## 一、基础文件操作
### 1. 打开/关闭文件
使用 `open()` 函数来打开一个文件。`open()` 函数返回一个文件对象，可以通过这个对象对文件进行读写操作。

**基本语法：**
```python
file_object = open(file_name, mode)
```

**实例：**
```python
# 传统写法（需手动关闭）
file = open("test.txt", "r")  # 打开文件
content = file.read()        # 读取内容
file.close()                 # 必须关闭！

# 推荐写法（自动管理资源）
with open("data.txt", "w") as f:  # with语句自动关闭
    f.write("Hello World")
```
**注意：**
`with` 语句用于自动管理文件资源，确保文件在操作完成后被正确关闭。

### 2. 文件打开模式
| 模式 | 描述 | 文件存在 | 文件不存在 |
|-----|------|---------|----------|
| r   | 只读  | 读取     | 报错      |
| w   | 写入  | 清空创建 | 新建      |
| a   | 追加  | 追加内容 | 新建      |
| r+  | 读写  | 从头覆盖 | 报错      |
| w+  | 读写  | 清空创建 | 新建      |

## 二、文件读写方法
### 1. 读取操作
使用文件对象的 `read()`、`readline()` 和 `readlines()` 方法来读取文件内容。

```python
# 读取全部内容
with open("poem.txt", "r") as f:
    print(f.read())

# 逐行读取（推荐大文件）
with open("data.csv", "r") as f:
    for line in f:
        print(line.strip())  # 去除换行符

# 读取指定字节
with open("image.jpg", "rb") as f:  # 二进制模式
    header = f.read(100)
```

### 2. 写入操作
使用文件对象的 `write()` 和 `writelines()` 方法来写入文件内容。

```python
# 写入单行
with open("log.txt", "a") as f:
    f.write("2023-12-25 操作记录\n")

# 写入多行（需自己添加换行符）
lines = ["第一行\n", "第二行\n"]
with open("output.txt", "w") as f:
    f.writelines(lines)

# 追加内容
with open('example.txt', 'a') as f:
    f.write("This is an appended line.\n")
```

## 三、文件指针
文件对象有一个内部指针，用于跟踪当前读写的位置。可以使用 `seek()` 方法移动指针，使用 `tell()` 方法获取当前指针位置。

**实例：**
```python
with open('example.txt', 'r+') as file:
    # 读取前5个字符
    content = file.read(5)
    print(content)  # 输出: Hello

    # 移动指针到文件开头
    file.seek(0)

    # 读取整个文件
    content = file.read()
    print(content)  # 输出: Hello, World!
```

## 四、路径处理
### 使用os模块
```python
import os

# 组合安全路径
file_path = os.path.join("docs", "report.txt")  # → docs/report.txt

# 检查文件是否存在
if os.path.exists("data.db"):
    print("数据库已存在")
```

## 五、常见问题

| 常见问题       | 解决方法                      |
|--------------|-----------------------------|
| 中文乱码       | 指定编码：open(..., encoding="utf-8") |
| 路径错误       | 使用os.path处理路径              |
| 忘记关闭文件     | 使用with语句                   |
| 大文件内存溢出   | 分块读取或逐行处理                |
