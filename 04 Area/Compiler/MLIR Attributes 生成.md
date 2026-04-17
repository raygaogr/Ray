---
tags:
  - MLIR
  - Compiler
title: MLIR Attributes 和 Types 生成
date: 2026-01-05 17:25
type: permanent-note
---
## 一、生成 Attributes 的 CMakeLists 文件
在 [[MLIR Types 生成#一、生成 Types 的 CMakeLists 文件| Types 的 CMakeLists]] 文件的基础上，进行修改
```python
################################>>修改<<##################################
set(LLVM_TARGET_DEFINITIONS NorthStarAttrs.td)
#########################################################################


# 生成NorthStar Dialect 的声明
mlir_tablegen(NorthStarDialect.h.inc --gen-dialect-decls -dialect=north_star)

# 生成NorthStar Dialect 的实现
mlir_tablegen(NorthStarDialect.cpp.inc --gen-dialect-defs -dialect=north_star)

# 生成NorthStar Type 的声明
mlir_tablegen(NorthStarTypes.h.inc -gen-typedef-decls -dialect=north_star)
# 生成NorthStar Type 的实现
mlir_tablegen(NorthStarTypes.cpp.inc -gen-typedef-defs -dialect=north_star)


################################>>新增<<##################################
# 生成NorthStar Enums 的声明
mlir_tablegen(NorthStarEunms.h.inc -gen-enum-decls -dialect=north_star)
# 生成NorthStar Enums 的实现
mlir_tablegen(NorthStarEunms.cpp.inc -gen-enum-defs -dialect=north_star)

# 生成NorthStar Attr 的声明
mlir_tablegen(NorthStarAttrs.h.inc -gen-attrdef-decls -dialect=north_star)
# 生成NorthStar Attr 的实现
mlir_tablegen(NorthStarAttrs.cpp.inc -gen-attrdef-defs -dialect=north_star)
#########################################################################

# 将生成的命令们定义为为target
add_public_tablegen_target(NorthStarDialectIncGen${ch_num})

add_mlir_dialect_library(MLIRNorthStarDialect${ch_num}
						NorthStarDialect.cpp
						NorthStarTypes.cpp
################################>>新增<<##################################
						NorthStarAttrs.cpp
#########################################################################
						DEPENDS
						NorthStarDialectIncGen${ch_num}

						LINK_LIBS PUBLIC
						MLIRIR
						MLIRTensorDialect
)
```


## 二、Attributes 中 tablegen 文件的主要内容

**Attributes(属性)**
- 定义**编译时常量数据**（compile-time constant metadata）
- 编译时存在，不参与运行时计算
- 例如：常量值、符号引用、配置参数、优化提示

### 1、汇编格式
- **`assemblyFormat `**: 定义文本的序列化格式

### 2、自定义的类声明
- **`extraClassDeclaration`**: 向生成的类注入自定义的成员函数
### 3、输入参数
- **`parameters`**: 参考 [[MLIR Types 生成#2、类型的参数| Types 的参数]]


## 三、**Tabelgen 的写法
```c++
#ifndef DIALECT_NORTH_STAR_ATTRS_TD
#define DIALECT_NORTH_STAR_ATTRS_TD


include "mlir/IR/EnumAttr.td"
include "Dialect/NorthStar/IR/NorthStarEunms.td"
include "Interfaces/DistributeParallelismInterfaces.td"
  
class NorthStar_Attr<string name, string attrMnemonic, list<Trait> traits = [], string baseCppClass = "::mlir::Attribute">
: AttrDef<NorthStar_Dialect, name, traits, baseCppClass> {

	let mnemonic = attrMnemonic;
	let attrName = dialect.name # "." # attrMnemonic;
	let genStorageClass = 1;
	let hasStorageCustomConstructor = 0;
	let skipDefaultBuilders = 0;
	let genVerifyDecl = 0;
}

  

def NorthStar_DataParallelism: NorthStar_Attr<"DataParallelism", "DP", [DataParallelAttr]>{
	let parameters = (ins "int64_t":$DP_nums, ArrayRefParameter<"int64_t">:$devices);
	let builders = [
		AttrBuilder<(ins "int64_t":$DP_nums),
			[{
				llvm::SmallVector<int64_t> device_ids;
				for (auto i : llvm::index_range(0, DP_nums)) {
					device_ids.push_back(i);
				}
				return $_get($_ctxt, DP_nums, device_ids);
			}]
		>		
		];
		
	let assemblyFormat = [{
		`<`
		`DP` `=` $DP_nums `:` $devices
		`>`
	}];

}

#endif //DIALECT_NORTH_STAR_ATTRS_TD
```

## 四、Types 中 C++ 代码
### 1、头文件
```c++
#ifndef DIALECT_NORTH_STAR_ATTRS_H
#define DIALECT_NORTH_STAR_ATTRS_H

#include "Dialect/NorthStar/IR/NorthStarEunms.h"
#include "mlir/Dialect/Tensor/IR/Tensor.h"
#include "mlir/IR/MLIRContext.h"

// 导入 tablegen 生成的头文件
#define GET_ATTRDEF_CLASSES
#include "Dialect/NorthStar/IR/NorthStarAttrs.h.inc"

#include "Dialect/NorthStar/IR/NorthStarEunms.h.inc"

#endif // DIALECT_NORTH_STAR_ATTRS_H
```

### 2、源文件
```c++
#include "Dialect/NorthStar/IR/NorthStarAttrs.h"
#include "Dialect/NorthStar/IR/NorthStarDialect.h"
#include "Dialect/NorthStar/IR/NorthStarEunms.h"

#include "llvm/ADT/TypeSwitch.h"
#include "llvm/Support/LogicalResult.h"
#include "llvm/Support/raw_ostream.h"
#include "mlir/Dialect/Arith/IR/Arith.h"
#include "mlir/IR/BuiltinTypeInterfaces.h"
#include "mlir/IR/DialectImplementation.h"
#include "mlir/IR/OpImplementation.h"
#include "mlir/Support/LLVM.h"


#define GET_ATTRDEF_CLASSES
#include "Dialect/NorthStar/IR/NorthStarAttrs.cpp.inc"
#include "Dialect/NorthStar/IR/NorthStarEunms.cpp.inc"

namespace mlir::north_star {

void NorthStarDialect::registerAttrs() {
	llvm::outs() << "register " << getDialectNamespace() << " Attr\n";
// 将属性注入到方言中
	addAttributes<
#define GET_ATTRDEF_LIST
#include "Dialect/NorthStar/IR/NorthStarAttrs.cpp.inc"
	>();
}

bool LayoutAttr::isChannelLast() { return getValue() == Layout::NHWC; }

} // namespace mlir::north_star
```

### 3、主函数
```c++
#include <cstddef>

#include "Dialect/NorthStar/IR/NorthStarAttrs.h"
#include "Dialect/NorthStar/IR/NorthStarDialect.h"
#include "Dialect/NorthStar/IR/NorthStarTypes.h"

#include "llvm/ADT/APFloat.h"
#include "llvm/ADT/ArrayRef.h"
#include "llvm/ADT/SmallVector.h"
#include "llvm/Support/raw_ostream.h"
#include "mlir/Dialect/GPU/IR/GPUDialect.h"
#include "mlir/IR/BuiltinAttributes.h"
#include "mlir/IR/BuiltinTypeInterfaces.h"
#include "mlir/IR/BuiltinTypes.h"
#include "mlir/IR/DialectRegistry.h"
#include "mlir/IR/MLIRContext.h"
#include "mlir/Support/LLVM.h"


void attributeBrief() {

	auto context = new mlir::MLIRContext;
	context->getOrLoadDialect<mlir::north_star::NorthStarDialect>();
	
	
	// Float Attr 表示浮点数的Attribute
	auto f32_attr = mlir::FloatAttr::get(mlir::Float32Type::get(context), 2);
	llvm::outs() << "F32 Attribute :\t";
	f32_attr.dump();

	// Integer Attr 表示整数的Attribute
	auto i32_attr =
	mlir::IntegerAttr::get(mlir::IntegerType::get(context, 32), 10);
	llvm::outs() << "I32 Attribute :\t";
	i32_attr.dump();
	
	// StrideLayout Attr 表示内存布局信息的Attribute
	auto stride_layout_attr = mlir::StridedLayoutAttr::get(context, 1, {6, 3, 1});
	llvm::outs() << "StrideLayout Attribute :\t";
	stride_layout_attr.dump();
	
	// String Attr 表示字符串的Attribute
	auto str_attr = mlir::StringAttr::get(context, "Hello, MLIR!");
	llvm::outs() << "String Attribute :\t";
	str_attr.dump();
	
	// StrRef Attr 表示符号的Attribute
	auto str_ref_attr = mlir::SymbolRefAttr::get(str_attr);
	llvm::outs() << "SymbolRef Attribute :\t";
	str_ref_attr.dump();
	
	// Type Attr 储存Type 的Attribute
	auto type_attr = mlir::TypeAttr::get(mlir::north_star::NSTensorType::get(
	context, {1, 2, 3}, mlir::Float32Type::get(context)));
	llvm::outs() << "Type Attribute :\t";
	type_attr.dump();
	
	  
	// Unit Attr 一般作为标记使用
	auto unit_attr = mlir::UnitAttr::get(context);
	llvm::outs() << "Unit Attribute :\t";
	unit_attr.dump();
	
	auto i64_arr_attr = mlir::DenseI64ArrayAttr::get(context, {1, 2, 3});
	llvm::outs() << "Array Attribute :\t";
	i64_arr_attr.dump();
	
	auto dense_attr = mlir::DenseElementsAttr::get(
	mlir::RankedTensorType::get({2, 2}, mlir::Float32Type::get(context)),
	llvm::ArrayRef<float>{1, 2, 3, 4});
	llvm::outs() << "Dense Attribute :\t";
	dense_attr.dump();
	
	delete context;
}

  

void CH4() {
	
	attributeBrief();
	
	// 初始化方言注册器
	mlir::DialectRegistry registry;
	
	// 初始化上下文环境
	mlir::MLIRContext context(registry);
	
	// 加载/注册方言
	auto dialect = context.getOrLoadDialect<mlir::north_star::NorthStarDialect>();
	
	// Layout Eunms
	auto nchw = mlir::north_star::Layout::NCHW;
	llvm::outs() << "NCHW: " << mlir::north_star::stringifyEnum(nchw) << "\n";
	
	// LayoutAttr
	auto nchw_attr = mlir::north_star::LayoutAttr::get(&context, nchw);
	llvm::outs() << "NCHW LayoutAttribute :\t";
	nchw_attr.dump();
	
	// DataParallelismAttr
	auto dp_attr = mlir::north_star::DataParallelismAttr::get(&context, 2);
	llvm::outs() << "DataParallelism Attribute :\t";
	dp_attr.dump();

}

int main() { CH4(); }
```
## 五、Attributes vs Types：使用场景对比

|特性|Attributes|Types|
|---|---|---|
|**用途**|编译时常量、元数据|运行时值的类型|
|**示例**|常量42、符号名"foo"、配置参数|i32、f64、tensor<4xf32>|
|**关联**|附加到操作、块参数|定义SSA值的种类|
|**可变性**|不可变|通常不可变（需显式标记可变）|
|**默认基类**|`::mlir::Attribute`|`::mlir::Type`|
|**命名约定**|`NameAttr`|`NameType`|
|**语法前缀**|`#dialect.attr`|`!dialect.type`|




