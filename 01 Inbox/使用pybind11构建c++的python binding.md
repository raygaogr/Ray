---
tags:
  - pybind
title: 使用pybind11构建c++的python binding
date: 2026-01-16 16:34
type: permanent-note
---
---
## C++ 对外接口声明
写一份 c++ 的接口说明，告诉调用者模块中有哪些接口可以使用。比如：

```c++
#include <pybind11/iostream.h>
#include <pybind11/numpy.h>
#include <pybind11/pybind11>
#include <pybind11/stl> 


class py_module {
public:
	py_module() = default;
	~py_module();
	
	void load(std::string filename);
	void set_tensor(std::string name, py::array_t<float, py::array::c_style | py::array::forecast> data);
	
public:
	py::list all_tensor_name;
	
private:
	std::unique_ptr<mlir::MLIRContext> context_;
}
```

## C++具体功能实现
上面功能函数的具体实现逻辑
```c++
py_final_module::~py_final_module() {
	evaluator_.reset();
	auto module = module_.release();
	if (module) {
		module.erase();
	}
	context_.reset();
}

void py_final_module::load(std::string filename) {
	DialectRegistry registry;
	registry.insert<func::FuncDialect, top::TopDialect, tpu::TpuDialect,
	quant::QuantizationDialect>();
	context_ = std::make_unique<MLIRContext>(registry);
	module_ = parseSourceFile<ModuleOp>(filename, context_.get());
}

void py_final_module::set_tensor(
	std::string name,
	py::array_t<float, py::array::c_style | py::array::forcecast> data) {
	evaluator_->setTensor(name, data.data(), data.size() * sizeof(float), false);
}
```

## **Python binding 的核心代码**

```c++
// 注册一个模块名，pymlir 内部别名为 m
PYBIND11_MODULE(pymlir, m) {
	// 注册一个类，名为 py_module, 放入 m 中，命名为 module
	py::class_<py_module>(m, "module", "MLIR Module")
	    // 绑定构造函数
		.def(py::init<>())
		// 绑定类中的 load 函数，函数地址的格式为： &类名::函数名
		.def（“load”, &py_module::load, "load module from IR"）
		// 绑定 set_tensor 函数
		.def("set_tensor", &py_module::set_tensor)
		// 绑定类中 public 的成员变量
		.def_readonly("all_tensor_names", &py_module::all_tensor_names)；
	
	// 用于将c++的错误输出流重定向到python端方便debug调试
	py::scoped_ostream_redirect output{std::cerr, py::module::import("sys").attr("stderr")};
}

```

## **使用 pybind 11 的 cmake 工具编译打包成可被 python 调用的工具包**

CmakeLists.txt 的写法：
```python
cmake_minimum_required(VERSION 2.8.12)
if (POLICY CMP0048)
	# cmake warns if loaded from a min-3.0-required parent dir, so silence the warning:
	cmake_policy(SET CMP0048 NEW)
endif()

project(pymlir)

find_package(pybind11 REQUIRED CONFIG)

file(GLOB _sources pyfinalmlir.cpp)

pybind11_add_module(pymlir ${_sources})
target_link_libraries(pymlir PRIVATE
	TPUMLIRInitAll
	MLIRTransforms
	MLIRParser
	LLVMCore
	LLVMSupport
)

install(TARGETS pymlir DESTINATION python)
```