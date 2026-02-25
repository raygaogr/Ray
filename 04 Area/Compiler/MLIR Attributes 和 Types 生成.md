---
tags:
  - MLIR
  - compiler
title: MLIR Attributes 和 Types 生成
date: 2026-01-05 17:25
type:
---
## Attributes vs Types：核心区别

### **Types（类型）**

- 定义**SSA值的种类**（what kind of value）
- 运行时存在，描述计算的数据类型
- 例如：`i32`, `f64`, `tensor<4x8xf32>`, `memref<?xf32>`

### **Attributes（属性）**

- 定义**编译时常量数据**（compile-time constant metadata）
- 编译时存在，不参与运行时计算
- 例如：常量值、符号引用、配置参数、优化提示

## 共同基础设施：AttrOrTypeDef

两者共享相同的基础定义机制：
```c++
class AttrOrTypeDef<string valueType, string name, list<Trait> defTraits, string baseCppClass>
```

### 1. **核心字段**

- **`cppBaseClassName`**: C++基类名称
- **`description`**: 详细的人类可读描述
- **`storageClass`**: 存储类名称（默认：`name + valueType + "Storage"`）
- **`storageNamespace`**: 存储类命名空间（默认：`"detail"`）
- **`genStorageClass`**: 是否自动生成存储类
- **`hasStorageCustomConstructor`**: 是否有自定义存储构造函数

### 2. **参数系统（Parameters）**

```c++
dag parameters = (ins);
```

参数定义了Attribute/Type的内部数据：

#### **AttrOrTypeParameter类**

```c++
class AttrOrTypeParameter<string type, string desc, string accessorType = "">
```

核心字段：

- **`cppType`**: 参数的C++类型
- **`cppAccessorType`**: 访问器返回类型（可以不同于cppType）
- **`cppStorageType`**: 存储类型（如`std::string`存储`StringRef`）
- **`allocator`**: 自定义内存分配代码
- **`comparator`**: 相等性比较代码（默认用\=\=）
- **`convertFromStorage`**: 从存储类型转换的代码
- **`summary`**: 参数描述
- **`syntax`**: 汇编语法格式字符串
- **`parser`**: 自定义解析器（默认：`FieldParser<T>::parse`）
- **`printer`**: 自定义打印器（默认：`$_printer << $_self`）
- **`defaultValue`**: 默认值（使参数可选）

#### **常用参数类型**

**基础参数：**

- `AttrParameter` / `TypeParameter`: 基础参数类
- `OptionalParameter`: 可选参数（有默认值`T()`）
- `DefaultValuedParameter`: 带指定默认值的参数

**字符串参数：**

```c++
class StringRefParameter<string desc = "", string value = "">
```

- 需要内存分配（`copyInto`）
- 存储类型：`std::string`
- 访问类型：`StringRef`
- 内置字符串打印器

**数组参数：**

```c++
class ArrayRefParameter<string arrayOf, string desc = "">
```

- 需要内存分配
- 存储类型：`SmallVector<T>`
- 访问类型：`ArrayRef<T>`

```c++
class OptionalArrayRefParameter<string arrayOf, string desc = "">
```

- 可选数组（可以为空）
- 支持可选组解析

**特殊数值参数：**

```c++
class APFloatParameter<string desc>
```

- 使用位精确比较（`bitwiseIsEqual`）
- 用于浮点常量

**自分配参数：**

```c++
class SelfAllocationParameter<string type, string desc>
```

- 类型有自己的`allocateInto`方法

```c++
class ArrayRefOfSelfAllocationParameter<string arrayOf, string desc>
```

- 数组元素自己分配内存

**属性特殊参数：**

```c++
class AttributeSelfTypeParameter<string desc, string derivedType = "::mlir::Type", string typeBuilder = "">
```
- 表示属性的"自身类型"
- 从属性后的可选类型派生
- 默认值：`NoneType::get($_ctxt)`

### 3. **构建器（Builders）**

#### **默认构建器**

自动生成：

```c++
static <ClassName> get(MLIRContext *, <parameters>);
```

#### **自定义构建器**

```c++
class AttrOrTypeBuilder<dag parameters, code bodyCode = "",
string returnTypeStr = "">
```

- **`dagParams`**: DAG格式的参数列表
- **`body`**: 构建器实现代码
- **`returnType`**: 返回类型（默认为类本身）
- **`hasInferredContextParam`**: 是否从其他参数推断上下文

**上下文推断构建器：**

```c++
class AttrOrTypeBuilderWithInferredContext<dag parameters, code bodyCode = "", string returnType = "">
```

- 不需要显式传递`MLIRContext*`
- 从其他参数（如`Type`、`Attribute`）推断

**示例：**

```c++
AttrBuilder<(ins "int":$width, CArg<"float", "3.0f">:$height), [{ return $_get($_ctxt, width, height);}]>
```

特殊变量：

- **`$_get`**: 调用基类的get方法
- **`$_ctxt`**: MLIR上下文
- **`$_builder`**: Builder实例

控制选项：

- **`skipDefaultBuilders`**: 跳过默认构建器（必须提供自定义）

### 4. **特征（Traits）**

#### **Attribute特征**

```c++
class NativeAttrTrait<string name, code extraAttrDeclaration = [{}], code extraAttrDefinition = [{}]>
```

- 定义属性特定的行为
- 可添加额外声明和定义

```c++
class ParamNativeAttrTrait<string prop, string params>
class GenInternalAttrTrait<string prop>
class PredAttrTrait<string descr, Pred pred>
```

#### **Type特征**

```c++
class NativeTypeTrait<string name, code extraTypeDeclaration = [{}], code extraTypeDefinition = [{}]>
```

```c++
class ParamNativeTypeTrait<string prop, string params>
class GenInternalTypeTrait<string prop>
class PredTypeTrait<string descr, Pred pred>
```

**重要特征：**

```c++
def MutableType : NativeTypeTrait<"IsMutable">;
```

- 必须显式添加到可变类型
- 大多数MLIR类型是不可变的

### 5. **助记符和格式化**

- **`mnemonic`**: 文本关键字（用于解析/打印）
    - 例如：`"i"` for IntegerType, `"tensor"` for TensorType

#### **声明式格式**

```c++
string assemblyFormat = ?;
```

- 自动生成解析器和打印器
- 语法：
    - `` `<` `` - 字面量
    - `$param` - 参数引用
    - `^` - 可选组
    - 例如：`` `<` $width `x` $height `>` ``

#### **自定义格式**

```c++
bit hasCustomAssemblyFormat = 0;
```

- 设为1时生成`parse`和`print`方法声明
- 需要在C++中手动实现

### 6. **访问器和验证**

- **`genAccessors`**: 为每个参数生成getter方法（默认true）
- **`genVerifyDecl`**: 生成验证和`getChecked`方法
    - `getChecked`: 验证失败返回null
    - 用于构造时验证参数合法性

### 7. **额外代码注入**

- **`extraClassDeclaration`**: 类声明中的额外C++代码
- **`extraClassDefinition`**: 源文件中的额外C++代码
    - `$cppClass`会被替换为类名

## AttrDef：属性定义

```c++
class AttrDef<Dialect dialect, string name, list<Trait> traits = [], string baseCppClass = "::mlir::Attribute">
```

### **属性特定字段**

- **`cppClassName`**: C++类名（`name + "Attr"`）
- **`attrName`**: 唯一属性名（`dialect.name + "." + mnemonic`）
- **`convertFromStorage`**: 存储到返回类型的转换
    - 默认：`::llvm::cast<cppType>($_self)`
- **`predicate`**: 约束谓词
    - 默认：`::llvm::isa<cppType>($_self)`

### **特殊属性类型**

#### **LocationAttrDef**

```c++
class LocationAttrDef<Dialect dialect, string name, list<Trait> traits = []>
```

- 用于定义位置属性（源代码位置）
- 继承自`LocationAttr`
- 自动添加`IsLocation`特征

#### **ArrayOfAttr**

```c++
class ArrayOfAttr<Dialect dialect, string name, string attrMnemonic, string eltName, list<Trait> traits = []>
```

- 定义包含元素数组的属性
- 自动生成：
    - 迭代器方法：`begin()`, `end()`, `empty()`, `size()`
    - 访问方法：`front()`, `back()`, `operator[]`
    - 隐式转换：`operator ArrayRef<T>()`

示例格式：`[elem1, elem2, elem3]`

## TypeDef：类型定义

```c++
class TypeDef<Dialect dialect, string name, list<Trait> traits = [],string baseCppClass = "::mlir::Type">
```

### **类型特定字段**

- **`cppClassName`**: C++类名（`name + "Type"`）
- **`typeName`**: 唯一类型名（`dialect.name + "." + mnemonic`）
- **`builderCall`**: 无参数类型的常量构建器
    - 示例：`$_builder.getType<IntegerType>()`
- **`predicate`**: 约束谓词（类似属性）

## 实际应用示例

### **Attribute示例**

**整数属性：**

```c++
def IntegerAttr : AttrDef<SomeDialect, "Integer"> {
	let parameters = (ins "APInt":$value);
	let mnemonic = "int";
	let assemblyFormat = "$value";
}

// 使用：#dialect.int<42>
```

**数组属性：**

```c++
def ArrayAttr : AttrDef<SomeDialect, "Array"> {
	let parameters = (ins ArrayRefParameter<"Attribute">:$value);
	let mnemonic = "array";
	let assemblyFormat = "`[` $value `]`";
}

// 使用：#dialect.array[#int<1>, #int<2>, #int<3>]
```

**字符串属性：**

```c++
def StringAttr : AttrDef<SomeDialect, "String"> {
	let parameters = (ins StringRefParameter<"the string">:$value);
	let mnemonic = "str";
	let assemblyFormat = "$value";
}

// 使用：#dialect.str<"hello">
```

### **Type示例**

**简单类型（无参数）：**

```c++
def IndexType : TypeDef<SomeDialect, "Index"> {
	let mnemonic = "index";
}
// 使用：!dialect.index
```

**参数化类型：**

```c++
def IntegerType : TypeDef<SomeDialect, "Integer"> {
	let parameters = (ins "unsigned":$width);
	let mnemonic = "i";
	let assemblyFormat = "$width";
}
// 使用：!dialect.i32, !dialect.i64
```

**复杂类型：**

```c++
def TensorType : TypeDef<SomeDialect, "Tensor"> {
	let parameters = (ins
	ArrayRefParameter<"int64_t">:$shape,
	"Type":$elementType
	);
	let mnemonic = "tensor";
	let assemblyFormat = "`<` $shape `x` $elementType `>`";
}
// 使用：!dialect.tensor<4x8xf32>
```

## 存储机制

**存储类（Storage Class）：**

- 管理Attribute/Type的内部数据
- 存储在`MLIRContext`的唯一化池中
- 支持高效的值相等性比较和共享

**自动生成或自定义：**

- `genStorageClass = 1`: 自动生成
- `genStorageClass = 0`: 使用自定义存储类
- `hasStorageCustomConstructor`: 自定义构造逻辑

## Attributes vs Types：使用场景对比

|特性|Attributes|Types|
|---|---|---|
|**用途**|编译时常量、元数据|运行时值的类型|
|**示例**|常量42、符号名"foo"、配置参数|i32、f64、tensor<4xf32>|
|**关联**|附加到操作、块参数|定义SSA值的种类|
|**可变性**|不可变|通常不可变（需显式标记可变）|
|**默认基类**|`::mlir::Attribute`|`::mlir::Type`|
|**命名约定**|`NameAttr`|`NameType`|
|**语法前缀**|`#dialect.attr`|`!dialect.type`|

## 关键设计优势

1. **统一基础设施**: Attributes和Types共享相同的定义机制
2. **类型安全**: 通过TableGen生成类型安全的C++代码
3. **声明式**: 用TableGen声明，减少样板代码
4. **可扩展**: 方言可定义自己的Attributes和Types
5. **内存高效**: 使用唯一化存储池，避免重复
6. **参数化**: 支持泛型定义（如`tensor<NxMxT>`）
7. **验证**: 支持构造时验证
8. **格式化**: 自动或自定义的解析和打印

这个基础设施使得MLIR能够灵活地表示从高级语言到机器码各个层次的类型系统和元数据。


