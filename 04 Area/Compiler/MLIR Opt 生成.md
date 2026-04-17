---
tags:
  - MLIR
  - Compiler
title: MLIR Opt 生成
date: 2026-04-14 16:25
type:
---
---
## 一、生成 Operations 的 CMakeLists 文件
在 [[MLIR Pass 生成#一、生成 Passes 的 CMakeLists 文件| Passes CMakeLists]] 文件的基础上，进行添改
```python
set(LIBS
	${dialect_libs}
	${conversion_libs}
	${extension_libs}

	MLIRAffineAnalysis
	MLIRAnalysis
	MLIRCastInterfaces
	MLIRDialect
	MLIROptLib
	MLIRParser
	MLIRPass
	MLIRTransforms
	MLIRTransformUtils
	MLIRSupport
	MLIRIR
	MLIRToLLVMIRTranslationRegistration
	
	MLIRNorthStarDialect${ch_num}
	MLIRNorthStarTransforms${ch_num}
	MLIRTutorialUtils${ch_num}
	
	MLIRCAPIDebug
)

# 生成 NS-opt 用于 mlir 文件的优化
add_mlir_tool(NS-opt${ch_num}
			NS-opt.cpp
		
			DEPENDS
			${LIBS}
)

target_link_libraries("NS-opt${ch_num}" PRIVATE ${LIBS})

llvm_update_compile_flags(NS-opt${ch_num})
mlir_check_all_link_libraries(NS-opt${ch_num})
export_executable_symbols_for_plugins(NS-opt${ch_num})

```

## 四、MLIR-Opt 中 C++ 代码
```c++

#include "Dialect/NorthStar/Transforms/Passes.h"
#include "Dialect/NorthStar/IR/NorthStarDialect.h"

#include "llvm/Support/CommandLine.h"
#include "llvm/Support/InitLLVM.h"
#include "llvm/Support/SourceMgr.h"
#include "llvm/Support/ToolOutputFile.h"
#include "llvm/Support/raw_ostream.h"
#include "mlir/Config/mlir-config.h"
#include "mlir/IR/AsmState.h"
#include "mlir/IR/Dialect.h"
#include "mlir/IR/MLIRContext.h"
#include "mlir/InitAllDialects.h"
#include "mlir/InitAllExtensions.h"
#include "mlir/InitAllPasses.h"
#include "mlir/Pass/Pass.h"
#include "mlir/Pass/PassManager.h"
#include "mlir/Support/FileUtilities.h"
#include "mlir/Target/LLVMIR/Dialect/All.h"
#include "mlir/Tools/mlir-opt/MlirOptMain.h"
#include "mlir-c/Debug.h"

  
int main(int argc, char **argv) {

	mlir::registerAllPasses();
	mlir::DialectRegistry registry;
	registerAllDialects(registry);
	// 注册自定义方言
	registry.insert<mlir::north_star::NorthStarDialect>();
	registerAllExtensions(registry);
	// 注册自定义的Pass
	mlir::north_star::registerNorthStarOptPasses();
	
	// mlirEnableGlobalDebug(true);
	// 
	return mlir::asMainReturnCode(mlir::MlirOptMain(argc, argv, "NS modular optimizer driver\n", registry));
}

```