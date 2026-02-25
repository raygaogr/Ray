---
tags:
  - MLIR
  - tpu
title: TPU-MLIR 源码阅读
date: 2026-01-14 11:38
type: permanent-note
---
---
## **1. model_transform.py** 

### 1.1 模型转换器
基于源模型的类型选择具体的前端模型转换工具，比如：`OnnxTransformer` , `MlirTransformer`, `TorchTransformer` 等
具体的 Transformer 继承自基类 `ModelTransformer`
#### `Class ModelTransform` 核心内容
```python
class ModelTransformer(object):
	def __init__(self, model_name, model_def):
		self.converter = BaseConverter()
				
	# 将前端的计算图转换成mlir的计算图
	def model_mlir(self):
		self.converter.generate_mlir("xxxx.mlir")
		
	# 模型转换的核心函数
	def model_transform(self):
		pass
		
	# 模型结果验证，转换后的模型与转换前的输出结果对比
	def model_validate(self):
		pass
	
	# 子类需要实现的抽象方法，用于模型的推理
	@abc.abstractmethod
	def origin_inference(self, inputs: dict):
		pass
```

#### `Class OnnxTransformer` 核心内容
继承自 `ModelTransformer` , 实现基于 onnx 的 origin_inference 方法。并将模型转换器设置成具体的 onnx 转换器
```python
class OnnxTransformer(ModelTransformer)：
	def __init__(self, )
		super().__init__()
		self.converter = OnnxConverter()
	
	def origin_inference(self, inputs):
		onnx_inference(inputs, self.converter.onnx_file)
```

#### `Method model_transform` 核心内容

```python
def model_transform(self):
	# 将 onnx 模型转换成 topDialect 的表达
	mlir_origin = self.converter.generate_mlir(mlir_file)
	
	# 针对top Dialect进行优化，调用tpuc-opt的优化pass
	patterns = mlir_opt_for_top(mlir_origin, self.mlir_file)
	
	# 对优化后的 mlir 进行解析
	self.module_parsered = MlirParser(self.mlir_file)
```

### 1.2 前端转换器
#### `Class` BaseConverter 核心内容

前端转换的基类，以计算图转换为中心，提供大量前端转换的方法
```python
class BaseConverter(object):
	def __init__(self):
		self.operands = dict() # 操作数
		self.tensors = dict() # 张量
		self.shapes = dict() # 张量的shape
		self.input_names = list() # 输入名
		self.output_names = list() # 输出名
	
	# mlir的生成方法，具体的converter有具体的实现
	def generate_mlir(self):
		pass
	
	# 给节点添加shape
	def addShape(self):
		pass
	
	# 添加操作节点
	def addOperand(self):
		pass
		
	# 添加权重信息，会增加张量以及对应的shape
	def addWeight(self):
		pass
		
	# 核心方法，创建weightop，生成weightop的mlir
	def getWeightOp(self):
		pass
	
```

#### `class OnnxConverter` 核心内容

整个模型转换过程中的核心类，提供了具体的 operation 的转换方法
```python

class OnnxConverter(BaseConverter):
	def __init__(self):
		self.load_onnx_model(onnx_file)
		self.MLIRImporter()
	
	# 加载onnx模型，并对onnx模型进行图优化，包括两部分，一是基于onnxsim库的优化，另一个是使用onnx_opt进行优化
	def load_onnx_model(self):
		self.select_output(output_names)
		self.model_simplify(input_shapes)
		self.addWeight()
		self.model, self.node_name_mapping = onnx_opt(self.model, dump_final_opt)
	
	# 生成初始的mlir文件
	def init_MLIRImporter(self):
		self.mlir = MLIRImporter(xxxxx)

```

#### `Method onnx_opt` 核心内容

进行 onnx 的图进行自定义的图优化
```python
def onnx_opt(model, dump=False, rigorous=True):
    # 移除常量输入
	remove_tensor_from_input(model)
	
	# 模式匹配的图优化
	pattern_functions = [
		TorchLayerNormPattern, # LayerNorm
		TorchPixelNormPattern, # PixelNorm
		TorchHardSigmoidPattern, # HardSigmoid
		TorchHardSwishPattern, # HardSwish
		TorchHardSwishPattern2, 
		TorchGELUPattern, # GELU
		TorchGELUPattern2,
	]
	# 进行模式匹配和替换
	graph_opt() 
```

### 1.3 MLIR 转换器
#### `class MLIRImporter`

用于 MLIR 环境的初始化，并提供 mlir 中 op 的创建工具
```python
class MLIRImporter(object):
	def __init__(self):
	 	self.ctx = Context()
		self.ctx.allow_unregistered_dialects = True
		self.loc = Location.unknown(self.ctx)
		self.ctx.__enter__()
		self.loc.__enter__()
		# 声明初始化的计算图入口，是一个MLIR的module对象
		self.declare_func(input_types, output_types)
	
	# 创建输入节点
	def create_input_op(self, loc, index, kargs: dict = {}):
		pass
	
	# 创建权重节点
	def create_weight_op(self, name, output_shape, data_type="F32", path=None):
		pass
```