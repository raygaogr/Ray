---
tags:
  - binding
title: Python前端绑定C++ Kernel
date: 2025-11-11 09:24
type: permanent-note
---
---
## 一、基于Pytorch 的方法
使用 Python 的 cpp_extension 库，具体代码如下：
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