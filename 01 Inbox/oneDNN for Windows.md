---
tags:
  - onednn
  - Windows
date: 2023-12-25T19:34:00
title: oneDNN for Windows
---

---

oneDNN 在 Windows 上的编译流程

```shell
mkdir build
cd build
cmake -G "Visual Stutio 16 2019" -A x64 ..
cmake --build . --config Release
```

> ℹ️ Note
> 3.2 版本以后需要使用 VS2019 进行编译。

> ℹ️ Note
> Visual Studio 无法正确识别 `CMAKE_BUILD_TYPE` 参数，因此如果需要编译 Release 版需要在 build 的时候使用 `--config` 参数。

