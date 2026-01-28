---
tags:
  - MLIR
  - compiler
title: MLIR Dialect生成
date: 2026-01-05 15:09
type:
---
---
## Dialect的核心功能

Dialect是MLIR中用于组织和扩展IR的模块化机制，它定义了一组相关的操作、类型、属性和转换规则。可以理解为一个"领域特定语言"的容器。

## Dialect的主要组成部分

### 1. **基本标识信息**

- **`name`**: 方言的唯一名称（必需）
- **`summary`**: 方言的简短摘要
- **`description`**: 方言的详细描述文档
- **`cppNamespace`**: C++命名空间，默认使用方言名称，支持嵌套命名空间（如`"A::B"`）

### 2. **依赖管理**

- **`dependentDialects`**: 该方言依赖的其他方言列表
    - 在方言构造时自动加载这些依赖
    - 用于规范化模式或接口中可能涉及的方言
    - 确保运行时所需方言都已加载

### 3. **属性管理**

- **`discardableAttrs`**: 可丢弃属性的键值对列表
    - 生成辅助类以类型安全的方式管理操作上的可丢弃属性
    - 允许在不影响语义的情况下添加元数据

### 4. **常量物化**

- **`hasConstantMaterializer`**: 是否重写常量物化钩子
    - 控制如何将常量值具体化为IR操作
    - 用于优化和常量折叠

### 5. **验证钩子**

Dialect可以定义多种验证钩子来确保IR的正确性：

- **`hasOperationAttrVerify`**: 验证操作属性
- **`hasRegionArgAttrVerify`**: 验证区域参数属性
- **`hasRegionResultAttrVerify`**: 验证区域结果属性

这些钩子允许方言实施自己的语义约束。

### 6. **解析和打印**

- **`useDefaultAttributePrinterParser`**: 使用默认生成的属性解析器和打印器
    
    - ODS自动生成解析和打印钩子的声明和实现
    - 自动分发到每个单独的属性
- **`useDefaultTypePrinterParser`**: 使用默认生成的类型解析器和打印器
    
    - 类似于属性，自动处理类型的文本表示

### 7. **接口和扩展**

- **`hasOperationInterfaceFallback`**: 操作接口回退钩子
    
    - 允许方言为未显式实现的接口提供默认行为
    - 支持动态接口实现
- **`isExtensible`**: 方言是否可在运行时扩展
    
    - 允许动态添加新操作或类型
    - 支持插件式架构

### 8. **优化支持**

- **`hasCanonicalizer`**: 是否提供规范化模式钩子
    - 定义该方言的规范化转换规则
    - 用于将IR转换为标准形式
    - 支持编译器优化

### 9. **生命周期管理**

- **`hasNonDefaultDestructor`**: 是否提供非默认析构函数
    - 如果为false，将生成默认析构函数实现
    - 用于资源清理和自定义终结化

### 10. **属性存储策略**

- **`usePropertiesForAttributes`**: 是否将固有属性存储为Properties
    - 默认为true
    - 影响ODS定义的属性在内存中的存储方式

### 11. **额外声明**

- **`extraClassDeclaration`**: 额外的C++代码块
    - 可以在方言类中添加自定义方法和成员
    - 提供完全的扩展灵活性

## Dialect的实际应用

MLIR中的典型方言包括：

- **`arith`**: 算术操作方言
- **`func`**: 函数和调用操作
- **`scf`**: 结构化控制流
- **`affine`**: 仿射表达式和循环
- **`tensor`**: 张量操作
- **`linalg`**: 线性代数操作
- **[llvm](vscode-file://vscode-app/usr/share/code/resources/app/out/vs/code/electron-browser/workbench/workbench.html)**: LLVM IR方言
- **`gpu`**: GPU特定操作

## Dialect的设计优势

1. **模块化**: 每个方言独立封装特定领域的操作
2. **可组合**: 不同方言可以在同一个IR中混合使用
3. **可扩展**: 用户可以定义自己的方言
4. **类型安全**: 通过TableGen生成类型安全的C++代码
5. **渐进式降低**: 支持从高级抽象逐步降低到机器码

## **Tabelgen 的写法**
```c++
#ifndef TEMP_DIALECT
#define TEMP_DIALECT

include mlir/IR/DialectBase.td

def Temp_Dialect : Dialect {
	// 方言的名字
	let name = "north_star";
	// 方言的概述
	let summary = "summary of NorthStar Dialect";
	// 方言的详细描述
	let description = "description of NorthStar Dialect";
	// 方言的依赖
	let dependentDialects = ["::mlir::tensor::TensorDialect"];
	// 用于生成比较标准的属性管理的代码 [4-7]
	let discardableAttrs = (ins);
	// 生成代码的命名空间
	let cppNamespace = "::mlir::north_star";
	// 额外的声明.
	let extraClassDeclaration = [{
	static void sayHello();
	}];
	// 规范化的声明. [14]
	let hasConstantMaterializer = 0;
	// 是否生成默认的析构函数
	let hasNonDefaultDestructor = 1;
	// 操作数的属性检验 [7]
	let hasOperationAttrVerify = 0;
	// RegionArg的属性检验 [7]
	let hasRegionArgAttrVerify = 0;
	// RegionResult的属性检验 [7]
	let hasRegionResultAttrVerify = 0;
	// [6]
	let hasOperationInterfaceFallback = 0;
	// 使用MLIR默认的属性解析输出.
	let useDefaultAttributePrinterParser = 0;
	// 使用MLIR默认的类型解析输出.
	let useDefaultTypePrinterParser = 0;
	// 是否有规范化patten[14].
	let hasCanonicalizer = 0;
	// 是否是可扩展的方言.
	let isExtensible = 0;
	// Whether inherent Attributes defined in ODS will be stored as Properties.
	let usePropertiesForAttributes = 1;
}

#endif
```