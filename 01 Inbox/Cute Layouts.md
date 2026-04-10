---
tags:
  - CuTe
title: Cute Layouts
date: 2026-02-28 08:42
type: permanent-note
---
---
`Layout` 本质上是一个函数映射，将逻辑坐标映射到实际的物理内存地址上，CuTe 的 `Layout` 提供了公共接口来访问多维张量，它会隐藏实际物理内存排布的细节，因此当张量的 Layout 发生变化后，用户的代码也可以不用变化。
## 基本类型和概念
### 整型
CuTe 将整型显示区分成动态（run-time）和静态（compile-time）两种类型：
- 动态整型是 c++ 中普通的整型，如 `int` 和 `size_t` 等；
- 静态整型常见表示成 `Int<1>`, `Int<2>` 或者 `_1`, `_2`，静态整型可以转换成动态整型，因此它可以和动态整型一起出现在表达式中；
### Tuple
`Tuple` 是一组有序且有限的零个或多个元素集合。
### IntTuple
常见的整型 Tuple 有：
- int{2}
- Int<3>{}
- make_tuple (int{2}, Int<3>{})
- make_tuple (uint16_t{42}, make_tuple (Int<1>{}, int32_t{3}), Int<17>{})
`CuTe` 将 `IntTuple` 的概念应用到很多场景，包括 `Shape`，`Stride` ，`Step` 和 `Coord`
`IntTuple` 常用的操作有：
- `rank(IntTuple)`: `IntTuple` 中的元素的数量，相当于 `tuple_size`；
- `get<I>(IntTuple)`: 获取 `IntTuple` 中的第 `I` 个元素
- `depth(IntTuple)`: `IntTuple` 的层数，单个整数的 depth 为 0，一个 tuple 的 depth 为 1，一个 tuple 嵌套另一个 tuple 的 depth 为 2，以此类推
- `size(IntTuple)`: `IntTuple` 中所有元素的乘积
### Shapes 和 Strides
都是复用的 `IntTuple` 的概念
### Layout
`Layout` 是 `Shape` 和 `Stride` 组成的 Tuple，形如(`Shape`， `Stride`)，语义上，它会将 Shape 中的坐标基于 Stride 映射成一个索引。
### Tensor
`Tensor` 是由 `Layout` 和实际的数据组成，这个数据可以是 `pointer` 或者是数组

---
## Layout 的创建和使用
`Layout` 常用的操作：
- `rank(Layout)`: Layout 中 modes 的数量，等于 `Layout` 的 `shape` 的 tuple size 
- `get<I>(Layout)`: 获取第 `I` 个 Layout
- `depth(Layout)`: `Layout` 的 `shape ` 的 depth
- `shape(Layout)`: `Layout` 的 `shape`
- `stride(Layout)`: `Layout` 的 `stride`
- `size(Layout)`: `Layout` 函数的定义域的大小，它表示这个 `Layout` 总共描述了多少个逻辑元素。无论这些元素在内存中是如何排列的（是紧密排列还是中间有空隙），逻辑上一共有多少个位置，`size` 就是多少。等价于 `size(shape(Layout))`
- `cosize(Layout)`: `Layout` 函数的陪域的大小，函数可能返回的值的范围。它通常是一个区间 `[0， M-1]`，但函数可能并没有使用其中的每一个值（如果存在空隙），`cosize` 描述了为了容纳这些逻辑元素，**至少需要多大的“内存空间”**。它计算的是**最后一个元素的地址 + 1**，等价于 `A(size(A) - 1) + 1`
### 多层次访问函数
- `get<I0,I1,...,IN>(x) := get<IN>(...(get<I1>(get<I0>(x)))...)`：获取 x 中第 `I0` 个元素中的第 `I1` 中的 ... 第 `IN` 个元素，【示例】**假设 `x` 是一个三维嵌套的 `IntTuple`：`x = [ [ [a, b], [c, d] ], [ [e, f], [g, h] ] ]`，那么：`get<0,1,0>(x)` 表示先取外层第 0 个元素（第一个大块），再取其中第 1 个元素），最后取该子块的第 0 个元素。最终得到 `[e, f]`**； 
- `rank<I...>(x) := rank(get<I...>(x))`：x 的第 `I...` 个元素的 rank；
- `depth<I...>(x) := depth(get<I...>(x))`：x 的第 `I...` 个元素的 depth
- `shape<I...>(x) := shape(get<I...>(x))`: x 的第 `I...` 个元素的 shape
- `size<I...>(x) := size(get<I...>(x))`：x 的第 `I...` 个元素的 size
### 构建 Layout
```C++
Layout s8 = make_layout(Int<8>{});
Layout d8 = make_layout(8);

Layout s2xs4 = make_layout(make_shape(Int<2>{},Int<4>{}));
Layout s2xd4 = make_layout(make_shape(Int<2>{},4));

Layout s2xd4_a = make_layout(make_shape (Int< 2>{},4),
                             make_stride(Int<12>{},Int<1>{}));
Layout s2xd4_col = make_layout(make_shape(Int<2>{},4),
                               LayoutLeft{});
Layout s2xd4_row = make_layout(make_shape(Int<2>{},4),
                               LayoutRight{});

Layout s2xh4 = make_layout(make_shape (2,make_shape (2,2)),
                           make_stride(4,make_stride(2,1)));
Layout s2xh4_col = make_layout(shape(s2xh4),
                               LayoutLeft{});
```
如果创建 Layout 时只提供了 Shape，没有提供 Stride，那么 CuTe 会自动根据 Shape **生成一个默认的 Stride**。默认策略是 `LayoutLeft`，但也可以显式指定 `LayoutRight` 来改变生成方式。无论 `LayoutLeft` 还是 `LayoutRight`，它们的生成规则都基于 **exclusive prefix product（不包含当前维度的前缀乘积）**。意思是：
- 对于一组维度大小 `d0, d1, d2, ..., dn-1`（按某种顺序排列），
- 每个维度的步长等于排在它**前面（或后面）**所有维度大小的乘积，但**不包括当前维度本身**。
### 使用 Layout
`Layout` 本质就是一个映射函数，把 `Shape` 的坐标基于 `Stride` 映射成一个 index，这个 index 可用于 data 的寻址
```c++
template <class Shape, class Stride>
void print2D(Layout<Shape,Stride> const& layout)
{
  for (int m = 0; m < size<0>(layout); ++m) {
    for (int n = 0; n < size<1>(layout); ++n) {
      printf("%3d  ", layout(m,n));
    }
    printf("\n");
  }
}
```
## Layout 概念
### Layout 兼容性
`Layout` A 与 `Layout` B 兼容是指 `Shape` A 与 `Shape` B 兼容，它们兼容需要满足下面两个条件：
- 1、A 的 size 和 B 的 size 相等
- 2、A 中的所有坐标在 B 中是有效的，即对于 A 的每个逻辑坐标（可能是多维或嵌套的），该坐标在 B 的坐标空间中也是合法的。
举例说明：
- Shape 24 和 Shape 32 不兼容
- Shape 24 与 Shape (4, 6) 兼容
- Shape (4, 6) 与 Shape ((2, 2), 6) 兼容
- Shape ((2, 2), 6) 与 Shape ((2, 2), (3, 2)) 兼容
- Shape 24 与 Shape ((2, 3), 4) 兼容
- Shape ((2, 3), 4) 与 Shape ((2,2), (3,2)) **不**兼容
- Shape 24 与 Shape (24) 兼容
- Shape (24) 与 Shape 24 **不兼容**
- Shape (24) 与 Shape (4, 6) **不兼容**
### Layout 坐标
布局 `L` 的逻辑元素集合是固定的（比如 6 个元素），这些元素可以用多种方式来索引。例如，一个 2×3 的矩阵可以用二维坐标 `(i,j)` 访问，也可以用一维坐标 `k`（按某种顺序展开）访问，只要一维坐标与二维坐标之间有一一对应关系。由于 `(2,3)` 与 `(6,)` 大小相等，并且存在合法的坐标映射（例如行主序或列主序），所以布局 `L` 既能接受二维坐标，也能接受一维坐标。
#### 坐标映射
```c++
auto shape = Shape<_3,Shape<_2,_3>>{};
print(idx2crd(   16, shape));                                // (1,(1,2))
print(idx2crd(_16{}, shape));                                // (_1,(_1,_2))
print(idx2crd(make_coord(   1,5), shape));                   // (1,(1,2))
print(idx2crd(make_coord(_1{},5), shape));                   // (_1,(1,2))
print(idx2crd(make_coord(   1,make_coord(1,   2)), shape));  // (1,(1,2))
print(idx2crd(make_coord(_1{},make_coord(1,_2{})), shape));  // (_1,(1,_2))
```
#### Index 映射
```c++
auto shape  = Shape <_3,Shape<  _2,_3>>{};
auto stride = Stride<_3,Stride<_12,_1>>{};
print(crd2idx(   16, shape, stride));       // 17
print(crd2idx(_16{}, shape, stride));       // _17
print(crd2idx(make_coord(   1,   5), shape, stride));  // 17
print(crd2idx(make_coord(_1{},   5), shape, stride));  // 17
print(crd2idx(make_coord(_1{},_5{}), shape, stride));  // _17
print(crd2idx(make_coord(   1,make_coord(   1,   2)), shape, stride));  // 17
print(crd2idx(make_coord(_1{},make_coord(_1{},_2{})), shape, stride));  // _17
```
## Layout 操作
### Sublayouts
```c++
Layout a   = Layout<Shape<_4,Shape<_3,_6>>>{}; // (4,(3,6)):(1,(4,12))
Layout a0  = layout<0>(a);                     // 4:1
Layout a1  = layout<1>(a);                     // (3,6):(4,12)
Layout a10 = layout<1,0>(a);                   // 3:4
Layout a11 = layout<1,1>(a);                   // 6:12
```

```c++
Layout a   = Layout<Shape<_2,_3,_5,_7>>{};     // (2,3,5,7):(1,2,6,30)
Layout a13 = select<1,3>(a);                   // (3,7):(2,30)
Layout a01 = select<0,1,3>(a);                 // (2,3,7):(1,2,30)
Layout a2  = select<2>(a);                     // (5):(6)
```

```c++
Layout a   = Layout<Shape<_2,_3,_5,_7>>{};     // (2,3,5,7):(1,2,6,30)
Layout a13 = take<1,3>(a);                     // (3,5):(2,6)
Layout a14 = take<1,4>(a);                     // (3,5,7):(2,6,30)
// take<1,1> not allowed. Empty layouts not allowed.
```

### 拼接
```c++
Layout a = Layout<_3,_1>{};                     // 3:1
Layout b = Layout<_4,_3>{};                     // 4:3
Layout row = make_layout(a, b);                 // (3,4):(1,3)
Layout col = make_layout(b, a);                 // (4,3):(3,1)
Layout q   = make_layout(row, col);             // ((3,4),(4,3)):((1,3),(3,1))
Layout aa  = make_layout(a);                    // (3):(1)
Layout aaa = make_layout(aa);                   // ((3)):((1))
Layout d   = make_layout(a, make_layout(a), a); // (3,(3),3):(1,(1),1)
```

```c++
Layout a = Layout<_3,_1>{};                     // 3:1
Layout b = Layout<_4,_3>{};                     // 4:3
Layout ab = append(a, b);                       // (3,4):(1,3)
Layout ba = prepend(a, b);                      // (4,3):(3,1)
Layout c  = append(ab, ab);                     // (3,4,(3,4)):(1,3,(1,3))
Layout d  = replace<2>(c, b);                   // (3,4,4):(1,3,3)
```
### 分组和拉平

```c++
Layout a = Layout<Shape<_2,_3,_5,_7>>{};  // (_2,_3,_5,_7):(_1,_2,_6,_30)
Layout b = group<0,2>(a);                 // ((_2,_3),_5,_7):((_1,_2),_6,_30)
Layout c = group<1,3>(b);                 // ((_2,_3),(_5,_7)):((_1,_2),(_6,_30))
Layout f = flatten(b);                    // (_2,_3,_5,_7):(_1,_2,_6,_30)
Layout e = flatten(c);                    // (_2,_3,_5,_7):(_1,_2,_6,_30)
```

## CuTe Layout 代数运算
Layout 的代数运算包括：
- `Layout` 函数的组合
- `Layout` 的“product”重新产生一个新的 `Layout`
- `Layout` 的“divide”拆分一个 `Layout` 产生新的

### Coalesce 运算
在 CuTe 中，`coalesce` 是一个用于简化布局表示的操作，它基于一个核心观点：**布局本质上是一个从整数到整数的函数**。理解这一点是掌握 `coalesce` 的关键。
`coalesce` 允许我们**重新组织布局的维度结构（Shape）和步长（Stride），但保持这个整数到整数的映射关系不变**。也就是说，我们可以改变布局的“模样”（比如把多个维度合并成一个，或者调整维度的划分），却**不改变每个逻辑元素最终对应的内存偏移量**。`coalesce` 是 CuTe 中一个重要的“规范化”工具，它剥离了布局的多维“外衣”，聚焦于其本质的整数函数，从而在保持映射不变的前提下简化表示。

**两个模式 Coalesce 规则**:
现在考虑一个只有两个整数模式的布局，记为 `(s0, s1) : (d0, d1)`。我们希望将这两个模式合并成一个或保留原样，定义合并操作 `s0:d0 ++ s1:d1` 的结果。共有四种情况：

**情况 1：`s0:d0 ++ _1:d1 => s0:d0`**
- 第二个模式的大小为 1（即 `s1 = 1`）。
- 根据上述理由，第二个模式可忽略，合并后只剩下第一个模式，其大小和步长不变。
 **情况 2：`_1:d0 ++ s1:d1 => s1:d1`**
- 第一个模式的大小为 1。
- 忽略第一个模式，合并后只剩下第二个模式。
 **情况 3：`s0:d0 ++ s1:s0*d0 => s0*s1:d0`**
- 这是可以合并的关键情况。条件：第二个模式的步长 `d1` 恰好等于第一个模式的大小 `s0` 乘以第一个模式的步长 `d0`，即 `d1 = s0 * d0`。
- **为什么可以合并？**  
    让我们分析这个布局作为整数函数的映射。对于任意整数逻辑序号 `x`（0 ≤ x < s0 \* s1），我们可以将它分解为两个模式的坐标：
    - 设 `x = i * s0 + j`，其中 `0 ≤ i < s1`（第二个模式的索引），`0 ≤ j < s0`（第一个模式的索引）。这里我们采用列主序的展开方式（即先变第一个模式），因为步长条件正是这种展开对应的自然顺序。
    - 实际的内存偏移为：`offset = j * d0 + i * d1 = j * d0 + i * (s0 * d0) = (j + i * s0) * d0 = x * d0`。
    - 因此，偏移直接等于 `x * d0`，与将两个模式合并为一个大小为 `s0*s1`、步长为 `d0` 的单一模式所得结果完全一致。
- 所以，这两个模式可以合并成一个新模式，大小为两模式大小之积，步长保持为第一个模式的步长。
**情况 4：`s0:d0 ++ s1:d1 => (s0,s1):(d0,d1)`**
- 如果以上条件都不满足，则无法合并，必须保留两个独立的模式。结果就是一个由两个模式组成的布局，其形状为 `(s0, s1)`，步长为 `(d0, d1)`。
#### By-mode Coalesce

默认的 `coalesce` 操作将布局视为一个从整数（逻辑元素序号）到整数（内存偏移）的函数，并尽可能合并相邻的模式以简化表示。这种简化可能会改变布局的维度数量（例如将二维布局合并为一维）。然而，在某些场景下，我们**希望布局仍然具有特定的多维形状**（例如保持二维），因为后续算法可能依赖于这种形状（比如循环嵌套、分块操作等）。因此，需要一种方式在合并时保留目标轮廓。

```c++
Layout coalesce(Layout const& layout, IntTuple const& trg_profile)
```

- **`layout`**：待合并的原始布局，可能具有嵌套结构。
- **`trg_profile`**：一个 `IntTuple`（例如 `Step<_1,_1>`），它描述了期望的结果形状的“轮廓”。这个轮廓中的每个元素可以是整数（如 `_1`）或嵌套的元组。整数值本身并不重要（只是作为标志），它们告诉函数：**在对应层次上对子布局应用 `coalesce`**。
关键思想是**递归处理**：
- 遍历 `trg_profile` 的结构。
- 当遇到一个整数（例如 `_1`）时，就对当前层次的对应子布局调用默认的 `coalesce`（即视为一维整数函数进行合并），并返回合并后的结果。
- 当遇到一个元组时，则继续递归进入下一层，对子布局的对应部分进一步按轮廓合并。
最终，返回的布局具有与 `trg_profile` 相同的嵌套结构，但每个叶子节点（对应整数标志处）的子布局被合并简化。

### Composition 运算
示例：
```c++
Functional composition, R := A o B
R(c) := (A o B)(c) := A(B(c))

Example
A = (6,2):(8,2)
B = (4,3):(3,1)

R( 0) = A(B( 0)) = A(B(0,0)) = A( 0) = A(0,0) =  0
R( 1) = A(B( 1)) = A(B(1,0)) = A( 3) = A(3,0) = 24
R( 2) = A(B( 2)) = A(B(2,0)) = A( 6) = A(0,1) =  2
R( 3) = A(B( 3)) = A(B(3,0)) = A( 9) = A(3,1) = 26
R( 4) = A(B( 4)) = A(B(0,1)) = A( 1) = A(1,0) =  8
R( 5) = A(B( 5)) = A(B(1,1)) = A( 4) = A(4,0) = 32
R( 6) = A(B( 6)) = A(B(2,1)) = A( 7) = A(1,1) = 10
R( 7) = A(B( 7)) = A(B(3,1)) = A(10) = A(4,1) = 34
R( 8) = A(B( 8)) = A(B(0,2)) = A( 2) = A(2,0) = 16
R( 9) = A(B( 9)) = A(B(1,2)) = A( 5) = A(5,0) = 40
R(10) = A(B(10)) = A(B(2,2)) = A( 8) = A(2,1) = 18
R(11) = A(B(11)) = A(B(3,2)) = A(11) = A(5,1) = 42
```
上面的 `B` 和 `R` 是兼容的，因为函数 B 的值域是函数 R 的作用域
#### **计算组合**

**组合的基本性质与简化**
首先，给出几个观察，用于简化组合的计算：
- **布局的拼接**：一个布局可以看作其子布局的拼接（concatenation），即 `B = (B_0, B_1, ...)`。例如，一个多模式布局可以分解为多个单模式布局的并列。
- **左分配律**：当 `B` 是单射（injective）时，组合对拼接满足左分配律：  
    `A ∘ (B_0, B_1, ...) = (A ∘ B_0, A ∘ B_1, ...)`。  
    这意味着我们可以分别处理每个子布局，再将结果拼接起来。
基于此，我们可以**不失一般性地假设**：
- `B` 是一个单模式布局，即 `B = s : d`，其中 `s` 是形状（大小），`d` 是步长。
- `A` 是一个已经扁平化并经过 `coalesce` 简化的一维模式序列（即已经合并了可合并的相邻模式）。
这样，我们只需专注于计算 `A ∘ (s:d)`。

*1、当 `A`  为单模式时的简单情况*
如果 `A` 本身也是单模式，即 `A = a : b`，那么组合结果非常直接：
```c++
R = A ∘ B = (a:b) ∘ (s:d) = s : (b * d)
```
**解释**：`B` 将输入坐标 `x`（0 ≤ x < s）映射到 `x * d`，然后 `A` 将此值映射到 `(x * d) * b = x * (b * d)`。因此结果布局的形状为 `s`，步长为 `b * d`。

*2、当 `A` 为多模式时的复杂情况*
若 `A` 有多个模式，记为 `A = (s0, s1, ..., sn-1) : (d0, d1, ..., dn-1)`，我们需要构造 `A ∘ (s:d)`。这相当于先对 `A` 进行步长为 `d` 的**均匀采样**（取第 0, d, 2d, ... 个元素），然后从采样结果中取出前 `s` 个元素，使得最终形状与 `B` 的输入大小 `s` 匹配。
这个过程分为两步：
**2.1 步长为 `d` 的采样：`A/d`**
我们想要得到一个新的布局，它包含 `A` 中所有索引为 `d` 的倍数的元素。这个新布局的形状可以通过对 `A` 的形状**从左到右依次除以 `d`** 得到，记作 `A / d`。这里的除法是指：从最左边的维度开始，用 `d` 依次“消耗”每个维度的大小，直到 `d` 被完全分解。结果的新形状每个维度等于原维度除以某个因子，且这些因子的乘积恰好等于 `d`。
例如，对于形状 `(6,2)`（总大小 12）：
- `(6,2) / 2 = (3,2)` （因为 2 = 2×1，第一维 6 除以 2 得 3，第二维不变）
- `(6,2) / 3 = (2,2)` （3 = 3×1，第一维 6 除以 3 得 2）
- `(6,2) / 6 = (1,2)` （6 = 6×1，第一维 6 除以 6 得 1）
- `(6,2) / 12 = (1,1)` （12 等于总大小，所有维度变为 1）
对于更复杂的形状 `(3,6,2,8)`（总大小 288）：
- `(3,6,2,8) / 3 = (1,6,2,8)` （3 = 3×1×1×1）
- `(3,6,2,8) / 6 = (1,3,2,8)` （6 = 3×2，第一维变 1，第二维 6 除以 2 得 3）
- `(3,6,2,8) / 9 = (1,2,2,8)` （9 = 3×3，第一维变 1，第二维 6 除以 3 得 2）
- `(3,6,2,8) / 72 = (1,1,1,4)` （72 = 3×6×2×2，最后一维 8 除以 2 得 4）
这些结果正是采样后的新形状。
**新布局的步长**也需要相应调整。新步长等于原步长乘以一个因子，该因子取决于 `d` 在每个维度上的分配。具体地，对于上述除法过程，新步长的计算规则为：从左到右，每个维度的新步长等于原步长乘以**从该维度开始剩余尚未除尽的除数**。例如，对于 `(3,6,2,8) / 72`，原步长为 `(w,x,y,z)`，得到的新步长为 `(72w, 24x, 4y, 2z)`，其中倍数依次为 72, 72/3=24, 24/6=4, 4/2=2。这个规律保证了新布局中每个坐标变化对应的偏移增量正确。
**2.2 取前 `s` 个元素：(A/d) % s**
经过采样，我们得到一个中间布局，其大小为 `size(A)/d`。现在需要从中取出前 `s` 个元素（因为 `B` 的形状为 `s`），这相当于对中间布局的形状进行**截取**，记作 `(A/d) % s`。截取操作也是从左到右进行：从最左边维度开始，依次取尽可能多的元素，直到达到总数 `s`，剩余维度变为 1，最后一个维度可能只取一部分。
例如，对 `(6,2)` 取前 2 个元素：
- `(6,2) % 2 = (2,1)` （前 2 个对应第一维 0,1，第二维 0）
- `(6,2) % 3 = (3,1)`
- `(6,2) % 6 = (6,1)`
- `(6,2) % 12 = (6,2)`（取全部）
对于 `(3,6,2,8)` 取前 6 个：
- `(3,6,2,8) % 6 = (3,2,1,1)` （前 6 个覆盖第一维全部 3 个和第二维前 2 个）
- `% 9 = (3,3,1,1)` （第一维 3，第二维 3）
- 对于中间布局 `(1,2,2,8)` 取前 16 个：
    - `(1,2,2,8) % 16 = (1,2,2,4)` （前 16 个：第一维 1，第二维 2，第三维 2，第四维 4）
截取后的新布局**步长保持不变**，因为只是缩小了每个维度的范围，并没有改变维度间的步进关系。
可以将 `Composition` 运算看成 Layout B 是从 Layout A 中挑选出特定的坐标产生的
### Complement 运算
在分块操作中，我们经常需要从一个大的张量中选取一个子块（tile），同时还需要知道剩余的部分（即“补集”）如何组织。例如，我们有一个 tile 的布局 `A`，希望将它重复多次以填满一个更大的形状。`complement` 的作用就是生成这个重复模式，使得原布局 `A` 和补集布局 `R` 一起构成一个完整的大布局，且 `A` 和 `R` 的像（偏移集合）除原点外互不相交。
### Division 运算
`logical_divide(A, B)` 将布局 `A` 拆分成两个 modes：第一个 mode 中所有元素都指向 `B` 而第二个 mode 中所有元素都不指向 `B`.
形式化公式如下：
$A \oslash B := A \circ (B,B^*)$
具体实现：
```c++
template <class LShape, class LStride,
          class TShape, class TStride>
auto logical_divide(Layout<LShape,LStride> const& layout,
                    Layout<TShape,TStride> const& tiler)
{
  return composition(layout, make_layout(tiler, complement(tiler, size(layout))));
}
```

可以注意到，上式中只与 composition、complement 和 concatenation 三种运算相关。

### Product 运算

`logical_product(A, B)` 会产生两个 mode 的 layout 布局，第一个就是 `A` 的 Layout，第二个布局是 B 的布局，但是每个元素是布局 `A` 的唯一拷贝。
形式化的公式如下：
$A \otimes B:=(A, A^*\circ B)$
具体实现：
```c++
template <class LShape, class LStride,
          class TShape, class TStride>
auto logical_product(Layout<LShape,LStride> const& layout,
                     Layout<TShape,TStride> const& tiler)
{
  return make_layout(layout, composition(complement(layout, size(layout)*cosize(tiler)), tiler));
}
```
