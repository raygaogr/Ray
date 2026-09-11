---
tags:
  - MLIR
  - Compiler
title: MLIR Concepts
date: 2025-08-04 10:44
type: permanent-note
---
---
## 基本的标识符
**%** 前缀：SSA 变量值
**@** 前缀：函数
**^** 前缀：基本块
**#** 前缀：属性别名
**:** 和 **->** 用来表示值或者操作类型
**!** 前缀：类型别名
**()** 用来表示参数
**{}** 用来表示作用域
**//** 用来表示注释
**<>** 用来表示类型参数
## 相关概念
### **Modules**
MLIR 操作的顶层容器
```python
module {
	// Operations
}
```

### **Functions**
一组按特定顺序执行的操作的集合
```python
func.func @my_function(%arg0: i32, %arg1: i32) -> i32 {
	// Operations
}
```

### **Dialects**
MLIR 中关于特定领域的操作和类型的集合
High-level 的常用的 dialect 有：
- `tensor`：这个 dialect 为多维数组提供创建和处理的相关操作，可以让 high-level 的数学表达以及变化处于无副作用的模式
- **`linalg`**：linear algebra 方言为线性代数的相关计算提供一组操作
- **`omp`**：OpenMP 方言支持并行编程模型的操作，支持开发者按 openmp 的方式表达并行
- **`affine`**：这个方言提供了一个框架用来表达仿射变换
- **`gpu`**：GPU 方言专门用于 GPU 编程，为异构框架提供并行操作
Low-level 的常用的 dialect 有：
- **`scf`**：structured control flow 方言提供操作表示结构化控制流，包括循环、条件语句
- **`func`**：为函数的定义和调用提供相关的操作
- **`memref`**：用于内存访问的相关操作，允许高效的管理和处理内存
- **`index`**：用于处理 index 的计算，这在数组元素访问中非常重要
- **`arith`**：提供基础数值计算操作包括整型和浮点类型
### **Operations**
MLIR 中工作的基本单元，类似于 LLVM 的 instructions，但还包括额外的方言的 namespace 和可能的类型声明
```python
%0 = "my_dialect.my_operation"(%arg0, %arg1): (i32, i32) -> i32
```
一个具体的示例：
```python
%0 = arith.addf %arg0, %arg1 : f32
```

### **Basic Blocks**
MLIR 中的基本块是一组从上到下按顺序执行的操作，不包含任何分支。每个基本块拥有一个单一入口但可能有多个出口。
```python
^bb1: // 后面block的标签
%then_result = arith.muli %result, 2 : i32
return %then_result : i32
```

### **Regions**
Regions 用来把一组 operation 进行关联，通常用来表示控制流的结构如循环和条件判断
```python
{
  ^bb1(%result : i32):
  %then_result = arith.muli %result, 2 : i32
  return %then_result : i32
}
```

### **Types**
用来表示一个变量的值的类型以及可执行的操作

### **Passes**
对 MLIR 中的方言的变换，优化并 lower 成更简单的结构
下面是一组可以传给 mlir-opt 的参数用来改变 MLIR，最常见的有：
- `convert-func-to-llvm`: Convert function-like operations to LLVM dialect
- `convert-math-to-llvm`: Convert math operations to LLVM dialect
- `convert-index-to-llvm`: Convert index operations to LLVM dialect
- `convert-scf-to-cf`: Convert structured control flow to CF dialect
- `convert-cf-to-llvm`: Convert control flow to LLVM dialect
- `convert-arith-to-llvm`: Convert arithmetic operations to LLVM dialect
- `reconcile-unrealized-casts`: Reconcile unrealized casts
- `convert-memref-to-llvm`: Convert memref operations to LLVM dialect
- `convert-tensor-to-llvm`: Convert tensor operations to LLVM dialect
- `convert-linalg-to-scf`: Convert linalg operations to `scf.for` loops
- `convert-linalg-to-affine-loops`: Convert linalg operations to `affine.for` loops
- `convert-omp-to-llvm`: Convert OpenMP operations to LLVM dialect
- `convert-vector-to-llvm`: Convert vector operations to LLVM dialect
