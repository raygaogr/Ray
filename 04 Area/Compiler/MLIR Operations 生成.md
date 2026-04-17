---
tags:
  - MLIR
  - Compiler
title: MLIR Operations 生成
date: 2026-04-13 08:37
type:
---
---
## 一、生成 Operations 的 CMakeLists 文件
在 [[MLIR Attributes 生成#一、生成 Attributes 的 CMakeLists 文件| Attributes 的 CMakeLists ]] 文件的基础上，进行修改
```python
################################>>修改<<##################################
set(LLVM_TARGET_DEFINITIONS NorthStarOps.td)
#########################################################################

# 生成NorthStar Dialect 的声明
mlir_tablegen(NorthStarDialect.h.inc --gen-dialect-decls -dialect=north_star)
# 生成NorthStar Dialect 的实现
mlir_tablegen(NorthStarDialect.cpp.inc --gen-dialect-defs -dialect=north_star)

# 生成NorthStar Type 的声明
mlir_tablegen(NorthStarTypes.h.inc -gen-typedef-decls -dialect=north_star)
# 生成NorthStar Type 的实现
mlir_tablegen(NorthStarTypes.cpp.inc -gen-typedef-defs -dialect=north_star)

# 生成NorthStar Enums 的声明
mlir_tablegen(NorthStarEunms.h.inc -gen-enum-decls -dialect=north_star)
# 生成NorthStar Enums 的实现
mlir_tablegen(NorthStarEunms.cpp.inc -gen-enum-defs -dialect=north_star)

# 生成NorthStar Attr 的声明
mlir_tablegen(NorthStarAttrs.h.inc -gen-attrdef-decls -dialect=north_star)
# 生成NorthStar Attr 的实现
mlir_tablegen(NorthStarAttrs.cpp.inc -gen-attrdef-defs -dialect=north_star)

################################>>新增<<##################################
# 生成NorthStar Op 的声明
mlir_tablegen(NorthStarOps.h.inc -gen-op-decls -dialect=north_star)
# 生成NorthStar Op 的实现
mlir_tablegen(NorthStarOps.cpp.inc -gen-op-defs -dialect=north_star)
#########################################################################

# 将生成的命令们定义为为target
add_public_tablegen_target(NorthStarDialectIncGen)


add_mlir_dialect_library(MLIRNorthStarDialect$
						NorthStarDialect.cpp
						NorthStarTypes.cpp
						NorthStarAttrs.cpp
################################>>新增<<##################################						
						NorthStarOps.cpp
						#########################################################################
						DEPENDS
						NorthStarDialectIncGen${ch_num}

						LINK_LIBS PUBLIC
						MLIRIR
						MLIRTensorDialect
)
```

## 二、Operations 中 tablegen 文件的主要内容
### 1、基本概念
**Operation** 是 MLIR IR 的**基本计算单元**，类似于：
- LLVM IR 的 `Instruction`
- 传统编译器的 `Stmt` / `Expr`

```
┌─────────────────────────────────────────────────────────┐
│                    Operation 结构                        │
├─────────────────────────────────────────────────────────┤
│  操作名 (Opcode)    :  "north_star.softmax"              │
├─────────────────────────────────────────────────────────┤
│  操作数 (Operands)  :  %input                            │
├─────────────────────────────────────────────────────────┤
│  结果 (Results)     :  %output                           │
├─────────────────────────────────────────────────────────┤
│  属性 (Attributes)  :  {axis = 1 : i64}                  │
├─────────────────────────────────────────────────────────┤
│  类型 (Types)       :  (!ns_tensor) -> !ns_tensor        │
├─────────────────────────────────────────────────────────┤
│  区域 (Regions)     :  （可选，如函数体、循环体）          │
└─────────────────────────────────────────────────────────┘
```

#### 1.1 核心功能

| 功能         | 说明                      |
| ---------- | ----------------------- |
| **计算表示**   | 表达领域特定的计算（深度学习、硬件指令等）   |
| **SSA 形式** | 每个结果是一个 SSA 值，支持数据流分析   |
| **多返回值**   | 一个操作可产生多个结果（优于 LLVM IR） |
| **嵌套区域**   | 支持结构化控制流（函数、循环、条件）      |
| **属性系统**   | 编译期常量参数（如轴、padding 大小）  |

#### 1.2 MLIR 中的层级关系

```
Module (模块)
    │
    ├── FuncOp (函数操作)
    │       │
    │       ├── Region (区域)
    │       │       │
    │       │       ├── Block (基本块)
    │       │       │       │
    │       │       │       ├── Operation (操作)
    │       │       │       │       ├── SoftmaxOp
    │       │       │       │       ├── AddOp
    │       │       │       │       └── BufferOp
    │       │       │       └── Operation
    │       │       │
    │       │       └── Block
    │       │
    │       └── Region
    │
    └── FuncOp
    
// 操作可以嵌套（scf.if, func.call 等）
```

#### 1.3 Operation 的生命周期

```
1. 定义 (TableGen.td)
   └── 声明操作语义、参数、约束
   
2. 生成 (mlir-tblgen)
   └── C++ 类（SoftmaxOp）、访问器、构建器
   
3. 创建 (C++ API)
   └── builder.create<SoftmaxOp>(...)
   
4. 验证 (verify)
   └── 检查操作合法性
   
5. 优化 (Pass)
   └── 规范化、常量折叠、融合
   
6. 转换 (Lowering)
   └── 转换为更低层级的操作
   
7. 输出 (CodeGen)
   └── 生成目标代码（LLVM IR、CUDA、等）
```

#### 1.4 Operation 与 Type/Attribute 的关系

```
┌──────────────────────────────────────────────────────────┐
│                    MLIR 类型系统                          │
├──────────────────────────────────────────────────────────┤
│  Operation (计算)                                         │
│     ├── 使用 Type 定义操作数和结果的类型                    │
│     │      └── !ns_tensor<[2,3], f32, 0>                 │
│     ├── 使用 Attribute 存储编译期参数                       │
│     │      └── {axis = 1 : i64}                          │
│     └── 实现 Trait/Interface 提供通用行为                   │
│            └── InferTypeOpInterface（类型推断接口）         │
├──────────────────────────────────────────────────────────┤
│  Type (类型)     ←  描述值的性质                           │
│  Attribute (属性) ←  编译期常量值                          │
│  Trait (特性)    ←  行为约束和能力                         │
└──────────────────────────────────────────────────────────┘
```

### 2、操作的输入参数
- **`arguments`**:  定义 `Operation` 的参数列表，将固定操作数与动态属性拼接在一起
#### `(ins OperandType:$input)` —— 固定操作数

|部分|含义|
|---|---|
| `ins` |**In**put **S**pecification，输入规范|
| `OperandType` |模板参数传入的类型（如 `AnyNSTensor`）|
| `$input` |参数名，生成 `getInput()` 方法|

#### `attributes` —— 动态属性

这是**模板参数**：
`class NorthStar_UnaryOp<..., dag attributes = (ins)> //                              ↑ 默认值是空的 (ins)`

| 传入值                   | 效果           |
| --------------------- | ------------ |
| `(ins)`（默认）           | 无额外属性        |
| `(ins I64Attr:$axis)` | 添加 `axis` 属性 |
#### `regions`

`let regions = (region SizeRegion<1>:$region)`

| 关键字             | 含义                                |
| --------------- | --------------------------------- |
| `region`        | 表示开始定义 Region                     |
| `SizeRegion<1>` | 约束：该 Region 必须包含恰好 1 个 Block      |
| `$region`       | 命名这个 Region 为 `$region`，用于 c++ 访问 |
#### `hasFolder` 是否启动常量折叠的功能

#### `hasCanonicalization` 将等价的 IR 转换为标准/规范形式，简化后续优化

## 三、**Tabelgen 的写法**
```c++
#ifndef DIALECT_NORTH_STAR_OPS_TD
#define DIALECT_NORTH_STAR_OPS_TD

include "Dialect/NorthStar/IR/NorthStarAttrs.td"

// 定义一个基类，用于统一管理自定义方言的 Operation，
class NorthStar_Op<string mnemonic, list<Trait> traits = []>
: Op<NorthStar_Dialect, mnemonic, traits> {
	let summary = cppNamespace#opName#"op";
	let description = "$_name op";
}

  
// 定义一个一元操作的基类
class NorthStar_UnaryOp<string mnemonic, Type OperandType, Type resultType = OperandType, list<Trait> traits = [], dag attributes = (ins)> : NorthStar_Op<mnemonic, [DeclareOpInterfaceMethods<DistributeParallelOp>] # traits>{

	let arguments = !con((ins
						  OperandType:$input),
						  attributes);

	let results = (outs
				   resultType:$result);
}


class NorthStar_BinaryOp<string mnemonic,Type OperandType, Type resultType = OperandType, list<Trait> traits = [], dag attributes = (ins)> : NorthStar_Op<mnemonic, [DeclareOpInterfaceMethods<DistributeParallelOp>] # traits>{

	let arguments = !con((ins
					 OperandType:$lhs,
					 OperandType:$rhs),
					 attributes);
	let results = (outs
				   resultType:$result);
}

class NorthStar_ElewiseUnaryOp<string mnemonic,Type OperandType, Type resultType = OperandType, list<Trait> traits = [], dag attributes = (ins)> :
	NorthStar_UnaryOp<mnemonic, OperandType, resultType, [DeclareOpInterfaceMethods<SupportedDataParallelismOp>] # traits, attributes>;

class NorthStar_ElewiseBinaryOp<string mnemonic,Type OperandType, Type resultType = OperandType,list<Trait> traits = [], dag attributes = (ins)>:
	NorthStar_BinaryOp<mnemonic, OperandType, resultType, [DeclareOpInterfaceMethods<SupportedDataParallelismOp>] # traits, attributes>;

def NorthStar_ConstOp : NorthStar_Op<"const", []>{
	let arguments = (ins
	 				 ElementsAttr:$value);
	let results = (outs
				   AnyNSTensor:$result);
}

  
// 代表 SoftmaxOp 需要显示地实现 SupportedDataParallelismOp 的两个接口方法
def NorthStar_SoftmaxOp : NorthStar_UnaryOp<"softmax", AnyNSTensor, AnyNSTensor, [DeclareOpInterfaceMethods<SupportedDataParallelismOp,["applyDataParallelism", "supportedDataParallelism"]>], (ins I64Attr:$axis)>{
	let hasVerifier = 1;
	let builders = [
		OpBuilder<(ins "::mlir::Value":$input, "int64_t":$axis),
				[{
					$_state.addOperands(input);
					$_state.getOrAddProperties<Properties>().axis = $_builder.getIntegerAttr(odsBuilder.getIntegerType(64,true), axis);
					$_state.addTypes(input.getType());
				}]>
	];
}

// ExpOp 使用默认实现
def NorthStar_ExpOp : NorthStar_ElewiseUnaryOp<"exp",AnyNSTensor>{
	let builders = [
		OpBuilder<(ins "::mlir::Value":$input) ,
			[{
				$_state.addOperands(input);
				$_state.addTypes(input.getType());
			}]>
	];
}

  
def NorthStar_AddOp : NorthStar_ElewiseBinaryOp<"add",AnyNSTensor>;
def NorthStar_SubOp : NorthStar_ElewiseBinaryOp<"sub",AnyNSTensor>;
def NorthStar_MulOp : NorthStar_ElewiseBinaryOp<"mul",AnyNSTensor>;
def NorthStar_DivOp : NorthStar_ElewiseBinaryOp<"div",AnyNSTensor>;

  
def NorthStar_AllToAllOp : NorthStar_Op<"all_to_all",[]>{
	let arguments = (ins
					AnyBuffer:$input,
					AnyBuffer:$output
	);
}

def NorthStar_BufferCastOp : NorthStar_Op<"buffer_cast",[]>{
	let description = "对数据切分的标记";
	let arguments = (ins
		Variadic<NSTensorOrBuffer>:$inputs,
		DistributeParallelAttr:$distribute_attr
	);
	
	let results = (outs Variadic<NSTensorOrBuffer>:$outputs);
	
	let hasVerifier = 1;
}

  
def NorthStar_BufferOp : NorthStar_Op<"buffer",[]>{
	let description = "将 多个device_id 的tensor 组合成 一个 buffer";
	let arguments = (ins Variadic<AnyNSTensor>:$tensors);
	
	let results = (outs AnyBuffer:$result);
	
	let hasVerifier = 1;
	
	let builders = [
		OpBuilder<(ins "::mlir::ValueRange":$tensors) ,
		[{
			$_state.addOperands(tensors);
			::llvm::SmallVector<int64_t> devices;
			for (auto tensor : tensors) {
				auto tensor_type =
				llvm::cast<::mlir::north_star::NSTensorType>(tensor.getType());
				devices.push_back(tensor_type.getDeviceId());
			}
			$_state.addTypes(::mlir::north_star::BufferType::get
		    ($_state.getContext(), devices));
		}]>
	];
}

  
def NorthStar_GetTensorOp: NorthStar_Op<"get_tensor",[]>{

	let description = "从buffer中取出指定device_id的tensor";
	let arguments = (ins
					AnyBuffer:$buffer,
					I64Attr:$device_id);

	let results = (outs
				AnyNSTensor:$result);
				let hasVerifier = 1;
}


def NorthStar_PrintOp: NorthStar_Op<"print",[]>{
	let arguments = (ins
	AnyType:$input
	);
}

#endif // INCLUDE_LLCOMPILER_DIALECT_NorthStar_IR_LLHOPS_TD_
```

## 四、Operations 中 C++ 代码
### 1、头文件
```c++
#ifndef DIALECT_NORTH_STAR_OPS_H
#define DIALECT_NORTH_STAR_OPS_H

#include "Dialect/NorthStar/IR/NorthStarTypes.h"

#define GET_OP_CLASSES
#include "Dialect/NorthStar/IR/NorthStarOps.h.inc"

#endif // DIALECT_NORTH_STAR_OPS_H
```

### 2、源文件
```c++
#include "Dialect/NorthStar/IR/NorthStarOps.h"
#include <algorithm>

#include "Dialect/NorthStar/IR/NorthStarDialect.h"
#include "Dialect/NorthStar/IR/NorthStarTypes.h"

#include "llvm/ADT/STLExtras.h"
#include "llvm/Support/Casting.h"
#include "llvm/Support/LogicalResult.h"
#include "mlir/IR/Value.h"

#define GET_OP_CLASSES
#include "Dialect/NorthStar/IR/NorthStarOps.cpp.inc"

namespace mlir::north_star {

void NorthStarDialect::registerOps() {
llvm::outs() << "register " << getDialectNamespace() << " Op\n";
	addOperations<
#define GET_OP_LIST
#include "Dialect/NorthStar/IR/NorthStarOps.cpp.inc"
	>();
}

::llvm::LogicalResult GetTensorOp::verify() {

	auto device_id = getDeviceId();
	auto buffer = getBuffer();
	if (isa<BlockArgument>(buffer)) {
		auto buffer_type = cast<BufferType>(buffer.getType());
		auto device_ids = buffer_type.getDevices();
		for (auto id : device_ids) {
			if (id == device_id) return llvm::success();
		}
		return llvm::failure();
	}

	auto buffer_op = llvm::cast_or_null<BufferOp>(buffer.getDefiningOp());
	if (!buffer_op) return llvm::failure();
	
	for (auto tensor : buffer_op.getTensors()) {
		auto tensor_type = cast_or_null<NSTensorType>(tensor.getType());
		if (!tensor_type) return llvm::failure();
		if (device_id == tensor_type.getDeviceId()) {
			if (tensor_type != getType()) return llvm::failure();
			return llvm::success();
		}
	}
	return llvm::failure();
};

  
::llvm::LogicalResult BufferOp::verify() {
	auto tensors = getTensors();
	auto devices = cast<BufferType>(getType()).getDevices();

	if (tensors.size() == 0) return llvm::failure();

	for (auto [index, device_id, tensor] : llvm::enumerate(devices, tensors)) {
		auto tensor_type = cast_or_null<NSTensorType>(tensor.getType());
		if (device_id != tensor_type.getDeviceId()) return llvm::failure();
	}
	return llvm::success();
}

  

::llvm::LogicalResult SoftmaxOp::verify() {
	auto axis = getAxis();
	if (axis < 0) return llvm::failure();
	auto input_type = cast<NSTensorType>(getInput().getType());
	if (axis >= input_type.getShape().size()) return llvm::failure();
	return llvm::success();
}

} // namespace mlir::north_star
```

### 3、主程序
```c++
#include <cstddef>
#include <cstdint>

#include "Dialect/NorthStar/IR/NorthStarAttrs.h"
#include "Dialect/NorthStar/IR/NorthStarDialect.h"
#include "Dialect/NorthStar/IR/NorthStarOps.h"
#include "Dialect/NorthStar/IR/NorthStarTypes.h"

#include "llvm/ADT/APFloat.h"
#include "llvm/ADT/ArrayRef.h"
#include "llvm/ADT/SmallVector.h"
#include "llvm/Support/Casting.h"
#include "llvm/Support/raw_ostream.h"
#include "mlir/Dialect/Func/IR/FuncOps.h"
#include "mlir/Dialect/GPU/IR/GPUDialect.h"
#include "mlir/IR/Block.h"
#include "mlir/IR/Builders.h"
#include "mlir/IR/BuiltinAttributes.h"
#include "mlir/IR/BuiltinOps.h"
#include "mlir/IR/BuiltinTypeInterfaces.h"
#include "mlir/IR/BuiltinTypes.h"
#include "mlir/IR/DialectRegistry.h"
#include "mlir/IR/MLIRContext.h"
#include "mlir/IR/OperationSupport.h"
#include "mlir/IR/ValueRange.h"
#include "mlir/Support/LLVM.h"


void CH5() {

	// 初始化方言注册器
	mlir::DialectRegistry registry;
	
	// 初始化上下文环境
	mlir::MLIRContext context(registry);
	
	// 加载/注册方言
	context.getOrLoadDialect<mlir::north_star::NorthStarDialect>();

	mlir::OpBuilder builder(&context);
	auto loc = builder.getUnknownLoc();

    // 创建MLIR的顶层容器，相当于一个文件或编译单元
	// ModuleOp
	auto module = builder.create<mlir::ModuleOp>(loc, "NorthStar");
	builder.setInsertionPointToStart(module.getBody());

	// ConstOp
	auto f32 = mlir::Float32Type::get(&context);
	auto shape = mlir::SmallVector<int64_t>({2, 2});
	auto const_value_1 =
	mlir::SmallVector<llvm::APFloat>(4, llvm::APFloat((float)1));
	auto const_value_2 =
	mlir::SmallVector<llvm::APFloat>(4, llvm::APFloat((float)2));
	auto tensor_type_1 =
	mlir::north_star::NSTensorType::get(&context, shape, f32, 0);
	auto tensor_type_2 =
	mlir::north_star::NSTensorType::get(&context, shape, f32, 1);
	auto const_1 = builder.create<mlir::north_star::ConstOp>(
	loc, tensor_type_1, mlir::DenseElementsAttr::get(mlir::RankedTensorType::get(shape, f32), const_value_1));
	auto const_2 = builder.create<mlir::north_star::ConstOp>(
	loc, tensor_type_1,
	mlir::DenseElementsAttr::get(mlir::RankedTensorType::get(shape, f32), const_value_1));
	auto const_3 = builder.create<mlir::north_star::ConstOp>(
	loc, tensor_type_2,
	mlir::DenseElementsAttr::get(mlir::RankedTensorType::get(shape, f32), const_value_2));
	auto const_4 = builder.create<mlir::north_star::ConstOp>(
	loc, tensor_type_2,
	mlir::DenseElementsAttr::get(mlir::RankedTensorType::get(shape, f32),const_value_2));
	llvm::outs() << "Const tensor in divece 0 :\n";
	const_1->dump();
	llvm::outs() << "Const tensor in divece 1 :\n";
	const_3->dump();

	// Buffer Op
	auto buffer_op = builder.create<mlir::north_star::BufferOp>(
	loc, mlir::ValueRange({const_1, const_3}));
	llvm::outs() << "Buffer Op :\n";
	buffer_op->dump();

	// Get Tensor Op
	auto get_tensor_op_1 = builder.create<mlir::north_star::GetTensorOp>(
	loc, tensor_type_1, buffer_op, 0);
	auto get_tensor_op_2 = builder.create<mlir::north_star::GetTensorOp>(
	loc, tensor_type_2, buffer_op, 1);
	llvm::outs() << "Get Tensor Op :\n";
	get_tensor_op_1->dump();
	get_tensor_op_2->dump();
	
	// Softmax Op
	auto softmax_op =
	builder.create<mlir::north_star::SoftmaxOp>(loc, get_tensor_op_1, 1);
	llvm::outs() << "Softmax Op :\n";
	softmax_op->dump();

	// Exp Op
	auto exp_op = builder.create<mlir::north_star::ExpOp>(loc, get_tensor_op_2);
	llvm::outs() << "Exp Op :\n";
	exp_op->dump();
	
	// all to all op
	auto out_buffer_op = builder.create<mlir::north_star::BufferOp>(
	loc, mlir::ValueRange({const_2, const_4}));
	
	auto all_to_all_op = builder.create<mlir::north_star::AllToAllOp>(loc, buffer_op, out_buffer_op);
	llvm::outs() << "All to All Op :\n";
	all_to_all_op->dump();

}

int main() { CH5(); }
```