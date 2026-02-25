---
tags:
title: TPU-MLIR 前端
date: 2025-12-29 10:43
type:
---

---
## OnnxTransformer

```python
class OnnxTransformer([ModelTransformer](TPU-MLIR#ModelTransformer)):
	def __init__(self,
	    model_name,
	    model_def, ...):
		super().__init__(model_name, model_def)
		self.converter = OnnxConverter(...)
	  
	def origin_inference(self, inputs: dict):
		return onnx_inference(inputs, self.converter.onnx_file)
```

## ModelTransformer

```python
class ModelTransformer(object):
	def __init__(self, model_name, model_def):
		self.model_name = model_name
		self.model_def = model_def
		self.do_mlir_infer = True
		self.converter = BaseConverter()
		self.in_f32_npz = self.model_name + '_in_f32.npz'
		self.ref_npz = self.model_name + '_ref_outputs.npz'
		self.file_recorder = None
```