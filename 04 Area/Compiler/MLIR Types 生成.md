---
tags:
  - MLIR
  - Compiler
title: MLIR Types 生成
date: 2026-04-10 10:30
type:
---
---
## 一、生成 Types 的 CMakeLists 文件
在 [[MLIR Dialect生成#一、生成 Dialect 的 CMakeLists 文件|Dialect CMakeLists]] 文件的基础上修改成下面的内容
```python
################################>>修改<<##################################
# 修改 tablegen 文件的入口
set(LLVM_TARGET_DEFINITIONS XXXTypes.td)
#########################################################################

# 保留 Dialect 相关的CMakeList 内容
mlir_tablegen(NorthStarDialect.h.inc --gen-dialect-decls -dialect=north_star)
mlir_tablegen(NorthStarDialect.cpp.inc --gen-dialect-defs -dialect=north_star)

################################>>新增<<##################################
# 生成 Types 用于导入的头文件和源文件
mlir_tablegen(XXXTypes.h.inc --gen-typedef-decls -dialect=north_star)
mlir_tablegen(XXXTypes.cpp.inc --gen-typedef-defs -dialect=north_star)
#########################################################################


# 将上面的生成命令定义成一个目标
add_public_tablegen_target(XXXDialectIncGen)


# 打包成库目标
add_mlir_dialect_library(MLIRXXXDialect
                         XXXDialect.cpp                         ################################>>新增<<##################################
                         XXXTypes.cpp
#########################################################################
                         
                         DEPENDS
                         XXXDialectIncGen
                         
                         LINK_LIBS PUBLIC
                         MLIRIR
                         MLIRTensorDialect)

```

## 二、Types 中 tablegen 文件的主要内容
**Type（类型）** 是对**值的性质**的抽象描述，它定义了：

| 方面       | 说明                    |
| -------- | --------------------- |
| **内存布局** | 值占用多少字节？如何对齐？         |
| **合法操作** | 可以对该值做什么？（加、减、索引、调用等） |
| **语义含义** | 值代表什么？（整数、浮点、张量、函数等）  |
### 1、描述性字段
- **`summary `**: 定义类型的概述
- **`description`**: 类型的详细描述
### 2、类型的参数
- **`parameters`**: 将类型参数化，**形成类型族**，它是类型本身的一部分，影响类型的唯一性，即两个类型如果相等，则说明这个两个类型的所有参数都相等。

使用 (ins ...) 语法，它是 TableGen 中定义的输入参数列表语法，遵循的格式如下：
```c++
"int64_t": $device_id
 |         |    | 
 |         |    |——— 参数名
 |         |———————— 表示这是一个命名引用
 |—————————————————— c++ 类型字符串
 
 ArrayRefParameter<"int64_t">: $shape
 |        |           |           |—— 参数名    
 |        |           |—————————————— 模板参数 c++ 类型
 |        |—————————————————————————— 参数包装器，数组引用
 |——————————————————————————————————— 参数类型
```

#### 2.1 基本类型
```c++
// 整数类型 
"int64_t":$dim → int64_t getDim() 

// 无符号整数 
"unsigned":$version → unsigned getVersion() 

// 布尔类型 
"bool":$isDynamic → bool getIsDynamic() 

// MLIR 类型 
"Type":$elementType → Type getElementType() 
"Attribute":$attr → Attribute getAttr()
```
#### 2.2 特殊包装类型

|包装类型|语法|用途|生成的 C++|
|---|---|---|---|
|**ArrayRefParameter**|`ArrayRefParameter<"T">`|数组/列表参数|`ArrayRef<T>`|
|**StringRefParameter**|`StringRefParameter<>`|字符串参数|`StringRef`|
|**OptionalParameter**|`OptionalParameter<"T">`|可选参数|`Optional<T>`|
|**DefaultValuedParameter**|`DefaultValuedParameter<"T", "default">`|带默认值的参数|`T`（带默认值）|
### 3、存储类配置
- **`genStorageClass`、`hasStorageCustomConstructor`**: 是 MLIR 中类型的内部存储实现，MLIR 使用 hash 机制管理类型，当 let genStrageClass = 1 时，TableGen 会自动生成 Storage 类
在 MLIR 中，**StorageClass** 是类型的**内部存储实现**，负责：

1. **存储类型参数**（如 shape、elementType、device_id）
2. **计算哈希值**（用于唯一化查找）
3. **相等比较**（判断两个类型是否相同）

### 4、类型构建器
- **`builders`**: 定义了创建类型的快捷方法
整体结构如下
```c++
let builders = [ 
  TypeBuilder<(ins 参数列表), 代码块>, 
  TypeBuilder<(ins ...), [{...}]>, 
  // 可以有多个 builder 
];
```

| 部分                     | 说明               |
| ---------------------- | ---------------- |
| `let builders = [...]` | 定义构建器列表          |
| `TypeBuilder<...>`     | 单个构建器的定义         |
| `(ins ...)`            | 输入参数声明           |
| `[{...}]`              | C++ 代码块（自定义构造逻辑） |
#### 4.1 TypeBuilder 语法详解

**4.1.1 参数列表 (ins ...)**
与 `parameters` 类似，但只声明这个 builder 需要的参数

**4.1.2 代码块 [{...}]**

| 符号          | 含义   | 说明                        |
| ----------- | ---- | ------------------------- |
| `$_get`     | 占位符  | 替换为 `Type::get()` 工厂函数    |
| `$参数名`      | 参数引用 | 如 `$shape` 引用传入的 shape 参数 |
| `$_context` | 上下文  | 隐式可用的 MLIRContext         |
### 5、汇编格式
- **`hasCustomAssemblyFormat` 、`assemblyFormat`**: 定义 MLIR 中汇编/文本的格式，用于在 `.mlir` 文件中的表示，第一种是自定义格式，需要开发者手动实现，第二种是自动格式，TableGen 自动生成打印/解析代码
### 6、验证声明
- **`genVerifyDecl`**: 类型验证，验证函数确保类型的参数合法有效

## 三、**Tabelgen 的写法**
```python
#ifndef DIALECT_NORTH_STAR_TYPES_TD
#define DIALECT_NORTH_STAR_TYPES_TD

include "mlir/IR/DialectBase.td"
include "mlir/IR/Traits.td"
include "mlir/IR/AttrTypeBase.td"
# 需要导入 Dialect 的td文件
include "Dialect/NorthStar/IR/NorthStarDialect.td"
include "mlir/IR/BuiltinTypeInterfaces.td"

# 定义方言的语法规则，继承 TypeDef 这个类，需要将它的 Dialect 参数指定到将定义的目标 Dialect 上
# 定义一个基类，说明所有继承 NorthStar_Type 的类型属于 NorthStar_Dialect 方言
class NorthStar_Type<string name, string typeMnemonic, list<Trait> traits = [], string baseCppClass = "::mlir::Type"> : TypeDef<NorthStar_Dialect, name, traits, baseCppClass> {
	let mnemonic = typeMnemonic;
	let typeName = dialect.name # "." # typeMnemonic;
}

# 定义一个具体的类型
def NorthStar_TensorType : NorthStar_Type<"NSTensor","ns_tensor",[]>{

// 概述
let summary = " the summary of north-star tensor type";
// 方言的详细描述
let description = "description of north-star tensor type";

  
// 参数
let parameters = (ins
	ArrayRefParameter<"int64_t">:$shape,
	"Type":$elementType,
	"int64_t":$device_id
);

// 是否生成StorageClass, 无特殊情况，建议设为ture
let genStorageClass = 1;

// 不建议改动
let hasStorageCustomConstructor = 0;

// 额外的builder 声明
let builders = [
TypeBuilder<(ins
	"::mlir::ArrayRef<int64_t>":$shape,
	"::mlir::Type":$elementType),
	[{
	return $_get(elementType.getContext(), shape, elementType, 0);
	}]>

];

let hasCustomAssemblyFormat = 1;
// let assemblyFormat = "`<`$shape`,`$elementType`,`$device_id`>`";
  
// 跳过默认的builder函数
let skipDefaultBuilders = 0;

// 是否生成类型检验的函数声明
let genVerifyDecl = 1;

  

let extraClassDeclaration = [{
// using TensorType::clone;
// using ShapedType::Trait<NSTensorType>::getElementTypeBitWidth;
// using ShapedType::Trait<NSTensorType>::getRank;
// using ShapedType::Trait<NSTensorType>::getNumElements;
// using ShapedType::Trait<NSTensorType>::isDynamicDim;
// using ShapedType::Trait<NSTensorType>::hasStaticShape;
// using ShapedType::Trait<NSTensorType>::getNumDynamicDims;
// using ShapedType::Trait<NSTensorType>::getDimSize;
// using ShapedType::Trait<NSTensorType>::getDynamicDimIndex;
// NSTensorType clone(::mlir::Type elementType) {
// return ::llvm::cast<NSTensorType>(cloneWith(getShape(), elementType));
// }
}];

}

#endif // DIALECT_NORTH_STAR_TYPES_TD
```

## 四、Types 中 C++ 代码
### 1、头文件
```c++
#ifndef DIALECT_NORTH_STAR_TYPES_H
#define DIALECT_NORTH_STAR_TYPES_H

// 引入 mlir 中的上下文
#include "mlir/IR/MLIRContext.h"

// 导入 tablegen 生成的 Types 的头文件
#define GET_TYPEDEF_CLASSES
#include "Dialect/NorthStar/IR/NorthStarTypes.h.inc"

#endif // DIALECT_NORTH_STAR_TYPES_H
```

### 2、源文件
```c++
#include "Dialect/NorthStar/IR/NorthStarTypes.h"
#include "Dialect/NorthStar/IR/NorthStarDialect.h"

#include "llvm/ADT/TypeSwitch.h"
#include "llvm/Support/LogicalResult.h"
#include "llvm/Support/raw_ostream.h"
#include "mlir/Dialect/Arith/IR/Arith.h"
#include "mlir/IR/BuiltinTypeInterfaces.h"
#include "mlir/IR/DialectImplementation.h"
#include "mlir/IR/OpImplementation.h"
#include "mlir/Support/LLVM.h"

// 把 tabllgen 生成的 cpp 文件导入进来
#define GET_TYPEDEF_CLASSES
#include "Dialect/NorthStar/IR/NorthStarTypes.cpp.inc"

namespace mlir::north_star {

void NorthStarDialect::registerType() {
	llvm::outs() << "register " << getDialectNamespace() << " Type\n";
	// 实现自定义类型的注册
	addTypes<
#define GET_TYPEDEF_LIST
#include "Dialect/NorthStar/IR/NorthStarTypes.cpp.inc"
    > ();
}

// 实现参数验证的接口
::llvm::LogicalResult NSTensorType::verify(
	::llvm::function_ref<::mlir::InFlightDiagnostic()> emitError,
	::llvm::ArrayRef<int64_t> shape, Type elementType, int64_t device_id) {
	if (device_id < 0) {
		return emitError() << " Invalid device id";
	}
	if (!elementType.isIntOrFloat()) {
		return emitError() << " Invalid element type ";
	}
	return llvm::success();
}

  
# 实现自定义的 assembly 解析方法，tensor的形式为 `<2x2xf32, 0>` 
Type NSTensorType::parse(AsmParser &parser) {
	// 解析左尖括号，也就是小于号 `<`
	if (parser.parseLess()) return Type();
	
	// 解析形状维度列表，并将结果存放到 dimensions 里面
	SmallVector<int64_t, 4> dimensions;
	if (parser.parseDimensionList(dimensions, 
	/*allowDynamic=*/true, // 运行动态维度 `?`
	/*withTrailingX=*/true) // 允许 `2x3x` 这样尾部 x 的格式
	) 
		return Type();
	
	// 解析元素类型，将结果存放到 elementType 里面
	auto typeLoc = parser.getCurrentLocation();
	Type elementType;
	if (parser.parseType(elementType)) return Type();
	
	// 解析 `,`
	if (parser.parseComma()) return Type();
	
	// 解析设备 id
	int device_id = 0;
	if (parser.parseInteger(device_id)) return Type()
	
	// 解析`>`
	if (parser.parseGreater()) return Type();

	return parser.getChecked<NSTensorType>(parser.getContext(), dimensions,
			elementType, device_id);
}

  
# 实现自定义的打印接口
void NSTensorType::print(AsmPrinter &printer) const {
	printer << "<";

	for (int64_t dim : getShape()) {
		if (dim < 0) {
			printer << "?" << 'x';
		} else {
			printer << dim << 'x';
		}
	}

	printer.printType(getElementType());
	printer << ",";
	printer << getDeviceId();
	printer << ">";
}
} // namespace mlir::north_star

  

#undef FIX
```

### 3、主程序
```c++

#include <cstddef>
#include "Dialect/NorthStar/IR/NorthStarDialect.h"
#include "Dialect/NorthStar/IR/NorthStarTypes.h"

#include "llvm/Support/raw_ostream.h"
#include "mlir/Dialect/GPU/IR/GPUDialect.h"
#include "mlir/IR/BuiltinAttributes.h"
#include "mlir/IR/BuiltinTypeInterfaces.h"
#include "mlir/IR/BuiltinTypes.h"
#include "mlir/IR/DialectRegistry.h"
#include "mlir/IR/MLIRContext.h"
#include "mlir/Support/LLVM.h"


void typeBrief() {

	// 文件定义：llvm-project/mlir/include/mlir/IR/BuiltinTypes.td
	auto context = new mlir::MLIRContext;

		// 浮点数，每种位宽和标准定义一个
	auto f32 = mlir::Float32Type::get(context);
	llvm::outs() << "F32类型 :\t";
	f32.dump();

	auto bf16 = mlir::BFloat16Type::get(context);
	llvm::outs() << "BF16类型 :\t";
	bf16.dump();

	// Index 类型，机器相关的整数类型
	auto index = mlir::IndexType::get(context);
	llvm::outs() << "Index 类型 :\t";
	index.dump();

	// 整数类型, 参数: 位宽&&有无符号
	auto i32 = mlir::IntegerType::get(context, 32);
	llvm::outs() << "I32 类型 :\t";
	i32.dump();

	auto ui16 = mlir::IntegerType::get(context, 16, mlir::IntegerType::Unsigned);
	llvm::outs() << "UI16 类型 :\t";
	ui16.dump();

  
	// 张量类型,表示的是数据，不会有内存的布局信息。
	auto static_tensor = mlir::RankedTensorType::get({1, 2, 3}, f32);
	llvm::outs() << "静态F32 张量类型 :\t";
	static_tensor.dump();

	// 动态张量
	auto dynamic_tensor =
	mlir::RankedTensorType::get({mlir::ShapedType::kDynamic, 2, 3}, f32);
	llvm::outs() << "动态F32 张量类型 :\t";
	dynamic_tensor.dump();

	// Memref类型：表示内存
	auto basic_memref = mlir::MemRefType::get({1, 2, 3}, f32);
	llvm::outs() << "静态F32 内存类型 :\t";
	basic_memref.dump();

	// 带有布局信息的内存
	auto stride_layout_memref = mlir::MemRefType::get(
	{1, 2, 3}, f32, mlir::StridedLayoutAttr::get(context, 1, {6, 3, 1}));
	llvm::outs() << "连续附带布局信息的 F32 内存类型 :\t";
	stride_layout_memref.dump();

	// 使用affine 表示布局信息的内存
	auto affine_memref = mlir::MemRefType::get(
	{1, 2, 3}, f32,
	mlir::StridedLayoutAttr::get(context, 1, {6, 3, 1}).getAffineMap());
	llvm::outs() << "连续附带 affine 布局信息的 F32 内存类型 :\t";
	affine_memref.dump();

	// 动态连续附带 affine 布局信息的内存
	auto dynamic_affine_memref =
	mlir::MemRefType::get({mlir::ShapedType::kDynamic, 2, 3}, f32,
	mlir::StridedLayoutAttr::get(
	context, 1, {mlir::ShapedType::kDynamic, 3, 1})
	.getAffineMap());
	llvm::outs() << "连续附带 affine 布局信息的动态 F32 内存类型 :\t";
	dynamic_affine_memref.dump();

	// 具有内存层级信息的内存
	auto L1_memref =
	mlir::MemRefType::get({mlir::ShapedType::kDynamic, 2, 3}, f32,
	mlir::StridedLayoutAttr::get(
	context, 1, {mlir::ShapedType::kDynamic, 3, 1})
	.getAffineMap(),
	1);
	llvm::outs() << "处于L1层级的 F32 内存类型 :\t";
	L1_memref.dump();

	// gpu 私有内存层级的内存
	context->getOrLoadDialect<mlir::gpu::GPUDialect>();
	auto gpu_memref =
	mlir::MemRefType::get({mlir::ShapedType::kDynamic, 2, 3}, f32,
	mlir::StridedLayoutAttr::get(
	context, 1, {mlir::ShapedType::kDynamic, 3, 1})
	.getAffineMap(),
	mlir::gpu::AddressSpaceAttr::get(
	context, mlir::gpu::AddressSpace::Private));
	llvm::outs() << "连续附带 affine 布局信息的动态 F32 Gpu Private内存类型 :\t";
	gpu_memref.dump();

  
	// 向量类型,定长的一段内存
	auto vector_type = mlir::VectorType::get(3, f32);
	llvm::outs() << "F32 1D向量类型 :\t";
	vector_type.dump();

	auto vector_2D_type = mlir::VectorType::get({3, 3}, f32);
	llvm::outs() << "F32 2D向量类型 :\t";
	vector_2D_type.dump();

	delete context;

}

  

void CH3() {
	typeBrief();
	
	// 初始化方言注册器
	mlir::DialectRegistry registry;
	
	// 初始化上下文环境
	mlir::MLIRContext context(registry);
	
	// 加载/注册方言
	auto dialect = context.getOrLoadDialect<mlir::north_star::NorthStarDialect>();
	
	// 调用方言中的方法
	dialect->sayHello();
	
	// 静态 NSTensor
	mlir::north_star::NSTensorType ns_tensor =
	mlir::north_star::NSTensorType::get(&context, {1, 2, 3},
	mlir::Float32Type::get(&context), 3);
	llvm::outs() << "North Star Tensor 类型 :\t";
	ns_tensor.dump();
	
	// 动态 NSTensor
	mlir::north_star::NSTensorType dy_ns_tensor =
	mlir::north_star::NSTensorType::get(&context,
	{mlir::ShapedType::kDynamic, 2, 3},
	mlir::Float32Type::get(&context), 3);
	llvm::outs() << "动态 North Star Tensor 类型 :\t";
	dy_ns_tensor.dump();
}

int main() { CH3(); }
```