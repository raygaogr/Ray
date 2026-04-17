---
tags:
  - MLIR
  - Compiler
title: MLIR Interfaces 生成
date: 2026-04-13 14:05
type:
---
---
## 一、生成 Interfaces 的 CMakeLists 文件
在 [[MLIR Operations 生成#一、生成 Operations 的 CMakeLists 文件| Operations 的 CMakeLists]] 文件的基础上进行修改
```python
set(LLVM_TARGET_DEFINITIONS DistributeParallelismInterfaces.td)

mlir_tablegen(DistributeParallelismOpInterfaces.h.inc -gen-op-interface-decls)
mlir_tablegen(DistributeParallelismOpInterfaces.cpp.inc -gen-op-interface-defs)
mlir_tablegen(DistributeParallelismAttrInterfaces.h.inc -gen-attr-interface-decls)
mlir_tablegen(DistributeParallelismAttrInterfaces.cpp.inc -gen-attr-interface-defs)

add_public_tablegen_target(MLIRDistributeParallelismInterfacesIncGen)

add_mlir_library(MLIRDistributeParallelismInterfaces
				DistributeParallelismInterfaces.cpp
				
				DEPENDS
				MLIRDistributeParallelismInterfacesIncGen${ch_num}

				LINK_LIBS PUBLIC
				MLIRIR
)
```

## 二、Operations 中 tablegen 文件的主要内容

Interface 是 MLIR 中的**多态机制**，允许为不同的 **Operation** 或 **Attribute** 定义统一的行为接口，而**不需要继承共同的基类**。
### 1、核心思想

```
传统 OOP:          MLIR Interface:
┌──────────┐        ┌─────────────┐
│ 基类     │        │ Interface   │◄────────┐
│  ├ 方法A │        │  ├ 方法A    │         │
│  ├ 方法B │        │  └ 方法B    │         │
└────┬─────┘        └─────────────┘         │
     │                    ▲                 │
  继承│                 实现│                 │
     │                    │                 │
┌────▼────┐   ┌───────────┴─────────┐       │
│OpA      │   │ OpA    OpB    OpC   │       │
│OpB      │   │  └─────┬─────┘      │       │
│OpC      │   │    各自实现接口方法   │       │
└─────────┘   └─────────────────────┘       │
                                            │
                Attribute 也能实现 Interface─┘
```

---

### 2、Interface 的两种类型

| 类型                | 说明                 | 基类                               |
| ----------------- | ------------------ | -------------------------------- |
| **OpInterface**   | 给 Operation 添加通用行为 | `OpInterface<"InterfaceName">`   |
| **AttrInterface** | 给 Attribute 添加通用行为 | `AttrInterface<"InterfaceName">` |
### 3、接口方法
- **`methods`**: 定义一组接口方法，使用 `InterfaceMethod</*desc=*/, /*returnType*/, /*methodName*/, /*接口参数(ins)*/, /*代码块，方法的主体*/, /*默认实现*/>` 语法来定义

## 三、**Tabelgen 的写法**
```python

#ifndef INTERFACES_DISTRIBUTE_PARALLELISM_INTERFACES_TD
#define INTERFACES_DISTRIBUTE_PARALLELISM_INTERFACES_TD

include "mlir/IR/Interfaces.td"
  

def DistributeParallelAttr: AttrInterface<"DistributeParallelAttr">{
	let description = "Properties related to distribute parallelism";
	let cppNamespace = "::mlir";
	let methods = [];
	let extraClassDeclaration = "";
	let extraSharedClassDeclaration = "";
}

  

def DataParallelAttr: AttrInterface<"DataParallelAttr",[DistributeParallelAttr]>{

	let description = "Properties related to distribute parallelism";
	let cppNamespace = "::mlir";
	
	let methods = [
		InterfaceMethod<[{
		DP 数量.
		}],
		"int64_t", "getDPNums">,
	
		InterfaceMethod<[{
		设备编号.
		}],
		"::llvm::ArrayRef<int64_t>", "getDevices">,
	];
	
	let extraClassDeclaration = "";
	let extraSharedClassDeclaration = "";
}

  

def SupportedDataParallelismOp: OpInterface<"SupportedDataParallelismOp">{

	let description = "Properties related to data parallelism";
	let cppNamespace = "::mlir";
	
	let methods = [
		InterfaceMethod<
			/*desc=*/ "进行数据并行的变换",
			/*returnType=*/ "::mlir::LogicalResult",
			/*methodName=*/ "applyDataParallelism",
			/*args=*/ (ins "::mlir::DistributeParallelAttr":$attr),
			/*methodBody=*/ "",
			/*defaultImpl=*/ [{
			return llvm::failure();
			}]
	>	,
	
		InterfaceMethod<
			/*desc=*/ "进行数据并行的变换",
			/*returnType=*/ "bool",
			/*methodName=*/ "supportedDataParallelism",
			/*args=*/ (ins),
			/*methodBody=*/ "",
			/*defaultImpl=*/ [{
				Operation* op = $_op.getOperation();
				if (op->getNumOperands() == 0) return true;
				auto base_type = op->getOperand(0).getType();
				if (!isa<mlir::ShapedType>(base_type)) return false;
				for (auto operand : op->getOperands()) {
					if (operand.getType() != base_type) return false;
				}
				return true;
			}]
		>		
	];

	let extraClassDeclaration = "";
	let extraSharedClassDeclaration = "";
}

  

def DistributeParallelOp:OpInterface<"DistributeParallelOp">{

	let description = "Properties related to distribute parallelism";
	let cppNamespace = "::mlir";
	let methods = [];
	
	let extraClassDeclaration = "";
	let extraSharedClassDeclaration = [{
		// 实现并行变换
		::mlir::LogicalResult applyDistributeParallelism(const ::mlir::DistributeParallelAttr attr){
		if (isa<mlir::DataParallelAttr>(attr)) {
			if (!isa<mlir::SupportedDataParallelismOp>($_op.getOperation())) return ::llvm::failure();
			return dyn_cast<mlir::SupportedDataParallelismOp>($_op.getOperation()).applyDataParallelism(attr);
		} else {
			llvm_unreachable("unsupported parallel type!");
		}
			return ::llvm::failure();
		};

		bool supportedDistributeParallelism(){
			if (isa<mlir::SupportedDataParallelismOp>($_op.getOperation())){
				return dyn_cast<mlir::SupportedDataParallelismOp>($_op.getOperation()).supportedDataParallelism();
		}else{
			llvm_unreachable("unsupported parallel type!");
		}
			return false;
		}
	}];
}

#endif // INTERFACES_DISTRIBUTE_PARALLELISM_INTERFACES_TD
```

## 四、Interfaces 中 C++ 代码

### 1、头文件
```c++
#ifndef DIALECT_NORTH_STAR_ATTRS_H
#define DIALECT_NORTH_STAR_ATTRS_H

#include "Dialect/NorthStar/IR/NorthStarEunms.h"
#include "mlir/Dialect/Tensor/IR/Tensor.h"

#include "mlir/IR/MLIRContext.h"
#include "Interfaces/DistributeParallelismInterfaces.h"

#define GET_ATTRDEF_CLASSES
#include "Dialect/NorthStar/IR/NorthStarAttrs.h.inc"

#endif // DIALECT_NORTH_STAR_ATTRS_H
```