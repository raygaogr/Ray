---
tags: 
title: "llama.cpp 源码阅读"
date: "2026-04-03 09:57"
type:
---
---
# Llama. Cpp 完整工作流深度分析

  

> 本文以 `examples/simple/simple.cpp` 为入口，逐层拆解 llama. Cpp 的完整推理工作流，并对核心数据结构、模块职责进行系统梳理。

  

---

  

## 1. 从 simple. Cpp 看全貌

  

`examples/simple/simple.cpp` 是 llama. Cpp 提供的最小可运行示例，仅 220 行，却完整覆盖了一次文本生成的全部生命周期。将其提炼为 7 个阶段：

  

```cpp

// 1. 加载动态后端

ggml_backend_load_all();

  

// 2. 加载模型

llama_model_params model_params = llama_model_default_params();

llama_model * model = llama_model_load_from_file(path, model_params);

const llama_vocab * vocab = llama_model_get_vocab(model);

  

// 3. Tokenize

std::vector<llama_token> prompt_tokens(n_prompt);

llama_tokenize(vocab, prompt.c_str(), prompt.size(), prompt_tokens.data(), ...);

  

// 4. 创建上下文

llama_context_params ctx_params = llama_context_default_params();

llama_context * ctx = llama_init_from_model(model, ctx_params);

  

// 5. 初始化采样器

llama_sampler * smpl = llama_sampler_chain_init(sparams);

llama_sampler_chain_add(smpl, llama_sampler_init_greedy());

  

// 6. Encode/Decode 主循环

llama_batch batch = llama_batch_get_one(prompt_tokens.data(), prompt_tokens.size());

for (...) {

if (llama_decode(ctx, batch)) { ... }

new_token_id = llama_sampler_sample(smpl, ctx, -1);

batch = llama_batch_get_one(&new_token_id, 1);

}

  

// 7. 释放资源

llama_sampler_free(smpl);

llama_free(ctx);

llama_model_free(model);

```

  

下面按照这 7 个阶段，深入拆解每一个核心模块。

  

---

  

## 2. 核心数据结构全景图

  

在分析流程之前，先建立对核心 C++ 结构体的认知。

  

### 2.1 `llama_model` —— 权重与架构的容器

  

**定义位置**：`src/llama-model.h` (line 402)

  

```cpp

struct llama_model {

llm_type type = LLM_TYPE_UNKNOWN;

llm_arch arch = LLM_ARCH_UNKNOWN;

std::string name = "n/a";

  

llama_hparams hparams = {};

llama_vocab vocab;

  

// 全局共享张量

struct ggml_tensor * tok_embd = nullptr; // token embedding

struct ggml_tensor * output_norm = nullptr; // final norm

struct ggml_tensor * output = nullptr; // lm_head

  

// 逐层权重

std::vector<llama_layer> layers;

  

llama_model_params params;

std::vector<ggml_backend_dev_t> devices;

// ...

};

```

  

**关键职责**：

- **超参数与词表**：`hparams` 保存 `n_embd`, `n_layer`, `n_head`, `n_ctx_train` 等；`vocab` 封装 tokenizer。

- **权重张量**：以 `ggml_tensor*` 形式持有所有模型权重，包括全局的 `tok_embd`、`output`，以及每一层的 `wq/wk/wv/wo`、`ffn_gate/up/down` 等。

- **设备分配**：`devices` 列表记录模型权重被 offload 到哪些 backend（CPU/GPU/RPC）。

- **计算图工厂**：提供 `build_graph(const llm_graph_params & params) const` 方法，负责根据输入 batch 构造 `ggml_cgraph`。

  

### 2.2 `llama_vocab` —— Tokenizer 封装

  

**定义位置**：`src/llama-vocab.h` (line 57)

  

```cpp

struct llama_vocab {

struct token_data {

std::string text;

float score;

llama_token_attr attr;

};

// ...

int32_t tokenize(...);

int32_t token_to_piece(...);

int32_t detokenize(...);

};

```

  

**关键职责**：

- 支持 SPM、BPE、WPM、UGM、RWKV 等多种词表类型。

- `llama_tokenize` 将文本转为 `llama_token` 数组；`llama_token_to_piece` 将 token 还原为字符串片段。

- 管理特殊 token：`BOS`、`EOS`、`EOT`、`PAD`、`FIM` 系列等。

  

### 2.3 `llama_context` —— 运行时环境

  

**定义位置**：`src/llama-context.h` (line 31)

  

```cpp

struct llama_context {

const llama_model & model;

llama_cparams cparams;

  

// Memory (KV Cache / Recurrent State)

std::unique_ptr<llama_memory_i> memory;

  

// 输出缓冲区

size_t logits_size = 0;

float * logits = nullptr;

size_t embd_size = 0;

float * embd = nullptr;

  

// Batch 分配器

std::unique_ptr<llama_batch_allocr> balloc;

  

// 计算调度

ggml_backend_sched_ptr sched;

ggml_backend_t backend_cpu = nullptr;

std::vector<ggml_backend_ptr> backends;

  

// 图结果复用

llm_graph_result_ptr gf_res_prev;

llm_graph_result_ptr gf_res_reserve;

// ...

};

```

  

**关键职责**：

- **上下文参数 `cparams`**：运行时的 `n_ctx`, `n_batch`, `n_ubatch`, `n_threads`, `flash_attn`, `causal_attn` 等。

- **Memory 模块**：抽象了 KV Cache（以及 RWKV/Mamba 等 recurrent state），负责序列的存储、shift、copy、rm。

- **Backend 调度器 `sched`**：`ggml_backend_sched` 是 GGML 的核心多后端调度器，自动将 `ggml_cgraph` 拆分到 CPU/GPU 上执行。

- **输出管理**：`logits` 和 `embd` 是从 backend 异步拉回到 host 的浮点缓冲区；`output_ids` 维护 batch 中哪些 token 需要输出 logits。

- **图复用**：`gf_res_prev` 保存上一次 decode 的计算图结果，如果 topology 相同则直接复用，避免重复 build/alloc。

  

### 2.4 `llama_batch` / `llama_ubatch` —— 输入批次

  

**C API 定义**：`include/llama.h` (line 230)

  

```c

typedef struct llama_batch {

int32_t n_tokens;

llama_token * token;

float * embd;

llama_pos * pos;

int32_t * n_seq_id;

llama_seq_id ** seq_id;

int8_t * logits;

} llama_batch;

```

  

**`llama_ubatch`**（内部使用，定义于 `src/llama-batch.h`）是对 `llama_batch` 的进一步拆分与规范化：

  

```cpp

struct llama_ubatch {

bool b_equal_seqs; // 所有序列长度是否相同

uint32_t n_tokens; // 总 token 数

uint32_t n_seq_tokens; // 每个序列的 token 数

uint32_t n_seqs; // 序列数

uint32_t n_seqs_unq; // 不同序列 id 的数量

llama_token * token;

float * embd;

llama_pos * pos;

// ...

};

```

  

**关键职责**：

- `llama_batch` 是用户可见的输入容器；`llama_ubatch` 是内部调度单元。

- `llama_batch_allocr` 负责将逻辑 `batch` 拆分为多个 `ubatch`，以适配 `n_ubatch`（物理 batch size）。

- 自动补全缺失字段：如果用户没有提供 `pos`、`seq_id`、`logits`，`llama_batch_allocr::init` 会自动推导（例如默认 seq_id=0，pos 从 memory 尾部自动递增）。

  

### 2.5 `llama_sampler` —— 采样器链

  

**C API 定义**：`include/llama.h` (line 1138)

  

```c

struct llama_sampler {

const struct llama_sampler_i * iface;

llama_sampler_context_t ctx;

};

  

struct llama_sampler_i {

const char * (*name)(...);

void (*accept)(..., llama_token token);

void (*apply)(..., llama_token_data_array * cur_p);

void (*reset)(...);

struct llama_sampler * (*clone)(...);

void (*free)(...);

};

```

  

**关键职责**：

- 采用**职责链模式**（Chain of Responsibility）。`llama_sampler_chain` 本身也是一个 `llama_sampler`，其 `apply` 方法按顺序调用链上每个 sampler 的 `apply`。

- 常见 sampler：`greedy`（取最大 logit）、`dist`（概率分布随机采样）、`top_k`、`top_p`、`min_p`、`typical`、`temperature`、`mirostat`、`grammar`、`penalties` 等。

- `llama_sampler_sample` 的工作流程：

1. 调用 `llama_get_logits_ith(ctx, idx)` 获取 logits 指针。

2. 构造 `llama_token_data_array cur_p`（大小为 `n_vocab`）。

3. 调用 `llama_sampler_apply(smpl, &cur_p)`。

4. 返回 `cur_p.data[cur_p.selected].id`，并调用 `llama_sampler_accept` 更新状态。

  

### 2.6 `llm_graph_params` / `llm_graph_result` / `llm_graph_context` —— 计算图构建

  

**定义位置**：`src/llama-graph.h`

  

```cpp

struct llm_graph_params {

llm_arch arch;

Llama_hparams hparams;

Llama_cparams cparams;

Llama_ubatch ubatch;

Llm_graph_type gtype; // DEFAULT / ENCODER / DECODER

Ggml_backend_sched_t sched;

// ...

Llm_graph_result * res;

};

  

Class llm_graph_result {

Ggml_tensor * t_logits = nullptr;

Ggml_tensor * t_embd = nullptr;

std::vector<llm_graph_input_ptr> inputs;

Ggml_cgraph * gf;

// ...

};

  

Struct llm_graph_context {

// 提供 build_attn, build_ffn, build_norm, build_moe_ffn 等工厂方法

Ggml_tensor * build_attn (...);

Ggml_tensor * build_ffn (...);

// ...

};

```

  

**关键职责**：

- `llm_graph_params` 是一次 graph build 的全部输入参数。`allow_reuse` 方法通过比较这些参数决定是否复用旧图。

- `llm_graph_result` 保存 build 后的 `ggml_cgraph*` 以及所有 input tensor 的引用，供 `set_inputs ()` 填充数据。

- `llm_graph_context` 是图构建的“绘图板”，根据 `arch`（LLaMA、Qwen、Mistral、Gemma 等）调用不同的 layer build 函数，把模型结构翻译成 `ggml_tensor` 运算节点。

  

### 2.7 `llama_memory_i` —— 内存抽象（KV Cache 核心）

  

**定义位置**：`src/llama-memory. H` (line 68)

  

```cpp

Struct llama_memory_i {

Virtual llama_memory_context_ptr init_batch (

Llama_batch_allocr & balloc, uint 32_t n_ubatch, bool embd_all) = 0;

  

Virtual llama_memory_context_ptr init_full () = 0;

Virtual llama_memory_context_ptr init_update (llama_context * lctx, bool optimize) = 0;

  

Virtual void clear (bool data) = 0;

Virtual bool seq_rm (llama_seq_id seq_id, llama_pos p 0, llama_pos p 1) = 0;

Virtual void seq_cp (llama_seq_id src, llama_seq_id dst, llama_pos p 0, llama_pos p 1) = 0;

Virtual void seq_add (llama_seq_id seq_id, llama_pos p 0, llama_pos p 1, llama_pos shift) = 0;

  

Virtual llama_pos seq_pos_min (llama_seq_id seq_id) const = 0;

Virtual llama_pos seq_pos_max (llama_seq_id seq_id) const = 0;

// ...

};

```

  

**关键职责**：

- 将 KV Cache 抽象为通用“Memory”。除了标准的 `llama_kv_cache`，还有针对 SWA（Sliding Window Attention）的 `llama_kv_cache_iswa`、针对 RWKV/Mamba 的 `llama_memory_recurrent`、以及混合模型用的 `llama_memory_hybrid`。

- `init_batch`：验证 batch 是否能放入 cache，并将其拆分为 `ubatch` + `memory_context`。

- `init_update`：处理序列的 shift、copy、defrag 等 metadata 更新，某些情况下需要额外发起一次小型 compute graph。

- `seq_pos_max` / `seq_pos_min`：跟踪每个序列在 cache 中的位置范围，用于自动推导新 batch 的 `pos`。

  

---

  

## 3. 分阶段深度拆解

  

### 3.1 阶段 1：Backend 初始化

  

```cpp

Ggml_backend_load_all ();

```

  

**实现**：`ggml_backend_load_all ()` 会扫描当前进程可加载的动态库（如 `ggml-cuda`、`ggml-vulkan`、`ggml-metal`、`ggml-sycl` 等），将其注册为 `ggml_backend_reg_t`。

  

**对 llama. Cpp 的意义**：

- 在调用 `llama_model_load_from_file` 之前，必须至少有一个 backend 被注册，否则加载会失败（`ggml_backend_reg_count () == 0` 时直接返回 `nullptr`）。

- 如果只有 CPU backend，它也是通过 `ggml_backend_init_by_type (GGML_BACKEND_DEVICE_TYPE_CPU, ...)` 在 `llama_context` 构造时自动初始化的。

  

### 3.2 阶段 2：模型加载

  

```cpp

Llama_model * model = llama_model_load_from_file (path, model_params);

```

  

**调用链**：

1. `llama_model_load_from_file`（`src/llama. Cpp` line 302）

2. -> `llama_model_load_from_file_impl`（line 155）

3. -> `llama_model_load`（line 102）

4. -> `llama_model_loader` 打开 GGUF 文件

  

**GGUF 加载流程**：

1. **读取元数据**：`model. Load_arch (ml)` 识别模型架构（`LLM_ARCH_LLAMA`、`LLM_ARCH_QWEN 2` 等）；`model. Load_hparams (ml)` 填充 `llama_hparams`；`model. Load_vocab (ml)` 构建 `llama_vocab`。

2. **读取张量**：`model. Load_tensors (ml)` 遍历 GGUF 中所有 tensor，根据名称映射到 `llama_model` 的对应字段（如 `blk. 0. Attn_q.weight` -> `layers[0]. Wq`）。

3. **Backend Buffer 分配**：每个 `ggml_tensor` 通过 `ggml_backend_buft_alloc_buffer` 分配到具体的 backend buffer 上。根据 `n_gpu_layers` 和 `split_mode` 决定层放置在 CPU 还是 GPU。

4. **数据拷贝/映射**：如果使用 `mmap`，数据可能延迟加载；否则从 GGUF 直接 read 到 backend buffer。

  

**关键数据结构**：`llama_layer`（`src/llama-model. H` line 192）保存了每一层的全部张量指针，是后续 `build_graph` 时的直接数据源。

  

### 3.3 阶段 3：Tokenize

  

```cpp

Const int n_prompt = -llama_tokenize (vocab, prompt. C_str (), prompt.Size (), NULL, 0, true, true);

std::vector<llama_token> prompt_tokens (n_prompt);

Llama_tokenize (vocab, prompt. C_str (), prompt.Size (), prompt_tokens.Data (), prompt_tokens.Size (), true, true);

```

  

**实现**：`src/llama-vocab. Cpp`

  

- 先传入 `NULL` 做一次 dry-run，返回需要的 token 数量（负值）。

- 根据词表类型（BPE/SPM/...）调用对应的 tokenizer 实现。

- `add_special=true` 会在开头自动添加 BOS token（如果模型配置要求）；`parse_special=true` 允许特殊 token 被正确解析而不是当作普通文本。

  

### 3.4 阶段 4：上下文创建

  

```cpp

Llama_context * ctx = llama_init_from_model (model, ctx_params);

```

  

**调用链**：`llama_init_from_model`（`src/llama-context. Cpp` line 2308）-> `new llama_context (model, params)`（line 19）。

  

**`llama_context` 构造函数的核心工作**：

  

#### A. 参数归一化

- `n_ctx`：如果用户传 0，则使用 `hparams. N_ctx_train`。

- `n_batch` / `n_ubatch`：`n_ubatch` 是实际物理 batch size，不能超过 `n_batch`；因果注意力模式下 `n_batch` 不能超过 `n_ctx`。

- Flash Attention 兼容性检查：例如 Grok 强制禁用，量化 V cache 必须启用 FA 等。

  

#### B. Backend 初始化

- 根据 `model. Devices` 初始化 GPU backend；自动加上 ACCEL backend（如 BLAS）和 CPU backend。

- 收集每个 backend 的 `set_n_threads` 函数指针，供后续动态调整线程数。

  

#### C. 输出缓冲区分配

- 调用 `output_reserve (params. N_seq_max)` 在 host 上分配 `logits` 和 `embd` 缓冲区。后续如果输出数量更大，会惰性扩容。

  

#### D. Memory 模块初始化

- 调用 `model. Create_memory (params_mem, cparams)` 根据模型架构创建合适的 memory 实现（标准 KV cache、iSWA、recurrent、hybrid）。

- `memory->init_full ()` 模拟一个“满”的 cache，用于后续 worst-case graph reserve。

  

#### E. 计算图调度器 `sched` 初始化

- `ggml_backend_sched_new (backend_ptrs, backend_buft, ...)` 创建多后端调度器。

- **Pipeline Parallelism**：如果多 GPU + layer split + 全 offload，且所有设备支持 async/events，则开启流水线并行。

  

#### F. 预分配 worst-case compute buffers

- **Prompt Processing (PP) graph**：`graph_reserve (n_tokens, n_seqs, n_tokens, mctx)` 用最大 `n_ubatch` 和 `n_seq_max` 预分配一次图，确保实际运行时不会触发 `ggml-alloc` 的重新分配。

- **Token Generation (TG) graph**：`graph_reserve (n_seqs, n_seqs, n_seqs, mctx)` 用 bs=1 预分配。

- 这两个 reserve 操作会分别统计 `n_nodes` 和 `n_splits`，并打印到日志。

  

**为什么需要 reserve？**

因为在 `ggml_backend_sched` 中，图的 topology 决定了一次性分配的 buffer 大小。如果运行时的图比 reserve 时大，就会触发昂贵的 realloc。因此 llama. Cpp 在构造 `llama_context` 时就用 worst-case 预分配好，保证运行时的实时性。

  

### 3.5 阶段 5：Sampler 初始化

  

```cpp

Llama_sampler * smpl = llama_sampler_chain_init (sparams);

Llama_sampler_chain_add (smpl, llama_sampler_init_greedy ());

```

  

**实现**：`src/llama-sampling. Cpp`

  

- `llama_sampler_chain` 是一个容器，内部持有 `std::vector<llama_sampler*> samplers`。

- `llama_sampler_chain_apply` 遍历每个 sampler，依次对 `llama_token_data_array` 做变换。

- `llama_sampler_init_greedy` 的 `apply` 只是简单地找出 logit 最大的 token 并设置 `cur_p->selected`。

  

### 3.6 阶段 6：Decode 主循环（核心中的核心）

  

```cpp

Llama_batch batch = llama_batch_get_one (prompt_tokens.Data (), prompt_tokens.Size ());

For (int n_pos = 0; n_pos + batch. N_tokens < n_prompt + n_predict; ) {

If (llama_decode (ctx, batch)) { ... }

N_pos += batch. N_tokens;

New_token_id = llama_sampler_sample (smpl, ctx, -1);

Batch = llama_batch_get_one (&new_token_id, 1);

}

```

  

这是整个推理流程中最复杂的部分。下面将其拆分为 **Batch 构建** -> **Decode 入口** -> **内部 ubatch 拆分** -> **Memory 准备** -> **Graph Build / Reuse** -> **Graph Compute** -> **结果提取** -> **Sampler**。

  

#### Step 6.1 Batch 构建

  

`llama_batch_get_one`（`src/llama-batch. Cpp` line 819）是一个 helper，它返回一个只包含 token 指针的轻量 `llama_batch`，其余字段（`pos`, `seq_id`, `logits`）为 `NULL`，表示由 `llama_decode` 内部自动推导：

  

```cpp

Struct llama_batch llama_batch_get_one (llama_token * tokens, int 32_t n_tokens) {

Return {

/*n_tokens =*/ n_tokens,

/*tokens =*/ tokens,

/*embd =*/ nullptr,

/*pos =*/ nullptr,

/*n_seq_id =*/ nullptr,

/*seq_id =*/ nullptr,

/*logits =*/ nullptr,

};

}

```

  

#### Step 6.2 Decode 入口

  

`llama_decode`（`src/llama-context. Cpp` line 2750）-> `ctx->decode (batch_inp)`（line 963）。

  

**`llama_context::decode` 的大致流程**：

  

```

Decode (batch_inp)

├── balloc->init (batch_inp, vocab, memory, n_embd, n_seq_max, output_all)

│ └── 自动补全 pos/seq_id/logits，校验 token 合法性，统计 n_outputs

├── memory_update (false)

│ └── 处理 pending 的 shift/copy/defrag

├── memory->init_batch (*balloc, n_ubatch, output_all)

│ └── 将 batch 拆分为 ubatch，检查 KV slot 是否足够

│ └── 返回 llama_memory_context_i (mctx)

├── output_reserve (n_outputs_all)

│ └── 确保 host 上的 logits/embd 缓冲区足够大

├── do {

│ ├── process_ubatch (ubatch, LLM_GRAPH_TYPE_DECODER, mctx, status)

│ │ ├── graph reuse check (gf_res_prev->can_reuse)

│ │ ├── if not reusable: model. Build_graph (gparams) -> ggml_cgraph

│ │ ├── ggml_backend_sched_alloc_graph (sched, gf)

│ │ ├── res->set_inputs (&ubatch) // 填充 token/pos/mask 等输入张量

│ │ └── graph_compute (gf, batched) // 异步调度执行

│ ├── extract logits from t_logits -> logits host buffer

│ ├── extract embeddings from t_embd -> embd host buffer

│ └── n_outputs_prev += n_outputs

│ } while (mctx->next ()) // 处理下一个 ubatch

└── output reorder (selection sort) // 保证输出顺序与用户 batch 一致

```

  

#### Step 6.3 Batch -> UBatch 拆分 (`llama_batch_allocr`)

  

**定义/实现**：`src/llama-batch. Cpp`

  

`llama_batch_allocr::init` 会：

1. **校验**：token 是否在词表范围内，seq_id 是否越界。

2. **自动推导缺失字段**：

- `n_seq_id` 默认全部为 1。

- `seq_id` 默认全部为 `seq_id=0`。

- `pos` 默认从 `memory->seq_pos_max (seq_id) + 1` 开始顺序递增。

- `logits` 默认仅最后一个 token 为 `true`（如果 `output_all` 则为全部 `true`）。

3. **拆分策略**：

- `split_simple (n_ubatch)`：简单按顺序切分。

- `split_equal (n_ubatch, sequential)`：保证每个序列长度相等，适合多序列并行。

- `split_seq (n_ubatch)`：按序列边界切分。

  

#### Step 6.4 Memory 准备 (`llama_memory_i`)

  

以标准 KV Cache（`llama_kv_cache`）为例：

  

1. `init_batch` 接收 `balloc` 和 `n_ubatch`。

2. 计算当前 batch 中每个序列需要占用的 KV 位置。

3. **寻找空闲 cell**：KV cache 内部是一个巨大的 cell 数组，每个 cell 对应一个 `(layer, head, position)` 的 K/V 向量。`init_batch` 需要为所有新 token 找到连续的或分散的 cell。

4. 如果找不到足够的 slot，返回 `LLAMA_MEMORY_STATUS_FAILED_PREPARE`，此时 `decode` 会尝试调用 `memory_update (true)` 做 defrag/optimize，然后重试；若仍然失败，返回 `1`（KV cache full）。

5. 成功后返回 `llama_kv_cache_context`，它持有当前 ubatch 以及指向 cache 中对应 cell 索引的映射。

  

#### Step 6.5 Graph Build / Reuse (`process_ubatch`)

  

**定义**：`llama_context::process_ubatch`（`src/llama-context. Cpp` line 737）。

  

```cpp

Llm_graph_result * llama_context:: process_ubatch (

Const llama_ubatch & ubatch,

Llm_graph_type gtype,

Llama_memory_context_i * mctx,

Ggml_status & ret)

{

// 1. 先应用 memory context（例如将 K/V 数据写入 cache 的 compute graph）

If (mctx && !Mctx->apply ()) { ret = GGML_STATUS_FAILED; return nullptr; }

  

// 2. 图复用判断

Auto * res = gf_res_prev.Get ();

Const auto gparams = graph_params (res, ubatch, mctx, gtype);

  

If (! Graph_reuse_disable && res->can_reuse (gparams)) {

N_reused++;

} else {

Res->reset ();

Ggml_backend_sched_reset (sched.Get ());

Gf = model. Build_graph (gparams); // <--- 进入 llama-model. Cpp 的庞大 build_graph

Ggml_backend_sched_alloc_graph (sched.Get (), gf);

}

  

// 3. 填充输入数据

Res->set_inputs (&ubatch);

  

// 4. 执行计算

Const auto status = graph_compute (res->get_gf (), ubatch. N_tokens > 1);

// ...

}

```

  

**`model. Build_graph`**（`src/llama-model. Cpp`）是 llama. Cpp 最庞大的函数之一，其核心逻辑：

1. 创建 `llm_graph_context ctx_graph (params)`。

2. 根据 `arch` 调用对应的 `build_*` 函数（如 `build_llama`、`build_qwen 2`、`build_gemma` 等）。

3. 逐层遍历 `layers`：

- `build_inp_embd` / `build_inp_pos`

- `build_norm`

- `build_attn`（根据 memory 类型选择 `build_attn_inp_kv`、`build_attn_inp_kv_iswa` 或 `build_attn_inp_no_cache`）

- 内部调用 `ggml_rope_ext`（RoPE）、`ggml_flash_attn_ext`（如果启用 FA）或手动 `mat_mul + soft_max`。

- `build_ffn` / `build_moe_ffn`

- Residual add

4. 最终 `build_dense_out` / `build_pooling`。

5. 将输出 logits/embd 张量挂到 `llm_graph_result` 上。

  

**Graph Reuse 的判定条件**：`llm_graph_params::allow_reuse`（`src/llama-graph. H` line 427）。只有当以下参数完全一致时才复用：

- `ubatch` 的 `n_tokens`, `n_seq_tokens`, `n_seqs`, `n_seqs_unq`, `equal_seqs`, token/embd 模式。

- `cparams. Embeddings`, `cparams. Causal_attn`。

- `arch`, `gtype`, `cvec`, `loras`, `cross`, `n_outputs`。

  

对于常见的 **Prompt Processing -> Token Generation** 场景，由于 `n_tokens` 从 `N` 变成 `1`，图 topology 会变化，因此第一次 TG 会触发一次新的 build，但后续连续的 TG 都会复用同一张图，这就是 llama. Cpp 日志中 `graphs reused = X` 的来源。

  

#### Step 6.6 Graph Compute (`graph_compute`)

  

```cpp

Ggml_status llama_context:: graph_compute (ggml_cgraph * gf, bool batched) {

Int n_threads = batched ? Cparams. N_threads_batch : cparams. N_threads;

// 设置 CPU backend 的 threadpool

// 设置所有 backend 的线程数

Return ggml_backend_sched_graph_compute_async (sched.Get (), gf);

}

```

  

- `ggml_backend_sched_graph_compute_async` 是 GGML 的异步调度入口。

- `sched` 会按照图节点之间的依赖关系，把每个 `ggml_tensor` 分配到其对应的 backend 上执行；在 CPU/GPU 边界自动插入 `cpy` 节点做数据传输。

- 因为是 async，真正的计算在 `llama_get_logits_ith` 或 `llama_synchronize` 被调用时（或隐式同步时）才会完成。

  

#### Step 6.7 结果提取

  

`decode` 在 `process_ubatch` 返回后，从 `res->get_logits ()`（`ggml_tensor*`）异步拷贝数据到 host 的 `logits` 缓冲区：

  

```cpp

If (t_logits && n_outputs > 0) {

Ggml_backend_t backend_res = ggml_backend_sched_get_tensor_backend (sched.Get (), t_logits);

Float * logits_out = logits + n_outputs_prev*n_vocab;

Ggml_backend_tensor_get_async (backend_res, t_logits, logits_out, 0, n_outputs*n_vocab*sizeof (float));

}

```

  

`llama_get_logits_ith (ctx, -1)` 随后通过 `output_reorder ()` 确保顺序正确后，返回指向最后一个 token logits 的指针。

  

#### Step 6.8 Sampler 采样

  

```cpp

New_token_id = llama_sampler_sample (smpl, ctx, -1);

```

  

**完整调用链**：

1. `llama_get_logits_ith (ctx, -1)` -> 同步（如果需要）并返回 logits 指针。

2. 构造 `llama_token_data_array cur_p`，大小为 `n_vocab`，每个元素的 `logit` 填充原始值，`p` 初始为 0。

3. `llama_sampler_apply (smpl, &cur_p)` -> chain 依次调用每个 sampler。

- 以 `greedy` 为例：遍历 `cur_p.data`，找到最大 logit 的索引赋给 `cur_p.selected`。

1. `llama_sampler_accept (smpl, token)` -> 更新需要维护历史的 sampler（如 `penalties`、`grammar`、`mirostat`）。

  

### 3.7 阶段 7：资源释放

  

```cpp

Llama_sampler_free (smpl);

Llama_free (ctx);

Llama_model_free (model);

```

  

- `llama_sampler_free`：递归释放 chain 中所有 sampler。

- `llama_free`：`delete ctx`，触发 `llama_context` 析构函数，释放 `sched`、`backends`、`memory`、`logits/embd` 缓冲区等。

- `llama_model_free`：`delete model`，释放所有 `ggml_tensor` 及其 backend buffer。

  

---

  

## 4. 关键设计思想总结

  

### 4.1 三层 Batch 抽象

  

| 层级 | 数据结构 | 作用 |

|------|----------|------|

| 用户层 | `llama_batch` | 用户填充 token/pos/seq_id/logits |

| 分配层 | `llama_batch_allocr` | 自动补全、校验、拆分为 ubatch |

| 执行层 | `llama_ubatch` | 实际交给 graph compute 的物理 batch |

  

### 4.2 计算图复用 (Graph Reuse)

  

Llama. Cpp 的极致性能很大程度上来自于 **Graph Reuse**。Token Generation 阶段每次输入只有 1 个 token，图 topology 不变，因此可以直接复用前一次的 `ggml_cgraph` 和 `ggml_backend_sched` 的 buffer 分配，仅需更新 input tensor 的数据即可。这避免了重复的 `build_graph` 和 `alloc_graph` 开销。

  

### 4.3 Memory 抽象化

  

`llama_memory_i` 将 KV Cache 提升为通用的“序列状态存储”抽象，使得 llama. Cpp 能统一支持：

- 标准 Transformer 的 KV Cache

- Sliding Window Attention (SWA) 的 iSWA cache

- RWKV / Mamba 的 Recurrent State

- Hybrid 模型的混合 Memory

  

### 4.4 Backend 调度与多 GPU

  

`ggml_backend_sched` 是底层 GGML 提供的“自动分图 + 数据搬运”调度器。Llama. Cpp 只需在 `build_graph` 时通过 `cb` 回调指定某些 tensor 应该放在哪个 backend 上（例如 `model. Dev_layer (il)`），剩下的 split、pipeline parallelism、async copy 全部由 `sched` 自动处理。

  

### 4.5 simple. Cpp 的每一行对应了什么？

  

| simple. Cpp 代码 | 对应模块/文件 | 核心动作 |

|-----------------|---------------|----------|

| `ggml_backend_load_all ()` | `ggml-backend` | 注册 CUDA/Metal/Vulkan 等后端 |

| `llama_model_load_from_file` | `src/llama. Cpp` / `src/llama-model. Cpp` | GGUF 解析 + Tensor 映射 + Backend Buffer 分配 |

| `llama_tokenize` | `src/llama-vocab. Cpp` | 文本 -> token ids |

| `llama_init_from_model` | `src/llama-context. Cpp` | 创建 Context + 初始化 Memory + Reserve 计算图 |

| `llama_sampler_chain_init` / `add` | `src/llama-sampling. Cpp` | 构建采样链 |

| `llama_batch_get_one` | `src/llama-batch. Cpp` | 构造轻量 batch |

| `llama_decode` | `src/llama-context. Cpp` | Batch -> UBatch -> Graph Build/Reuse -> Compute -> 提取 Logits |

| `llama_sampler_sample` | `src/llama-sampling. Cpp` | Logits -> 采样 -> Token |

| `llama_token_to_piece` | `src/llama-vocab. Cpp` | Token -> 字符串 |

| `llama_free` / `llama_model_free` | 各结构体析构函数 | 释放所有资源 |

  

---

  

## 5. 附录：核心文件索引

  

| 文件 | 职责 |

|------|------|

| `include/llama. H` | C API 声明，所有用户可见的结构体和函数 |

| `src/llama. Cpp` | C API 实现入口：模型加载、backend 初始化、chat template、系统信息 |

| `src/llama-model. H/. Cpp` | `llama_model` 定义、GGUF 加载、`build_graph` |

| `src/llama-context. H/. Cpp` | `llama_context` 定义、decode/encode、graph compute、输出管理 |

| `src/llama-vocab. H/. Cpp` | `llama_vocab` 定义、tokenize、detokenize |

| `src/llama-batch. Cpp` | `llama_batch_allocr`、batch/ubatch 拆分逻辑 |

| `src/llama-sampling. Cpp` | 所有 sampler 实现（greedy、top_k、top_p、grammar 等）|

| `src/llama-graph. H` | `llm_graph_params`、`llm_graph_result`、`llm_graph_context`、图构建接口 |

| `src/llama-memory. H` | `llama_memory_i` 抽象接口 |

| `src/llama-kv-cache. Cpp` | 标准 KV Cache 实现 |

| `src/llama-hparams. H` | `llama_hparams` 定义 |

| `examples/simple/simple. Cpp` | 最小可运行示例 |

  

---

  

*文档生成时间：基于 llama. Cpp 当前代码库（截至 2026-04）。*