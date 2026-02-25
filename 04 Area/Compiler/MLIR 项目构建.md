---
tags:
  - MLIR
  - compiler
title: MLIR 项目构建
date: 2026-01-05 08:48
type: permanent-note
---
---
## CMake 构建
### 1、项目的层次结构
一个 MLIR 项目通常包含以下的层次结构：
```python
├──include/
|  ├──Dialect/
|  |  ├──Dialect1/
|  |  |  └──Dialect1.h
|  |  ├──Dialect2/
|  |  |  └──Dialect2.h
|  |  ├──Passes.h
|  |  ├──Passes.td
|  |  └──CMakeLists.txt
|  ├──Interfaces/
|  |  ├──Interface1.h
|  |  ├──Interface1.td
|  |  ├──Interface2.h
|  |  ├──Interface2.td
|  |  └──CMakeLists.txt
|  └──Transforms/
|  |  ├──Transform1/
|  |  |  └──Transform1.h
|  |  ├──Transform2/
|  |  |  └──Transform2.h
|  |  ├──Passes.h
|  |  ├──Passes.td
|  |  └──CMakeLists.txt
|  └──CMakeLists.txt
└──src/
|  ├──Dialect/
|  |  ├──Dialect1/
|  |  |  └──Dialect1.cpp
|  |  ├──Dialect2/
|  |  |  └──Dialect2.cpp
|  |  └──CMakeLists.txt
|  ├──Interfaces/
|  |  ├──Interface1.cpp
|  |  ├──Interface2.cpp
|  |  └──CMakeLists.txt
|  └──Transforms/
|  |  ├──Transform1/
|  |  |  └──Transform1.h
|  |  ├──Transform2/
|  |  |  └──Transform2.h
|  |  └──CMakeLists.txt
|  └──CMakeLists.txt  
└──CMakeLists.txt
```
### 2、根目录下的 CMakeLists 文件
```python
cmake_minimum_required(VERSION 3.20.0)
project(name1 LANGUAGES CXX C)

set(CMAKE_CXX_STANDARD 17)

set(LLVM_DIR /path/to/llvmConfig.cmake)
set(MLIR_DIR /path/to/mlirConfig.cmake)
find_package(MLIR REQUIRED CONFIG)
list(APPEND CMAKE_MODULE_PATH ${LLVM_CMAKE_DIR})
list(APPEND CMAKE_MODULE_PATH ${MLIR_CMAKE_DIR})

include(TableGen)
include(AddLLVM)
include(AddMLIR)
include_directories(${LLVM_INCLUDE_DIRS})
include_directories(${MLIR_INCLUDE_DIRS})

get_property(dialect_libs GLOBAL PROPERTY MLIR_DIALECT_LIBS)
get_property(conversion_libs GLOBAL PROPERTY MLIR_CONVERSION_LIBS)
get_property(extension_libs GLOBAL PROPERTY MLIR_EXTENSION_LIBS)

add_subdirectory(include)
add_subdirectory(src)
```
### 3、Include 目录下的 CMakeLists 文件
```python
set(LLVM_TARGET_DEFINITIONS temp.td)
// 生成Pass相关
mlir_tablegen(temp.h.inc -gen-pass-decls -name temp) 
add_public_tabelgen_target(MLIRTempPassesIncGen) // 构建target
add_dependencies(mlir-headers MLIRTempPassesIncGen) // 确保依赖mlir-headers的项目都已经生成了target

// 生成Dialect相关
mlir_tablegen(temp.h.inc -gen-dialect-decls -dialect=temp) 
mlir_tablegen(temp.cpp.inc -gen-dialect-defs -dialect=temp) 
// 生成Type相关
mlir_tablegen(temp.h.inc -gen-typedef-decls -dialect=temp) 
mlir_tablegen(temp.cpp.inc -gen-typedef-defs -dialect=temp)
// 生成Enums相关
mlir_tablegen(temp.h.inc -gen-enum-decls -dialect=temp) 
mlir_tablegen(temp.cpp.inc -gen-enum-defs -dialect=temp) 
// 生成Attr相关
mlir_tablegen(temp.h.inc -gen-attrdef-decls -dialect=temp) 
mlir_tablegen(temp.cpp.inc -gen-attrdef-defs -dialect=temp)
// 生成Op相关
mlir_tablegen(temp.h.inc -gen-op-decls -dialect=temp) 
mlir_tablegen(temp.cpp.inc -gen-op-defs -dialect=temp)
add_public_tablegen_target(MLIRTempDialectIncGen)
```
### 4、src 目录下的 CMakeLists 文件
**`add_mlir_library`**

- **用途**：通用的 MLIR 库构建函数
- **适用于**：任何 MLIR 组件（Interface、Pass、Utility 等）
- **特点**：
    - 最基础、最灵活
    - 需要手动指定所有依赖和链接库
    - 不会自动注册到全局属性
- **示例**：构建 Interface、工具类库

**`add_mlir_dialect_library`**

- **用途**：专门用于构建 MLIR 方言库
- **适用于**：Dialect、Operations、Types、Attributes
- **特点**：
    - 自动注册到 `MLIR_DIALECT_LIBS` 全局属性
    - 自动添加方言相关的默认依赖
    - 会被前面提到的 `get_property(dialect_libs ...)` 获取到
- **示例**：构建 NorthStar Dialect

**`add_mlir_conversion_library`**

- **用途**：专门用于构建方言转换库
- **适用于**：Dialect Lowering、Conversion Pass
- **特点**：
    - 自动注册到 `MLIR_CONVERSION_LIBS` 全局属性
    - 自动添加转换相关的默认依赖
    - 会被 `get_property(conversion_libs ...)` 获取到
- **示例**：构建 NorthStarToLinalg 转换
```python
// 添加到dialect库中
add_mlir_dialect_library(TempDialectName
	TempDialect.cpp
	TempTypes.cpp
	TempAttrs.cpp
	TempOps.cpp
	
	DEPENDS
	MLIRTempDialectIncGen
	
	LINK_LIBS PUBLIC
	MLIRIR
	...
)

// 添加到conversion库中
add_mlir_conversion_library(MLIRTempToTarget
	TempToTarget.cpp
	
	DEPENDS
	MLIRTempConversionPassesIncGen
	
	LINK_LIBS PUBLIC
	MLIRTargetDialect
	MLIRPass
	MLIRTransformUtils
)

// 添加到mlir库中
add_mlir_library(MLIRTempInterfaces
	TempInterfaces.cpp
	
	DEPENDS
	MLIRTempInterfacesIncGen
	
	LINK_LIBS PUBLIC
	MLIR
)
```

