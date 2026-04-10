---
tags:
  - MLIR
  - Compiler
title: MLIR Dialect生成
date: 2026-01-05 15:09
type: permanent-note
---
---
## 一、生成 Dialect 的 CMakeLists 文件
MLIR 可以使用 tablegen 工具来自动地生成一些模版代码，其中 CMake 文件编写方式可以参考下面内容
```python
# 设置 Tablegen 的输入文件，告诉 LLVM/MLIR 的 CMake 工具链，后续 mlir_tablegen() 命令应该使用哪个 .td 文件作为源文件
set(LLVM_TARGET_DEFINITIONS xxx.td)

# 生成自定义 Dialect 的声明，包含类定义、成员函数的声明等。等价于 `mlir-tblgen xxx.td --gen-dialect-decls -dialect=north_star -o xxx.h.inc`
mlir_tablegen(xxx.h.inc --gen-dialect-decls -dialect=north_star)
# 生成自定义 Dialect 的 C++ 实现。
mlir_tablegen(xxx.cpp.inc --gen-dialect-defs -dialect=north_star)

# 将上面的 tablegen 命令封装成一个 CMake 的target，其他目标可以通过 add_dependencies() 依赖此目标，确保在编译依赖代码前完成相关文件的生成
add_public_tablegen_target(XXXDialectIncGen)

# 使用 MLIR 的 CMake 宏创建一个 Dialect 库目标
add_mlir_dialect_library(XXXDialect
                         XXXDialect.cpp
                         
                         DEPENDS # 要完成此库的构建，必须先完成下面目标的生成
                         XXXDialectIncGen
                         
                         LINK_LIBS PUBLIC
                         MLIRIR
                         MLIRTensorDialect)
```

## 二、Dialect 中 tablegen 文件的主要内容

Dialect 是 MLIR 中用于组织和扩展IR的模块化机制，它定义了一组相关的操作、类型、属性和转换规则。可以理解为一个"领域特定语言"的容器。

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
    - 允许使用这些依赖方言中的类型和操作

### 3. **属性管理**

- **`discardableAttrs`**: 可丢弃属性的键值对列表
    - 生成辅助类以类型安全的方式管理操作上的可丢弃属性
    - 允许在不影响语义的情况下添加元数据

### 4. **常量物化**

- **`hasConstantMaterializer`**: 是否重写常量物化钩子
    - 将编译期常量属性（Attribute）转换为表示 IR 的 Operation
    - 使得属性可以跨 Dialect 进行常量传播
    - 可以优化 Pass 产生新常量
    - 可以进行类型转换

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
    - 通过 Pattern 重写简化/标准化 Operation

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


## 三、**Tabelgen 的写法**
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

## 四、Dialect 中 C++ 代码
### 头文件
```c++
#ifndef XXX_H
#define XXX_H

// 在头文件中需要导入由tablegen生成的 h.inc 文件
#include "Path/to/XXXDialect.h.inc"

#endif
```

### 源文件
```c++
// 导入对应的头文件
#include "Path/to/XXXDialect.h"
// 导入由 tablegen 生成的源文件
#include "Path/to/XXXDialect.cpp.inc"

namespace mlir::xxx {

// 实现方言的初始化方法
void NorthStarDialect::initialize() {
	llvm::outs() << "initializing " << getDialectNamespace() << "\n";
	registerType();
	registerAttrs();
}

// 实现方言的析构函数
NorthStarDialect::~NorthStarDialect() {
	llvm::outs() << "destroying " << getDialectNamespace() << "\n";
}

// 实现在 extraClassDeclaration 当中声明的方法。
void NorthStarDialect::sayHello() {
	llvm::outs() << "Hello in " << getDialectNamespace() << "\n";
}

}
```

### 主程序
```c++
// 导入Dialect的头文件
#include "Path/to/XXXDialect.h"
// 导入 mlir 中方言的注册器
#include "mlir/IR/DialectRegistry.h"
// 导入 mlir 中MLIR的上下文环境
#include "mlir/IR/MLIRContext.h"

int mian() {
	// 创建方言注册器
	mlir::DialectRegistry registry;
	// 创建上下文环境
	mlir::MLIRContext context(registry);
    // 加载/注册方言
	auto dialect = context.getOrLoadDialect<mlir::north_star::NorthStarDialect>();

	// 调用方言中的方法
	dialect->sayHello();
}

```
