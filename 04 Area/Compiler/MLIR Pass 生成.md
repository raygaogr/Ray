---
tags:
  - MLIR
  - Compiler
title: MLIR Pass生成
date: 2026-01-05 10:35
type: permanent-note
---
---
## 一、生成 Passes 的 CMakeLists 文件
在 [[MLIR Interfaces 生成#一、生成 Interfaces 的 CMakeLists 文件| Interfaces 的CMakeLists]]  文件的基础上进行修改
```c++
set(LLVM_TARGET_DEFINITIONS Passes.td)

mlir_tablegen(Passes.h.inc -gen-pass-decls -name NorthStarOpt)
add_public_tablegen_target(MLIRNorthStarPassesIncGen${ch_num})
add_dependencies(mlir-headers MLIRNorthStarPassesIncGen${ch_num})

add_mlir_dialect_library(MLIRNorthStarTransforms${ch_num}
						ApplyDistributeTransform.cpp
						MarkDistributeParallelParameters.cpp
						
						DEPENDS
						MLIRNorthStarPassesIncGen${ch_num}
						
						LINK_LIBS PUBLIC
						MLIRNorthStarDialect${ch_num}
)
```

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

Pass 实现有三大类型
- **Walk 函数遍历**
- **Pattern Rewrite 来进行算子重写**
- **Lowering 的流程**
```c++
namespace mlir {
namespace xxx {
#define GEN_PASS_DEF_TEMPPASS
#include "path/to/Passes.h.inc"

/************************Walk 函数流程******************************/
struct APass : APassBase<APass> {
    using APassBase<APass>::APassBase
	void runOnOperation() {
		// Step 1 获取当前操作
		auto module = getOperation();
		
		// Step 2 walk 遍历
		module->walk([&](mlir::Operation* op/*mlir::func::FuncOp op*/){
			//Step 3 创建新操作
			// ...
		
			// 删除原op
			op->erase();
		});
	}
};
/******************************************************************/


/***********************Pattern Rewrite 流程************************/
struct BPass : BPassBase<BPass> {
    using BPassBase<BPass>::BPassBase
	void runOnOperation() {
		// Step 1 获取当前操作
		auto module = getOperation();
		
		// Step 2 创建 RewritePatternSet 对象
		auto context = getContext();
		RewritePatternSet patterns(context);
		patterns.addWithLabel<xxxOpFold/*定义的匹配和重新规则*/>(StringRef("xxxOpFold"), context, 2);
		
		// Step 3 设置 Rewrite 的参数配置
		GreedyRewriteConfig xxx_config;
		xxx_config.maxIterations = 10;
		xxx_config.useTopDownTraversal = true;
		
		// Step 4 应用 Pattern 匹配对模块重写
		applyPatternGreedily(getOperation(), patterns, xxx_config);
	}
};

struct xxxOpFold : public OpRewritePattern<mlir::Dialect::xxxOp/*需要重新的Op*/> {
	virtual LogicalResult match(mlir::Dialect::xxxOp op) const {
		/**实现模式匹配规则**/
	}
	
	virtual void rewrite(mlir::Dialect::xxxOp op, PatternRewrite& rewriter) const {
		/*实现重新规则*/
		
		//常见的过程有
		rewriter.replaceAllUsesWith(); // 替换
		rewriter.eraseOp(op); // 擦除
	}


}
/******************************************************************/


/***********************Lowering 的流程*****************************/
struct CPass : CPassBase<CPass> {
    using CPassBase<CPass>::CPassBase
	void runOnOperation() {
		// Step 1 获取当前操作
		auto module = getOperation();
		
		// Step 2 进行类型转换
		TypeConverter type_convert; // 创建类型转换器
		// 添加类型转换方法
		type_convert.addConversion([](XXXType type){
			return RankedTensorType::get(type.get)
		}); 
		// 添加源物化，当需要将目标类型的值转换回源类型时使用
		type_convert.addSourceMaterialization([&](OpBuilder &builder, Type resultType, ValueRange inputs, Location loc) -> std::optional<Value> {
		
		});
		// 添加目标物化
		type_convert.addTargetMaterialization(
		[&](OpBuilder &builder, Type resultType, ValueRange inputs,
		Location loc) -> std::optional<Value> {
			if (inputs.size() != 1) return std::nullopt;
			
			return builder.create<UnrealizedConversionCastOp>(loc, resultType, inputs).getResult(0);
		});
		
		
		// Step 3 创建 RewritePatternSet 对象
		auto context = getContext();
		RewritePatternSet patterns(context);
		patterns.add<SoftmaxOpToLinalgPattern, DeviceKernelOpConvertPattern>(type_convert, context);
		
		// Step 4 创建转换目标，即哪些Dialect和Op是合法的，哪些需要被转换
		ConversionTarget target(context);
		// 声明哪些方言合法
		target.addLegalDialect<tensor::TensorDialect>();
		target.addLegalDialect<linalg::LinalgDialect>();
		// 声明哪些操作合法
		target.addLegalOp(UnrelizedConversionCastOp)();
		// 声明哪些操作是动态合法
		target.addDynamicallyLegalOp<ReturnOp>([](Return op){
			//条件1 xxxx
			return false;
			//条件2 xxxx
			return true;
		});
		
		
		// Step 5 执行转换
		applyPartialConversion(module, target, patterns);
	}
};

struct SoftmaxOpToLinalgPattern : public OpConversionPattern/*注意这里是用的opconvertion*/<xxxOp/*需要转换的op*/> {
	using OpConversionPattern::OpConversionPattern;
	LogicalResult match(north_star::SoftmaxOp op) const final {
		return llvm::success();
	}
	
	void rewrite(mlir::Dialect::xxxOp op, OpAdaptor adaptor, ConversionPatternRewriter &rewriter) const final {
		// 获取 loc
		auto loc = op->getLoc();
		// 获取 typeconverter
		auto type_converter = getTypeConverter();
		
		// 对输出结果进行转换类型
		auto res_type = converter->convertType(op.getType());
		
		// 创建新操作
		auto output = rewriter.create<linalg::SoftmaxOp>(loc, res_type, adaptor.getInput(), output, adaptor.getAxis());
		
		// 替换op
		rewriter.replaceOp(op, new_softmax);
	}
}

/******************************************************************/


}
}
```