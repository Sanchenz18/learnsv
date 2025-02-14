---
{"dg-publish":true,"permalink":"/Python/Python打包/"}
---

## FIles Tree

#### 	Project
	 ├─data         //favicon file
     ├─src
	 │ ├─other0.py
	 │ └─other1.py
	 ├─example.spec
	 └─Makefile
## example.spec

### 适用于Windows Poweshell环境
```
# -*- mode: python ; coding: utf-8 -*-

import os

base_dir = os.path.dirname(os.path.abspath(__name__))

print("current dir:", os.getcwd())
print("calculated base_dir:", base_dir)

pyfiles = [
    'src/other0.py',
    'src/other1.py',
    'src/other2.py'
]

a = Analysis(
    ['src/hello.py'],
    pathex=[os.path.join(base_dir, 'src')],
    binaries=[],
    datas=[],
    hiddenimports=[],
    hookspath=[],
    hooksconfig={},
    runtime_hooks=[],
    excludes=[],
    noarchive=False,
    optimize=0,
)
pyz = PYZ(a.pure)

exe = EXE(
    pyz,
    a.scripts,
    a.binaries,
    a.datas,
    [],
    name='HelloPrect',
    debug=False,
    bootloader_ignore_signals=False,
    strip=False,
    upx=True,
    upx_exclude=[],
    runtime_tmpdir=None,
    console=True,
    disable_windowed_traceback=False,
    argv_emulation=False,
    target_arch=None,
    codesign_identity=None,
    entitlements_file=None,
    icon=['data/favicon.ico'],
)
```
### pyfiles
`main.py`中引用的其他python文件
### pathec
搜索其他文件的路径附加前缀
### name
生成的`.exe`文件名称
### console
运行时是否伴随终端
### icon
生成的`.exe`程序的图标

## Makefile
```makefile

# 定义变量
PYTHON = python
APP_NAME = 
SPEC_FILE = .spec
DIST_DIR = dist
BUILD_DIR = build
VENV_DIR = env
REQUIREMENTS = requirements.txt

.PHONY: all build clean install run generate-spec env release help
  
# 默认目标（执行完整构建流程）
all: clean install build

# 构建可执行文件
build:
    pyinstaller $(SPEC_FILE)
    @echo "✅ 构建完成！可执行文件在 $(DIST_DIR) 目录"

# 清理构建文件
clean:
    rm -rf $(DIST_DIR) $(BUILD_DIR)
    @echo "🧹 已清理构建产物"

# 安装依赖
install:
    $(PYTHON) -m pip install -r $(REQUIREMENTS)
    @echo "📦 依赖安装完成"

run:
    $(PYTHON) -m ./

env:
    $(PYTHON) -m venv $(VENV_DIR)
    @echo "🛠️ 虚拟环境已创建，使用以下命令激活："
    @echo "  Windows: .\$(VENV_DIR)\Scripts\activate"
    @echo "  Linux: source $(VENV_DIR)/bin/activate"

release: clean build

help:
    @echo "可用命令："
    @echo "  make build      - 构建可执行文件"
    @echo "  make clean      - 清理构建产物"
    @echo "  make install    - 安装依赖"
    @echo "  make run        - 运行程序测试"
    @echo "  make release    - 执行完整发布流程"
    @echo "  make env       - 创建虚拟环境"
    @echo "  make help       - 显示帮助信息"

```
