---
tags:
  - OpenMP
title: OpenMP Guide
date: 2026-04-21 10:21
type:
---
---
# C++ OpenMP 完全指南

  

> 适用版本：OpenMP 4.5 / 5.0 / 5.1

  

---

## 1. 基础入门

### 1.1 什么是 OpenMP

OpenMP（Open Multi-Processing）是一套基于**共享内存**的多线程并行编程 API，通过编译器指令（`#pragma omp`）、库函数和环境变量实现并行化。

### 1.2 编译选项

| 编译器         | 选项         |
| ----------- | ---------- |
| GCC / Clang | `-fopenmp` |
| Intel ICC   | `-qopenmp` |
| MSVC        | `/openmp`  |

### 1.3 Hello World


```cpp

#include <iostream>
#include <omp.h>


int main() {
#pragma omp parallel
	{
	
		int id = omp_get_thread_num();
		int n = omp_get_num_threads();
		std::cout << "Hello from thread " << id << " of " << n << std::endl;
	}
	
	return 0;
}

```


编译：`g++ -fopenmp hello.cpp -o hello`
运行：`OMP_NUM_THREADS=4 ./hello`

---

## 2. 并行区域原语（Parallel Region Directives）

### 2.1 `#pragma omp parallel` —— 创建并行区域

**含义**：由主线程派生出一组线程（team），每个线程执行并行区域内的代码。

**使用场景**：需要多个线程同时执行一段代码块，是最基础的并行原语。

```cpp

#pragma omp parallel [子句...]

{
// 所有线程都会执行这里
}

```

**常用子句**：
- `num_threads(N)`：显式指定线程数
- `private(x)`：每个线程有自己的 x 副本
- `shared(y)`：所有线程共享 y
- `default(shared|none)`：默认数据共享属性

```cpp

int x = 10;

#pragma omp parallel num_threads(4) private(x)
{
	x = omp_get_thread_num(); // 每个线程的 x 独立
}

```

---

### 2.2 `#pragma omp parallel for` —— 并行循环

**含义**：`parallel` + `for` 的组合简写，既创建线程组，又把 for 循环的迭代分配到各线程。
**使用场景**：循环迭代之间无依赖、可独立执行时的首选并行方式。
  

```cpp

#pragma omp parallel for schedule(static) num_threads(4)
for (int i = 0; i < N; ++i) {
	a[i] = b[i] + c[i];
}
```

**等价写法**：

```cpp
#pragma omp parallel
{
#pragma omp for
	for (int i = 0; i < N; ++i) {
		a[i] = b[i] + c[i];
	}
}
```

---
### 2.3 `#pragma omp for` —— 工作共享（Work-Sharing）

**含义**：仅分配循环迭代，**不创建新线程**。必须在已有的并行区域内使用。
**使用场景**：一个并行区域内有多个循环需要分别分配。
  
```cpp
#pragma omp parallel
{
#pragma omp for
	for (int i = 0; i < N; ++i) a[i] = i;

#pragma omp for
	for (int i = 0; i < N; ++i) b[i] = i * 2;
}
```

---

### 2.4 `#pragma omp sections` / `section` —— 任务分段

**含义**：将不同的代码段分配给不同线程执行，适合**功能并行**（不同线程做不同的事）。
**使用场景**：初始化、计算、输出等不同阶段可以并行。

```cpp

#pragma omp parallel sections
{
#pragma omp section
	{
		task_a();
	}

#pragma omp section
	{
		task_b();
	}

#pragma omp section
	{
		task_c();
	}
}
```

---
### 2.5 `#pragma omp single` —— 单线程执行

**含义**：并行区域内仅由一个线程执行该代码块，其他线程等待（隐式 barrier）。
**使用场景**：只需一个线程完成的初始化、IO 操作、资源分配。

```cpp
#pragma omp parallel
{
#pragma omp single
	{
		std::cout << "Initializing by thread " << omp_get_thread_num() << std::endl;
	}
// 所有线程继续
}

```

**变体**：
- `#pragma omp master`：仅主线程（0 号线程）执行，**不隐式 barrier**，其他线程不等待。

```cpp
#pragma omp master
{
// 只有 thread 0 执行，其他线程继续往下跑
}
```

---
## 3. 数据作用域子句（Data Scoping Clauses）

### 3.1 `shared(var)` —— 共享变量
**含义**：所有线程访问同一个变量。
**使用场景**：只读大数据、或需要通过锁/原子操作协作写入的数据。
**风险**：多线程同时写会产生数据竞争（Data Race）。

```cpp
int sum = 0;
#pragma omp parallel for shared(sum)
for (int i = 0; i < N; ++i) {
#pragma omp atomic
	sum += a[i]; // 必须加原子保护
}

```

---
### 3.2 `private(var)` —— 私有变量

**含义**：每个线程拥有该变量的**未初始化**私有副本。
**使用场景**：循环临时变量、线程局部计算结果。

```cpp
int tmp;
#pragma omp parallel for private(tmp)
for (int i = 0; i < N; ++i) {
	tmp = a[i] * 2; // 每个线程的 tmp 互不影响
	b[i] = tmp;
}

// 外部 tmp 的值不变

```

  

---
### 3.3 `firstprivate(var)` —— 带初始化的私有变量

**含义**：同 `private`，但进入并行区前用**主线程的值初始化**副本。
**使用场景**：线程需要读取某个初始值再独立修改。

```cpp

int offset = 100;

#pragma omp parallel for firstprivate(offset)
for (int i = 0; i < N; ++i) {
	b[i] = a[i] + offset; // 每个线程的 offset 初始都是 100
	offset += i; // 修改不影响其他线程
}

```

---
### 3.4 `lastprivate(var)` —— 保留最后一次迭代值

**含义**：同 `private`，但并行结束后，将**最后一次串行迭代**的值写回原始变量。
**使用场景**：需要知道循环最后一次迭代的某个状态。

```cpp

int last_i = -1;

#pragma omp parallel for lastprivate(last_i)
for (int i = 0; i < 100; ++i) {
	last_i = i; // 最后 last_i = 99
}

```

---

### 3.5 `reduction(op:var)` —— 归约操作

**含义**：每个线程拥有私有副本并行计算，最后按指定操作符合并结果。
**支持的操作符**：`+`、`-`、`*`、`&`、`|`、`^`、`&&`、`||`、`max`、`min`
**使用场景**：求和、求积、求最大最小值等聚合计算。

```cpp

int sum = 0;

#pragma omp parallel for reduction(+:sum)
for (int i = 0; i < N; ++i) {
	sum += a[i]; // 每个线程算局部和，最后自动相加
}

// sum 为总和
```

```cpp

double max_val = -1e300;
#pragma omp parallel for reduction(max:max_val)
for (int i = 0; i < N; ++i) {
	if (a[i] > max_val) max_val = a[i];
}

```

---

### 3.6 `default(shared|none)` —— 默认作用域

**含义**：
- `default(shared)`：未指定作用域的变量默认共享（OpenMP 默认行为）。
- `default(none)`：必须显式声明每个变量的作用域，强制编程者思考数据共享问题。
**推荐**：始终使用 `default(none)` 避免意外数据竞争。

```cpp

int a = 1, b = 2, c = 3;

#pragma omp parallel for default(none) private(a, b) shared(c)
for (int i = 0; i < N; ++i) {
	// a, b 私有；c 共享；未声明的变量编译报错
}

```
  

---
## 4. 调度策略（Loop Scheduling）

### 4.1 `schedule(static [,chunk])` —— 静态分配

**含义**：编译前将迭代按 chunk 大小**均匀静态划分**给各线程。
**特点**：开销最小，各线程负载均衡时效率最高。
**使用场景**：每次迭代计算量**大致相等**。

```cpp

#pragma omp parallel for schedule(static, 64)
for (int i = 0; i < 1000000; ++i) {
	a[i] = heavy_compute(i); // 每次计算量相近
}

```

---

### 4.2 `schedule(dynamic [,chunk])` —— 动态分配

**含义**：线程完成当前 chunk 后，向运行时系统**动态申请**下一批迭代。
**特点**：负载均衡最好，但调度开销较大。
**使用场景**：每次迭代计算量**差异很大**（如不规则网格、分支深度不同）。

```cpp

#pragma omp parallel for schedule(dynamic, 16)
for (int i = 0; i < N; ++i) {
	process_graph_node(i); // 不同节点处理时间差异大
}

```

---

### 4.3 `schedule(guided [,chunk])` —— 引导式分配

**含义**：开始时分配较大 chunk，随后 chunk 大小按**剩余迭代数 / 线程数**指数递减。
**特点**：结合了 static 的低开销和 dynamic 的负载均衡。
**使用场景**：迭代次数多且早期迭代耗时较长。

```cpp
#pragma omp parallel for schedule(guided)
for (int i = 0; i < N; ++i) {
	work(i);
}
```

---
### 4.4 `schedule(auto)` —— 自动选择

**含义**：编译器/运行时自行决定调度策略。
**使用场景**：不确定哪种策略好时，让运行时决定。

```cpp
#pragma omp parallel for schedule(auto)
for (int i = 0; i < N; ++i) { ... }
```

---

### 4.5 `schedule(runtime)` —— 运行时决定

**含义**：通过环境变量 `OMP_SCHEDULE` 控制调度策略。

```bash
export OMP_SCHEDULE="guided,4"
```


```cpp
#pragma omp parallel for schedule(runtime)
for (int i = 0; i < N; ++i) { ... }
```

---

## 5. 同步原语（Synchronization Directives）

### 5.1 `#pragma omp barrier` —— 显式屏障

**含义**：所有线程到达此点后才能继续执行。
**使用场景**：阶段式算法，确保前一步全部完成后再进入下一步。

```cpp
#pragma omp parallel
{
	do_part1();
#pragma omp barrier
	do_part2(); // 确保所有线程 part1 已完成
}
```

---

### 5.2 `#pragma omp critical` —— 临界区

**含义**：同一时刻只有一个线程可以执行该代码块，基于**互斥锁**实现。
**使用场景**：多线程需要读写共享变量，且无法使用原子操作或 reduction。

```cpp
#pragma omp parallel for
for (int i = 0; i < N; ++i) {
	int val = compute(i);
#pragma omp critical
	{
		global_map[val]++; // 保护共享容器
	}
}

```

**命名临界区**：

```cpp
#pragma omp critical(map_update)
{ global_map[key] = value; }
```

不同名的临界区可以并行进入。

---

### 5.3 `#pragma omp atomic` —— 原子操作

**含义**：对单个变量的读写操作是原子的，**比 critical 轻量**。
**支持操作**：`read`、`write`、`update`（`++`、`--`、`+=`、`*=`、`&=` 等）、`capture`。
**使用场景**：简单的计数、累加、位操作。

```cpp
int counter = 0;

#pragma omp parallel for
for (int i = 0; i < N; ++i) {
	if (condition(i)) {
#pragma omp atomic
		counter++;
	}
}
```


```cpp
// atomic capture：读取旧值并更新
#pragma omp atomic capture
{ old = x; x += y; }
```

---
### 5.4 `#pragma omp ordered` —— 按序执行

**含义**：在并行循环中，被标记的代码块按迭代顺序串行执行。
**使用场景**：循环主体可并行，但某部分必须按顺序（如按序输出、累积依赖）。

```cpp
#pragma omp parallel for ordered
for (int i = 0; i < N; ++i) {
	a[i] = compute(i); // 并行
#pragma omp ordered
	{
		std::cout << i << " "; // 按顺序打印
	}
}
```

---

### 5.5 `#pragma omp flush` —— 内存刷新

**含义**：强制线程将寄存器中的变量值写回共享内存，并从内存重新读取。
**使用场景**：需要确保多线程看到最新内存状态（自定义锁、无锁算法）。

```cpp
int flag = 0;

#pragma omp parallel
{
	if (omp_get_thread_num() == 0) {
		data = prepare_data();
#pragma omp flush(data, flag)
		flag = 1;
#pragma omp flush(flag)
	} else {
		while (flag == 0) {
#pragma omp flush(flag)
		}
#pragma omp flush(data)
		use(data);
	}
}
```

---
## 6. 任务并行（Task Parallelism）

### 6.1 `#pragma omp task` —— 创建任务

**含义**：生成一个异步任务，可由线程池中的任意线程执行。
**使用场景**：递归算法（如快速排序、N 皇后）、不规则并行、生产者-消费者模式。

```cpp
void quicksort(int* arr, int left, int right) {
	if (left < right) {
		int pivot = partition(arr, left, right);
	
#pragma omp task
		quicksort(arr, left, pivot - 1);
	
#pragma omp task
		quicksort(arr, pivot + 1, right);
	}
}

#pragma omp parallel
{
#pragma omp single
	quicksort(arr, 0, N - 1);
}
```


---
### 6.2 `#pragma omp taskwait` —— 等待子任务

**含义**：当前任务等待其生成的所有子任务完成。
**使用场景**：任务之间有依赖关系，需要等子任务完成后才能继续。

```cpp
#pragma omp task
{ compute_a(); }

#pragma omp task
{ compute_b(); }

#pragma omp taskwait // 等 a 和 b 都完成
combine(a, b);
```

---
### 6.3 `#pragma omp taskgroup` —— 任务组同步

**含义**：等待当前任务及其**所有后代任务**（子任务的子任务...）全部完成。
**使用场景**：递归生成多层任务时，比 `taskwait` 更彻底。

```cpp
#pragma omp taskgroup
{
#pragma omp task
	recursive_work(0);

#pragma omp task
	recursive_work(1);
} // 等所有递归任务全部完成
```


---
### 6.4 `depend` 子句 —— 任务依赖

**含义**：显式声明任务之间的数据依赖关系，运行时自动调度。

- `depend(in: x)`：读取 x，等前面写 x 的任务完成
- `depend(out: x)`：写入 x，等前面读写 x 的任务完成
- `depend(inout: x)`：读写 x

**使用场景**：流水线并行、DAG 调度。

```cpp
#pragma omp task depend(out: a)
{ produce(a); }

#pragma omp task depend(in: a) depend(out: b)
{ transform(a, b); }

#pragma omp task depend(in: b)
{ consume(b); }
```

---

## 7. SIMD 向量化
### 7.1 `#pragma omp simd` —— 强制 SIMD

**含义**：提示编译器对循环做 SIMD 向量化，同时可指定对齐、 safelen 等。
**使用场景**：需要显式控制向量化，或编译器自动向量化失败时。

```cpp
#pragma omp simd safelen(8)
for (int i = 0; i < N; ++i) {
	c[i] = a[i] + b[i];
}
```

**子句**：
- `safelen(N)`：最大向量长度
- `aligned(ptr: 16)`：内存对齐提示
- `reduction(+:sum)`：SIMD 归约

---
### 7.2 `#pragma omp declare simd` —— 函数向量化
**含义**：声明一个函数可以被 SIMD 调用。

```cpp
#pragma omp declare simd
float func(float a, float b) {
	return a * a + b;
}

#pragma omp simd
for (int i = 0; i < N; ++i) {
	c[i] = func(a[i], b[i]);
}
```

---

## 8. 线程管理与环境

### 8.1 常用运行时库函数

```cpp
#include <omp.h>
int omp_get_thread_num(); // 当前线程 ID（0 ~ N-1）
int omp_get_num_threads(); // 当前并行区线程总数
Int omp_get_max_threads (); // 可创建的最大线程数
Int omp_get_num_procs (); // 物理 CPU 核心数
Void omp_set_num_threads (int); // 设置默认线程数
Bool omp_in_parallel (); // 是否在并行区域内
Void omp_set_nested (int); // 是否允许嵌套并行
```

### 8.2 环境变量

| 变量                | 作用                              |
| ----------------- | ------------------------------- |
| `OMP_NUM_THREADS` | 设置默认线程数                         |
| `OMP_SCHEDULE`    | 设置 `schedule (runtime)` 的策略     |
| `OMP_NESTED`      | 是否开启嵌套并行                        |
| `OMP_PROC_BIND`   | 线程绑定策略（spread/close/master）     |
| `OMP_PLACES`      | 指定线程放置位置（cores/sockets/threads） |
| `OMP_DYNAMIC`     | 是否允许运行时动态调整线程数                  |

---
## 9. 嵌套并行（Nested Parallelism）

**含义**：在一个并行区域内再开一个新的 `parallel` 区域。
**默认**：大多数实现默认关闭嵌套并行（内部 parallel 串行执行）。

**开启方式**：
```cpp

Omp_set_nested (1); // 或通过环境变量 OMP_NESTED=true

```


```cpp
#pragma omp parallel for num_threads (2)
For (int i = 0; i < 2; ++i) {

#pragma omp parallel for num_threads (4)
	For (int j = 0; j < 100; ++j) {
	// 外层 2 线程 × 内层 4 线程 = 最多 8 线程
	}
}
```

**注意**：嵌套并行容易导致线程爆炸，通常用 `omp_get_level ()` 控制嵌套深度。

---
## 10. 项目代码中的实际用例

以下代码来自 `lib/Dialect/Top/Canonicalize/Conv. Cpp`，展示了典型的 OpenMP 并行循环：
  
```cpp

// 例 1：大规模矩阵/卷积权重合并

Int eckk = E * C * K * K;

#pragma omp parallel for schedule (static, omp_schedule (eckk))

For (int i = 0; i < eckk; ++i) {

Int e = i / ckk;

Int c = (i % ckk) / kk;

Int k 1 = (i % kk) / K;

Int k 2 = i % K;

Float sum = 0.0;

For (int d = 0; d < D; ++d) {

Sum += (*Filterop_f 32)[e * D * K * K + d * K * K + k 1 * K + k 2] *

(*prefilter_f 32_tp)[c * D + d];

}

(*filter_merge)[e * C * K * K + c * K * K + k 1 * K + k 2] = sum;

}

  

// 例 2：bias 计算并行

#pragma omp parallel for schedule (static, omp_schedule (E))

For (int e = 0; e < E; ++e) {

Float sum = 0.0;

For (int d = 0; d < D; ++d) {

For (int u = 0; u < K; ++u) {

For (int v = 0; v < K; ++v) {

Sum += (*preBiasop_f 32)[d] *

(*Filterop_f 32)[e * D * K * K + d * K * K + u * K + v];

}

}

}

// ...

}

```

  

**要点**：

- 使用 `schedule (static)`：循环体内每次迭代计算量基本相同（权重访问模式固定），静态调度开销最小。

- 外层循环并行、内层累加串行：避免了多线程对 `sum` 的写竞争（每个线程有私有的 `sum` 局部变量）。

  

---

  

## 11. 最佳实践与常见陷阱

### 11.1 最佳实践

1. **始终使用 `default (none)`**，显式声明所有变量作用域。
2. **优先使用 `reduction` 替代手动 atomic/critical** 做累加。
3. **循环迭代次数要足够大**（通常 > 1000），否则线程开销得不偿失。
4. **避免在并行区内做 I/O**（`cout`、文件读写），会严重拖慢性能。
5. **注意 false sharing**：多个线程同时写同一缓存行（64 字节）的不同变量，会导致缓存失效。用 `padding` 或 `firstprivate` 避免。

  

### 11.2 常见陷阱

  

| 陷阱       | 说明                                           |
| -------- | -------------------------------------------- |
| 数据竞争     | 多个线程同时读写共享变量，未加保护                            |
| 迭代依赖     | ` #pragma omp for` 要求循环迭代之间无依赖               |
| 私有变量未初始化 | `private` 变量进入并行区后是未定义值，要用 `firstprivate`    |
| 死锁       | 在 `critical` 内再进另一个 `critical`，或 barrier 不匹配 |
| 动态内存不安全  | 多线程同时 `new/delete` 同一指针                      |

---

  

## 12. 速查表

  

| 原语                     | 类别       | 核心作用               |
| ---------------------- | -------- | ------------------ |
| `parallel`             | 并行区      | 创建线程组              |
| `parallel for`         | 并行区+工作共享 | 并行化循环              |
| `for`                  | 工作共享     | 在已有并行区内分配循环        |
| `sections` / `section` | 工作共享     | 功能并行，不同代码段分给不同线程   |
| `single`               | 工作共享     | 仅一个线程执行，隐式 barrier |
| `master`               | 工作共享     | 仅主线程执行，无 barrier   |
| `task`                 | 任务并行     | 创建异步任务             |
| `taskwait`             | 任务同步     | 等子任务完成             |
| `taskgroup`            | 任务同步     | 等所有后代任务完成          |
| `barrier`              | 同步       | 显式屏障               |
| `critical`             | 同步       | 互斥临界区              |
| `atomic`               | 同步       | 单变量原子操作            |
| `ordered`              | 同步       | 按序执行               |
| `flush`                | 同步       | 内存一致性刷新            |
| `simd`                 | 向量化      | SIMD 向量化提示         |
| `reduction`            | 数据子句     | 归约聚合               |
| `private`              | 数据子句     | 未初始化私有副本           |
| `firstprivate`         | 数据子句     | 带初始化私有副本           |
| `lastprivate`          | 数据子句     | 保留末次迭代值            |
| `shared`               | 数据子句     | 共享变量               |
| `schedule`             | 调度子句     | 循环迭代分配策略           |
| `num_threads`          | 并行区子句    | 指定线程数              |
| `depend`               | 任务子句     | 声明任务依赖             |
| `collapse`             | 循环子句     | 合并多层循环为单层迭代空间      |

  