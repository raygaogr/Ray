## llama Model

```c++
struct llama_model {
    llm_type type = LLM_TYPE_UNKNOWN;
    llm_arch arch = LLM_ARCH_UNKNOWN;

    std::string name = "n/a";

    llama_hparams hparams = {};
    llama_vocab   vocab;

    // for classifier models
    std::vector<std::string> classifier_labels;

    struct ggml_tensor * tok_embd   = nullptr;
    struct ggml_tensor * type_embd  = nullptr;
    struct ggml_tensor * pos_embd   = nullptr;
    struct ggml_tensor * tok_norm   = nullptr;
    struct ggml_tensor * tok_norm_b = nullptr;

    struct ggml_tensor * output_norm     = nullptr;
    struct ggml_tensor * output_norm_b   = nullptr;
    struct ggml_tensor * output          = nullptr;
    struct ggml_tensor * output_b        = nullptr;
    struct ggml_tensor * output_norm_enc = nullptr;

    // classifier
    struct ggml_tensor * cls       = nullptr;
    struct ggml_tensor * cls_b     = nullptr;
    struct ggml_tensor * cls_out   = nullptr;
    struct ggml_tensor * cls_out_b = nullptr;

    struct ggml_tensor * conv1d   = nullptr;
    struct ggml_tensor * conv1d_b = nullptr;

    // gemma3n altup
    struct ggml_tensor * tok_embd_per_layer   = nullptr;
    struct ggml_tensor * altup_proj           = nullptr;
    struct ggml_tensor * altup_unembd_proj    = nullptr;
    struct ggml_tensor * per_layer_model_proj = nullptr;
    struct ggml_tensor * per_layer_proj_norm  = nullptr;

    std::vector<llama_layer> layers;

    //Dense linear projections for SentenceTransformers models like embeddinggemma
    // For Sentence Transformers models structure see
    // https://sbert.net/docs/sentence_transformer/usage/custom_models.html#structure-of-sentence-transformer-models
    struct ggml_tensor * dense_2_out_layers = nullptr;
    struct ggml_tensor * dense_3_out_layers = nullptr;

    llama_model_params params;

    // gguf metadata
    std::unordered_map<std::string, std::string> gguf_kv;

    // list of devices used in this model
    std::vector<ggml_backend_dev_t> devices;

    // for quantize-stats only
    std::vector<std::pair<std::string, struct ggml_tensor *>> tensors_by_name;

    int64_t t_load_us  = 0;
    int64_t t_start_us = 0;

    explicit llama_model(const struct llama_model_params & params);
    ~llama_model();

    void load_stats  (llama_model_loader & ml);
    void load_arch   (llama_model_loader & ml);
    void load_hparams(llama_model_loader & ml);
    void load_vocab  (llama_model_loader & ml);
    bool load_tensors(llama_model_loader & ml); // returns false if cancelled by progress_callback

    std::string arch_name() const;
    std::string type_name() const;

    std::string desc() const;

    size_t size() const; // file size
    size_t n_tensors() const;
    size_t n_devices() const;

    std::map<ggml_backend_buffer_type_t, size_t> memory_breakdown() const;

    // total number of parameters in the model
    uint64_t n_elements() const;

    void print_info() const;

    ggml_backend_dev_t dev_layer(int il) const;
    ggml_backend_dev_t dev_output() const;

    ggml_backend_buffer_type_t select_buft(int il) const;

    bool has_tensor_overrides() const;

    const struct ggml_tensor * get_tensor(const char * name) const;

    float get_rope_freq_base (const llama_cparams & cparams, int il) const;
    float get_rope_freq_scale(const llama_cparams & cparams, int il) const;

    ggml_tensor * get_rope_factors(const llama_cparams & cparams, int il) const;

    // note: can mutate `cparams`
    // TODO: move this to new llm_arch_model_i interface
    llama_memory_i * create_memory(const llama_memory_params & params, llama_cparams & cparams) const;

    // TODO: move this to new llm_arch_model_i interface
    ggml_cgraph * build_graph(const llm_graph_params & params) const;

private:
    struct impl;
    std::unique_ptr<impl> pimpl;
};
```

## Backend device property
```c++
struct ggml_backend_dev_props {  
    // device name  
    const char * name;  
    // device description  
    const char * description;  
    // device free memory in bytes  
    size_t memory_free;  
    // device total memory in bytes  
    size_t memory_total;  
    // device type  
    enum ggml_backend_dev_type type;  
    // device id  
    //   for PCI devices, this should be the PCI bus id formatted as "domain:bus:device.function" (e.g. "0000:01:00.0")    //   if the id is unknown, this should be NULL    const char * device_id;  
    // device capabilities  
    struct ggml_backend_dev_caps caps;  
};
```

## 获取后端注册器
首先这是一个静态函数，说明该函数只在当前文件中可见，其他文件无法调用，一方面是防止与其他文件的同名函数冲突，另一方面可以隐藏函数实现细节。第二个 `static` 在函数内部用于单例模式，只初始化一次，第一次调用 `get_reg()` 时创建对象，而且生命周期一直存活在函数结束。
```c++
static ggml_backend_registry & get_reg() {  
    static ggml_backend_registry reg;  
    return reg;  
}
```

## 后端注册器
后端注册器在创建实例时，会自动地注册所有后端，包括后端 reg 以及后端设备。
```c++
struct ggml_backend_reg_entry {
  ggml_backend_reg_t reg;
  dl_handle_ptr handle;
};

struct ggml_backend_registry {  
    std::vector<ggml_backend_reg_entry> backends;  
    std::vector<ggml_backend_dev_t> devices;
    
    ggml_backend_registry() {  
        register_backend(ggml_backend_cuda_reg());
        register_backend(ggml_backend_cpu_reg());
    }
}
```

## 后端注册器实例
```c++
struct ggml_backend_reg {  
    int api_version; // initialize to GGML_BACKEND_API_VERSION  
    struct ggml_backend_reg_i iface;  
    void * context;  
};
```
## 后端注册
```c++
struct ggml_backend_reg {  
    int api_version; // initialize to GGML_BACKEND_API_VERSION  
    struct ggml_backend_reg_i iface;  
    void * context;  
};
```
 
 
## 后端注册接口
```c++
static const ggml_backend_reg_i ggml_backend_cuda_reg_interface = {  
    /* .get_name          = */ ggml_backend_cuda_reg_get_name,  
    /* .get_device_count  = */ ggml_backend_cuda_reg_get_device_count,  
    /* .get_device        = */ ggml_backend_cuda_reg_get_device,  
    /* .get_proc_address  = */ ggml_backend_cuda_reg_get_proc_address,  
};
```
##  后端上下文
```c++
struct ggml_backend_cuda_reg_context {  
    std::vector<ggml_backend_dev_t> devices;  
};
```

## 后端设备上下文
```c++
struct ggml_backend_cuda_device_context {  
    int device;  
    std::string name;  
    std::string description;  
    std::string pci_bus_id;  
};
```

## 后端设备
```c++
struct ggml_backend_device {  
    struct ggml_backend_device_i iface;  
    ggml_backend_reg_t reg;  
    void * context;  
};
```

## 后端设备功能接口
```c++
static const ggml_backend_device_i ggml_backend_cuda_device_interface = {  
    /* .get_name                = */ ggml_backend_cuda_device_get_name,  
    /* .get_description         = */ ggml_backend_cuda_device_get_description,  
    /* .get_memory              = */ ggml_backend_cuda_device_get_memory,  
    /* .get_type                = */ ggml_backend_cuda_device_get_type,  
    /* .get_props               = */ ggml_backend_cuda_device_get_props,  
    /* .init_backend            = */ ggml_backend_cuda_device_init_backend,  
    /* .get_buffer_type         = */ ggml_backend_cuda_device_get_buffer_type,  
    /* .get_host_buffer_type    = */ ggml_backend_cuda_device_get_host_buffer_type,  
    /* .buffer_from_host_ptr    = */ NULL,  
    /* .supports_op             = */ ggml_backend_cuda_device_supports_op,  
    /* .supports_buft           = */ ggml_backend_cuda_device_supports_buft,  
    /* .offload_op              = */ ggml_backend_cuda_device_offload_op,  
    /* .event_new               = */ ggml_backend_cuda_device_event_new,  
    /* .event_free              = */ ggml_backend_cuda_device_event_free,  
    /* .event_synchronize       = */ ggml_backend_cuda_device_event_synchronize,  
};
```
## 获取后端设备属性
```c++
void ggml_backend_dev_get_props(ggml_backend_dev_t device, struct ggml_backend_dev_props * props) {  
    memset(props, 0, sizeof(*props));  
    device->iface.get_props(device, props);  
}
```
## 后端设备属性

```c++
struct ggml_backend_dev_props {  
    // device name  
    const char * name;  
    // device description  
    const char * description;  
    // device free memory in bytes  
    size_t memory_free;  
    // device total memory in bytes  
    size_t memory_total;  
    // device type  
    enum ggml_backend_dev_type type;  
    // device id  
    //   for PCI devices, this should be the PCI bus id formatted as "domain:bus:device.function" (e.g. "0000:01:00.0")    //   if the id is unknown, this should be NULL    
    const char * device_id;  
    // device capabilities  
    struct ggml_backend_dev_caps caps;  
};
```

## 加载模型
```c++
static struct llama_model * llama_model_load_from_file_impl( 
        const std::string & path_model,  
        std::vector<std::string> & splits,  
        struct llama_model_params params);
```
## Llama 模型
```c++
struct llama_model {
llm_type type = LLM_TYPE_UNKNOWN;  
llm_arch arch = LLM_ARCH_UNKNOWN;  
  
llama_hparams hparams = {};  
llama_vocab   vocab;  
  
// for classifier models  
std::vector<std::string> classifier_labels;  
  
struct ggml_tensor * tok_embd   = nullptr;  
struct ggml_tensor * type_embd  = nullptr;  
struct ggml_tensor * pos_embd   = nullptr;  
struct ggml_tensor * tok_norm   = nullptr;  
struct ggml_tensor * tok_norm_b = nullptr;  
  
struct ggml_tensor * output_norm     = nullptr;  
struct ggml_tensor * output_norm_b   = nullptr;  
struct ggml_tensor * output          = nullptr;  
struct ggml_tensor * output_b        = nullptr;  
struct ggml_tensor * output_norm_enc = nullptr;  
  
// classifier  
struct ggml_tensor * cls       = nullptr;  
struct ggml_tensor * cls_b     = nullptr;  
struct ggml_tensor * cls_out   = nullptr;  
struct ggml_tensor * cls_out_b = nullptr;  
  
struct ggml_tensor * conv1d   = nullptr;  
struct ggml_tensor * conv1d_b = nullptr;  
  
// gemma3n altup  
struct ggml_tensor * tok_embd_per_layer   = nullptr;  
struct ggml_tensor * altup_proj           = nullptr;  
struct ggml_tensor * altup_unembd_proj    = nullptr;  
struct ggml_tensor * per_layer_model_proj = nullptr;  
struct ggml_tensor * per_layer_proj_norm  = nullptr;  
  
std::vector<llama_layer> layers;  
  
struct ggml_tensor * dense_2_out_layers = nullptr;  
struct ggml_tensor * dense_3_out_layers = nullptr;  
  
llama_model_params params;  
  
// gguf metadata  
std::unordered_map<std::string, std::string> gguf_kv;  
  
// list of devices used in this model  
std::vector<ggml_backend_dev_t> devices;  
  
// for quantize-stats only  
std::vector<std::pair<std::string, struct ggml_tensor *>> tensors_by_name;  
  
int64_t t_load_us  = 0;  
int64_t t_start_us = 0;
	explicit llama_model(const struct llama_model_params & params);
private:  
    struct impl;  
    std::unique_ptr<impl> pimpl;
}
```

## 加载模型实现
```c++
static int llama_model_load(const std::string & fname, std::vector<std::string> & splits, llama_model & model, llama_model_params & params)
```

## 模型加载器
```c++
struct llama_model_loader {
	gguf_context_ptr meta;
	std::vector<ggml_context_ptr> contexts;
	std::map<std::string, llama_tensor_weight, weight_name_comparer> weights_map;
	meta.reset(gguf_init_from_file(fname.c_str(), params));
}
```

## 模型上下文
```c++
struct gguf_context {  
    uint32_t version = GGUF_VERSION;  
  
    std::vector<struct gguf_kv> kv;  
    std::vector<struct gguf_tensor_info> info;  
  
    size_t alignment = GGUF_DEFAULT_ALIGNMENT;  
    size_t offset    = 0; // offset of `data` from beginning of file  
    size_t size      = 0; // size of `data` in bytes  
  
    void * data = nullptr;  
};
```

## 张量
```c++
struct ggml_tensor {  
    enum ggml_type type;  
  
    struct ggml_backend_buffer * buffer;  
  
    int64_t ne[GGML_MAX_DIMS]; // number of elements  
    size_t  nb[GGML_MAX_DIMS]; // stride in bytes:  
                               // nb[0] = ggml_type_size(type)                               
                               // nb[1] = nb[0]   * (ne[0] / ggml_blck_size(type)) + padding                               
                               // nb[i] = nb[i-1] * ne[i-1]  
    // compute data    
    enum ggml_op op;  
  
    // op params - allocated as int32_t for alignment  
    int32_t op_params[GGML_MAX_OP_PARAMS / sizeof(int32_t)];  
  
    int32_t flags;  
  
    struct ggml_tensor * src[GGML_MAX_SRC];  
  
    // source tensor and offset for views  
    struct ggml_tensor * view_src;  
    size_t               view_offs;  
  
    void * data;  
  
    char name[GGML_MAX_NAME];  
  
    void * extra; // extra things e.g. for ggml-cuda.cu  
  
    char padding[8];  
};
```