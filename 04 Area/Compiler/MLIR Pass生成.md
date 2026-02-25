---
tags:
  - MLIR
  - compiler
title: MLIR Pass生成
date: 2026-01-05 10:35
type: permanent-note
---
---
## Pass的核心功能

### 1. **IR转换和优化**

Pass是对MLIR中间表示进行转换、优化和分析的基本单元。每个Pass都可以：

- 遍历和修改操作（Operations）
- 分析代码结构
- 应用优化策略
- 执行IR级别的转换

### 2. **Pass的主要组成部分**

#### **PassBase类** - Pass的基础定义

- **`argument`**: 命令行参数，用于在命令行中指定该Pass
- **`baseClass`**: C++基类，定义Pass的类型
- **`summary`**: Pass的单行简短描述
- **`description`**: Pass的详细人类可读描述
- **`constructor`**: 创建Pass实例的C++构造函数调用
- **`dependentDialects`**: Pass可能生成实体的方言列表
- **`options`**: Pass提供的配置选项
- **`statistics`**: Pass收集的统计信息

#### **Option类** - Pass的配置选项

允许在运行时配置Pass的行为：

- **`cppName`**: C++变量名
- **`argument`**: 命令行参数名
- **`type`**: C++类型（如int、bool、string）
- **`defaultValue`**: 默认值
- **`description`**: 选项描述
- **`additionalOptFlags`**: 附加标志

例如：可以定义优化级别、是否启用某些特性等选项。

#### **Statistic类** - Pass的统计信息

用于收集和报告Pass执行的统计数据：

- **`cppName`**: C++统计变量名
- **`name`**: 统计信息显示名称
- **`description`**: 统计信息描述

例如：优化次数、删除的指令数量、转换的操作数等。
### 3. **Pass的类型**

#### **OperationPass**
```c++
class Pass<string passArg, string operation = "">
```
- 针对特定Operation类型的Pass
- 可以指定操作类型（如`func.func`、`module`等）
- 是最常用的Pass类型

#### **InterfacePass**
```c++
class InterfacePass<string passArg, string interface>
```
- 针对实现特定接口的操作的Pass
- 更灵活，可以处理多种操作类型
- 只要操作实现了指定接口即可

### 4. **Pass的实际应用场景**

- **优化Pass**: 常量折叠、死代码消除、公共子表达式消除
- **转换Pass**: 将高级IR降低到低级IR（如Affine → SCF → Standard）
- **分析Pass**: 收集程序信息但不修改IR
- **规范化Pass**: 将IR转换为标准形式
- **验证Pass**: 检查IR的正确性和一致性

### 5. **Pass的执行模型**

Pass通过PassManager执行，支持：

- **Pass管道**: 多个Pass按顺序执行
- **嵌套执行**: 在不同IR层级上执行不同Pass
- **并行化**: 某些Pass可以并行执行
- **依赖管理**: 自动加载依赖的方言

### 6. **方言依赖（dependentDialects）**

Pass可以声明它可能产生的方言，确保：

- 在Pass执行前加载必要的方言
- 避免运行时错误
- 支持动态方言注册

### 7. **Tabelgen 的写法**
```c++
#ifndef TEMP_PASS_TD_
#define TEMP_PASS_TD_

include mlir/Pass/PassBase.td

def TempPass : Pass<"temp-pass", "::mlir::TempPass"> {
	let summary = "The summary of the Pass";
	let description = [{
		The description of the Pass
	}];
	let dependentDialect = ["mlir::affine::AffineDialect"];
}

#endif
```

语法结构
```
def  TempPass : Pass<"temp-pass", "func::FuncOp"> {}
|    |        | |     |           └── 参数：指定Pass操作的类型
|    |        | |     └── 参数：命令行标志
|    |        | └── 基类：Pass模板
|    |        └── 继承符号
|    └── 记录名称（c++ 类名基础）
└── TabelGen 定义关键字
```
**命令行标志：** 表示用户来调用这个 Pass 的名称
```shell
./mlir-opt --temp-pass input.mlir
```

### 8. C++实现
- TempPass.h
```c++
#ifndef TEMP_PASS_H_
#define TEMP_PASS_H_

namespace mlir {
namespace xxx {

#define GEN_PASS_DECL_TEMPPASS
#include "path/to/Passes.h.inc"

}
}
#endif
```
- TempPass.cpp
```c++
namespace mlir {
namespace xxx {
#define GEN_PASS_DEF_TEMPPASS
#include "path/to/Passes.h.inc"

// CRTP方式实现TempPass类
struct TempPass : TempPassBase<TempPass> {
	using TempPassBase::TempPassBase;
	
	void runOperation(){
		xxxxx;
		signalPassFailure()
	}
}
}
}
```