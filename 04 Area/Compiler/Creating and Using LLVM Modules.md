---
tags:
  - MLIR
title: Creating and Using LLVM Modules
date: 2025-10-21 17:26
type:
---
---
介绍一个简单 LLVM 模块返回 42 作为退出的 code，并将它编译成一个 shared library，并用 python 调用。这个示例展示了从 LLVM IR 到执行 code 的完整流程
## 1. 写 LLVM IR
创建一个文件命名为 simple.ll 并写入如下代码：
```python
define i32 @main() {
    ret i32 42
}
```

## 2. 编译成动态链接库
```bash
llc -filetype=obj --relocation-model=pic simple.ll -o simple.o
clang -shared -fPIC simple.o -o libsimple.so
clang simple.o -o simple # optionally create an executable
```

## 3. 用 python 调用
```python
import ctypes

module = ctypes.CDLL("./libsimple.so")

module.main.argtypes = []
module.main.restype = ctypes.c_int

print(module.main())
```