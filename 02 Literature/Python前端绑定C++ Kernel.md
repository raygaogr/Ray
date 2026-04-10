---
tags:
  - PythonBinding
title: Python前端绑定C++ Kernel
date: 2025-11-11 09:24
type: permanent-note
---
---
## 一、基于Pytorch 的方法
使用 Python 的 cpp_extension 库，python前端代码如下：
```python
import torch
from torch.utils.cpp_extension import load

lib = load(
	name = "aa", // 编译的动态库名,比如这里是 aa.so
	sources = ["aa.cu", "bb.cpp"], // 需要编译的源码文件
	extra_cuda_flags = [
		"-O3",
		"-U__CUDA_NO_HALF_OPERATORS__",
		"-U__CUDA_NO_HALF_CONVERSIONS__",
		"-U__CUDA_NO_HALF2_OPERATORS__",
		"-U__CUDA_NO_BFLOAT16_CONVERSIONS__",
		"--expt-relaxed-constexpr",
		"--expt-extended-lambda",
		"--use_fast_math",
	],
	extra_cflags=["-std=c++17"],
)
```

C++ 前端代码如下：
```c++
#include <torch/extension.h>

__global__ void elementwise_add_f32_kernel(float *a, float *b, float *c, int N) {
	int idx = blockDim.x * blockIdx.x + threadIdx.x;
	if (idx < N)
		c[idx] = a[idx] + b[idx];
}

#define STRINGFY(str) #str
#define TORCH_BINDING_COMMON_EXTENSTION(func) \
	m.def(STRINGFY(func), &func, STRINGFY(func));

PYBIND11_MODULE(TORCH_EXTENSION_NAME, m) {
	TORCH_BINDING_COMMON_EXTENSTION(elementwise_add_f32)
}
```

关键点：
1. 导入 <torch/extension.h> 头文件
2. 使用 PYBIND11_MODULE 宏将 c++函数封装到 module 中，其中第一个参数是 torch 的扩展库名，这里就是 **aa** ，第二个参数模块名通常用 **m** 进行表示。