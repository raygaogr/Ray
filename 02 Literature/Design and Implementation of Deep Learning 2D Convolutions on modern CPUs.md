---
tags:
  - HPC
  - CPU
  - Convolition
aliases:
  - Conv2d on cpus
date: 2023-12-21T15:14:00
---
	
---

- **Naive Conv2d with ReLU**

```php
// Naive Conv2D with ReLU
 for b=0, B, 1 do  // batch
   for m=0, M, 1 do  // output channels
     for y=0, Y, 1 do  // output height
       for x=0, X, 1 do  // output width
         float acc = 0;
         for k.y=0, K.Y, 1 do  // kernel height
           for k.x=0, K.X, 1 do  // kernel width
             for d=0, D, 1 do  // input channels
               acc += in[b][y * stride.y + k.y][x * stride.x + k.x][d] * filt[m][k.y][k.x][d];
             endfor
           endfor
         endfor
         acc += bias_array[m];
         out[b][y][x][m] = ReLU(acc);
       endfor
     endfor
   endfor
 endfor
```



- **Conv2D with vectorization** ^358595

```php
__m256 acc, inp, mask, bias
 for b=0, B, 1 do
   for m=0, M, m0 do // vectorized loop
     for y=0, Y, 1 do
       for x=0, X, 1 do
         acc = setzero();
         for k.y=0, K.Y, 1 do
           for k.x=0, K.X, 1 do
             for d, D, d0 do // vectorized loop
               inp = broadcast(in[b][y*stride.y+k.y][x*stride.x+k.x][d:d+d0]);
               mask = load(filt[m:m+m0][k.y][k.x][d:d+d0]);
               acc = fmadd(inp, mask, acc);
             endfor
           endfor
         endfor
         acc = hadd(acc);  // d0-level horizontal add
         acc = put.consecutive(acc)  // put the results of the horizontal additions into consecutive position
         bias = load(bias_array[m:m+m0]);
         acc = add(acc, bias);
         acc = ReLU(acc);
         store(out[b][y][x][m:m+m0], acc);
       endfor
     endfor
   endfor
 endfor
```



- **Conv2D with register blocking**(Rm=2, Rx=2)

```php
__m256 acc0,acc1,acc2,acc3,mask0,mask1,inp,bias0,bias1;
 for b=0,B,1 do
   for m=0,M,Tm do
     for y=0,Y,1 do
       for x=0,X,Rx do
         for m2=m,m+Tm,m0*Rm do
           acc0 = setzero();
           acc1 = acc0; acc2 = acc0; acc3 = acc0;
           for k.y=0,K.Y,1 do
             for k.x=0,K.X,1 do
               for d=0,D,d0 do
                 inp = broadcast(in[b][y*stride.y+k.y][x*stride.x+k.x][d:d+d0]);
                 mask0 = load(filt[m2:m2+m0][k.y][k.x][d:d+d0]);
                 mask1 = load(filt[m2+m0:m2+2*m0][k.y][k.x][d:d+d0]);
                 acc0 = fmadd(inp, mask0, acc0);
                 acc1 = fmadd(inp, mask1, acc1);
                 inp = broadcast(in[b][y*stride.y+k.y][(x+1).stride.x+k.x][d:d+d0]);
                 acc2 = fmadd(inp, mask0, acc2);
                 acc3 = fmadd(inp, mask1, acc3);
               endfor
             endfor
           endfor
           bias0 = load(bias_array[m2:m2+m0]);
           bias1 = load(bias_array[m2+m0:m2+2*m0]);
           acc0 = add(acc0, bias0);
           acc1 = add(acc1, bias1);
           acc2 = add(acc2, bias0);
           acc3 = add(acc3, bias1);
           acc0 = ReLU(acc0);
           acc1 = ReLU(acc1);
           acc2 = ReLU(acc2);
           acc3 = ReLU(acc3);
           store(out[b][y][x][m2:m2+m0], acc0);
           store(out[b][y][x][m2+m0:m2+2*m0], acc1);
           store(out[b][y][x+1][m2:m2+m0], acc2);
           store(out[b][y][x+1][m2+m0:m2+2*m0], acc3);
         endfor
       endfor
     endfor
   endfor
```

### Number of execited L/S instructions for each array

- OPS 定义（最内核循环的次数）

  $$
  OPS = B \times M/m0 \times Y \times X \times K.Y \times K.X \times D/d0
  $$

- in 数组调用`load`指令的次数
  $$
  In.Loads = \frac{OPS}{Rb\times Ry \times Rx \times Rm \times Rd} \times Rb \times Ry \times Rx \times Rd
  $$
- filter 数组调用`load` 指令的次数

  $$
  Filt.Loads = \frac{OPS}{Rb\times Ry \times Rx \times Rm \times Rd} \times Rm \times Rd
  $$

- out 数组调用`store`指令的次数
  $$
  Out.Stores =\frac{(\frac{OPS}{K.Y \times K.X \times D/d0})}{Rb\times Ry \times Rx \times Rm} \times Rb \times Ry \times Rx \times Rm^{'}
  $$

其中，

$$
Rm^{'} = \lceil Rm/d0 \rceil + Z, \text{where } Z=0 \text{ when} \\ ((Rm \times m0)\%(m0 \times d0)) \text{ is a power of 2 or zero, and Z=1 otherwise.}
$$

$m0 \times d0$ 个元素需要进行向量化，而 $m0 \times Rm$ 个元素会存储在连续的内存空间中。

$$
L/S.overall = OPS / Rm + OPS/(Rb\times Ry\times Rx) + (OPS/(D/d0\times K.Y\times K.X)\times (\lceil Rm/d0 \rceil +Z)/Rm)
$$

在最内层循环中使用的寄存器数量应该不能超过硬件的可用寄存器总数，约束条件如下：

$$
0.85 \times Regs < Rb\times Ry\times Rx\times Rm + Rb\times Ry\times Rx + Rm+ext < Regs
$$

$$
0.85 \times Regs < Rx\times Rm + Rx + Rm+ext < Regs
$$

$$
0.85 \times Regs < Rx\times Rm + \min(Rx , Rm)+1+ext < Regs
$$

### Data Reuse

当 kernel_size $\ne$ 1 时，对于 in 数组的加载指令可以通过利用数据复用大幅度减少。例如：3 x 3 的卷积核平移一个位置时，6/9 的 in 数组会被复用。
Hello 你好
