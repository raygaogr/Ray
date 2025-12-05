---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'



# test

```C++
template <
typename ElementA_,
typename LayoutA_,
typename ElementB_,
typename LayoutB_,
typename ElementC_,
typename LayoutC_,
typename ElementAccumulator_ = ElementC_,
typename OperatorClass_ = arch::OpClassSimt,
typename ArchTag_ = arch::Sm70,

typename ThreadblockShape_ = typename DefaultGemmConfiguration<
OperatorClass_, ArchTag_, ElementA_, ElementB_, ElementC_,
ElementAccumulator_>::ThreadblockShape,

typename WarpShape_ = typename DefaultGemmConfiguration<
OperatorClass_, ArchTag_, ElementA_, ElementB_, ElementC_,
ElementAccumulator_>::WarpShape,

typename InstructionShape_ = typename DefaultGemmConfiguration<
OperatorClass_, ArchTag_, ElementA_, ElementB_, ElementC_,
ElementAccumulator_>::InstructionShape,

typename EpilogueOutputOp_ = typename DefaultGemmConfiguration<
OperatorClass_, ArchTag_, ElementA_, ElementB_, ElementC_,
ElementAccumulator_>::EpilogueOutputOp,

typename ThreadblockSwizzle_ = typename threadblock::GemmIdentityThreadblockSwizzle<>,

int Stages = DefaultGemmConfiguration<OperatorClass_, ArchTag_, ElementA_, ElementB_,
ElementC_, ElementAccumulator_>::kStages,

int AlignmentA = DefaultGemmConfiguration<OperatorClass_, ArchTag_, ElementA_, ElementB_,
ElementC_, ElementAccumulator_>::kAlignmentA,

int AlignmentB = DefaultGemmConfiguration<OperatorClass_, ArchTag_, ElementA_, ElementB_,
ElementC_, ElementAccumulator_>::kAlignmentB,

bool SplitKSerial = false,

typename Operator_ = typename DefaultGemmConfiguration<
OperatorClass_, ArchTag_, ElementA_, ElementB_, ElementC_,
ElementAccumulator_>::Operator,

bool GatherA = false,

bool GatherB = false,

bool ScatterD = false,

typename PermuteDLayout = layout::NoPermute>
```


# device::Gemm

```c++
template <
    /// Element type for A matrix operand
    typename ElementA_,
    /// Layout type for A matrix operand
    typename LayoutA_,
    /// Element type for B matrix operand
    typename ElementB_,
    /// Layout type for B matrix operand
    typename LayoutB_,
    /// Element type for C and D matrix operands
    typename ElementC_,
    /// Layout type for C and D matrix operands
    typename LayoutC_,
    /// Element type for internal accumulation
    typename ElementAccumulator_ = ElementC_,
    /// Operator class tag
    typename OperatorClass_ = arch::OpClassSimt,
    /// Tag indicating architecture to tune for
    typename ArchTag_ = arch::Sm70,
    /// Threadblock-level tile size (concept: GemmShape)
    typename ThreadblockShape_ = typename DefaultGemmConfiguration<
        OperatorClass_, ArchTag_, ElementA_, ElementB_, ElementC_,
        ElementAccumulator_>::ThreadblockShape,
    /// Warp-level tile size (concept: GemmShape)
    typename WarpShape_ = typename DefaultGemmConfiguration<
        OperatorClass_, ArchTag_, ElementA_, ElementB_, ElementC_,
        ElementAccumulator_>::WarpShape,
    /// Instruction-level tile size (concept: GemmShape)
    typename InstructionShape_ = typename DefaultGemmConfiguration<
        OperatorClass_, ArchTag_, ElementA_, ElementB_, ElementC_,
        ElementAccumulator_>::InstructionShape,
    /// Epilogue output operator
    typename EpilogueOutputOp_ = typename DefaultGemmConfiguration<
        OperatorClass_, ArchTag_, ElementA_, ElementB_, ElementC_,
        ElementAccumulator_>::EpilogueOutputOp,
    /// Threadblock-level swizzling operator
    typename ThreadblockSwizzle_ =
        typename threadblock::GemmIdentityThreadblockSwizzle<>,
    /// Number of stages used in the pipelined mainloop
    int Stages =
        DefaultGemmConfiguration<OperatorClass_, ArchTag_, ElementA_, ElementB_,
                                 ElementC_, ElementAccumulator_>::kStages,
    /// Access granularity of A matrix in units of elements
    int AlignmentA =
        DefaultGemmConfiguration<OperatorClass_, ArchTag_, ElementA_, ElementB_,
                                 ElementC_, ElementAccumulator_>::kAlignmentA,
    /// Access granularity of B matrix in units of elements
    int AlignmentB =
        DefaultGemmConfiguration<OperatorClass_, ArchTag_, ElementA_, ElementB_,
                                 ElementC_, ElementAccumulator_>::kAlignmentB,
    /// If true, kernel supports split-K with serial reduction
    bool SplitKSerial = false,
    /// Operation performed by GEMM
    typename Operator_ = typename DefaultGemmConfiguration<
        OperatorClass_, ArchTag_, ElementA_, ElementB_, ElementC_,
        ElementAccumulator_>::Operator,
    /// Gather operand A by using an index array
    bool GatherA = false,
    /// Gather operand B by using an index array
    bool GatherB = false,
    /// Scatter result D by using an index array
    bool ScatterD = false,
    /// Permute result D
    typename PermuteDLayout = layout::NoPermute>
class Gemm
```

# Excalidraw Data

## Text Elements
DefaultGemm ^gvxbqwR7

    typename ElementA_,
    typename LayoutA_,
    int kAlignmentA,
    typename ElementB_,
    typename LayoutB_,
    int kAlignmentB,
    typename ElementC_,
    typename LayoutC_,
    typename ElementAccumulator,
    typename OperatorClass,
    typename ArchTag,
    typename ThreadblockShape,
    typename WarpShape,
    typename InstructionShape,
    typename EpilogueOutputOp,
    typename ThreadblockSwizzle,
    int Stages,
    bool SplitKSerial,
    typename Operator,
    SharedMemoryClearOption SharedMemoryClear = SharedMemoryClearOption::kNone,
    bool GatherA = false,
    bool GatherB = false,
    bool ScatterD = false,
    typename PermuteDLayout = layout::NoPermute,
    typename PermuteALayout = layout::NoPermute,
    typename PermuteBLayout = layout::NoPermute,
    typename Enable = void ^DmrXiSmf

成员： ^3pwqf58X

模板参数： ^xLgBvAIV

Gemm ^oksOSpHP

模板参数 ^cNFlzgWK

成员 ^tZGQhYWJ

using TensorRefA = TensorRef<ElementA const, LayoutA>;
using TensorRefB = TensorRef<ElementB const, LayoutB>;
using TensorRefC = TensorRef<ElementC const, LayoutC>;
using TensorRefD = TensorRef<ElementC, LayoutC>; ^52Q7UcbF

DefaultGemmConfiguration ^Dc1nwiZe

模板参数 ^lZHmaFnB

  typename OperatorClass,
  typename ArchTag,
  typename ElementA, 
  typename ElementB, 
  typename ElementC,
  typename ElementAccumulator ^vuQmMRAB

成员 ^ex4hWiZd

static int const kAlignmentA = 1; ^cXHCGjFA

static int const kAlignmentB = 1; ^ZTo29Gl8

using ThreadblockShape = GemmShape<128, 128, 8>; ^DHSzmtrt

using WarpShape = GemmShape<32, 64, 8>; ^lugG9eq1

using InstructionShape = GemmShape<1, 1, 1>; ^WZmefpli

static int const kStages = 2; ^oPfpfZnl

using EpilogueOutputOp = epilogue::thread::[[Attachment/Canvas/CUTLASS.excalidraw.md#^6O0vhfGn|LinearCombination]]<
        ElementC,
        1,
        ElementAccumulator,
        ElementAccumulator>; ^AOzBGZ9u

using Operator = arch::OpMultiplyAdd; ^zuJj1taW

typename ElementA_,
typename LayoutA_,
typename ElementB_,
typename LayoutB_,
typename ElementC_,
typename LayoutC_,
typename ElementAccumulator_ = ElementC_,
typename OperatorClass_ = arch::OpClassSimt,
typename ArchTag_ = arch::Sm70, ^qpunXEMc

typename [[Attachment/Canvas/CUTLASS.excalidraw.md#^lugG9eq1|WarpShape]]_ = typename DefaultGemmConfiguration<
        OperatorClass_, ArchTag_, ElementA_, ElementB_, ElementC_,
        ElementAccumulator_>::WarpShape, ^F3K7kwn7

typename [[Attachment/Canvas/CUTLASS.excalidraw.md#^WZmefpli|InstructionShape]]_ = typename DefaultGemmConfiguration<
        OperatorClass_, ArchTag_, ElementA_, ElementB_, ElementC_,
        ElementAccumulator_>::InstructionShape, ^xlTWyMS4

typename [[Attachment/Canvas/CUTLASS.excalidraw.md#^AOzBGZ9u|EpilogueOutputOp]]_ = typename DefaultGemmConfiguration<
        OperatorClass_, ArchTag_, ElementA_, ElementB_, ElementC_,
        ElementAccumulator_>::EpilogueOutputOp, ^e83iWmaS

typename [[Attachment/Canvas/CUTLASS.excalidraw.md#^DHSzmtrt|ThreadblockShape]]_ = typename DefaultGemmConfiguration<
        OperatorClass_, ArchTag_, ElementA_, ElementB_, ElementC_,
        ElementAccumulator_>::ThreadblockShape, ^tMNbLXn0

typename [[Attachment/Canvas/CUTLASS.excalidraw.md#^tK_CqjLImBZOerPaQxt8M|GemmIdentityThreadblockSwizzle]]_ =
        typename threadblock::GemmIdentityThreadblockSwizzle<> ^hPZWFlQM

DefaultMma ^kSFQyUJm

模板参数 ^oaamP1ar

成员 ^rpHRGARv

GemmIdentityThreadblockSwizzle ^qUnCjuzM

模板参数 ^kc34l0i0

  int N = 1 ^bRqog8nu

成员 ^CUorL0aA

  static GemmCoord get_tiled_shape(
    GemmCoord problem_size,
    GemmCoord tile_size,
    int split_k_slices) {

    return GemmCoord(
      (problem_size.m() + tile_size.m() - 1) / tile_size.m(),
      (problem_size.n() + tile_size.n() - 1) / tile_size.n(),
      split_k_slices);
  } ^qU30Z6nF

int [[Attachment/Canvas/CUTLASS.excalidraw.md#^oPfpfZnl|kStages]] = DefaultGemmConfiguration<OperatorClass_, ArchTag_, 
         ElementA_, ElementB_, ElementC_, ElementAccumulator_>::kStages, ^6QVwThn5

int [[Attachment/Canvas/CUTLASS.excalidraw.md#^cXHCGjFA|kAlignmentA]] = DefaultGemmConfiguration<OperatorClass_, ArchTag_, 
         ElementA_, ElementB_, ElementC_, ElementAccumulator_>::kAlignmentA, ^f9sSxNPJ

int [[Attachment/Canvas/CUTLASS.excalidraw.md#^ZTo29Gl8|AlignmentB]] = DefaultGemmConfiguration<OperatorClass_, ArchTag_, 
         ElementA_, ElementB_, ElementC_, ElementAccumulator_>::kAlignmentB, ^HiZnkjnv

bool SplitKSerial = false, ^GPaNB2RQ

typename [[Attachment/Canvas/CUTLASS.excalidraw.md#^zuJj1taW|Operator]]_ = typename DefaultGemmConfiguration<
        OperatorClass_, ArchTag_, ElementA_, ElementB_, ElementC_,
        ElementAccumulator_>::Operator, ^uNo4Arj6

bool GatherA = false,
bool GatherB = false,
bool ScatterD = false,
typename PermuteDLayout = layout::NoPermute ^Rn83ySP3

static int const kStages = Stages;
static int const kAlignmentA = AlignmentA;
static int const kAlignmentB = AlignmentB;
static int const kAlignmentC = EpilogueOutputOp::kCount;
static bool const kSplitKSerial = SplitKSerial;
static ComplexTransform const kTransformA = ComplexTransform::kNone;
static ComplexTransform const kTransformB = ComplexTransform::kNone; ^aNWWOIJN

using [[Attachment/Canvas/CUTLASS.excalidraw.md#^cCxqMGS4|GemmKernel]] = typename kernel::DefaultGemm<
        ElementA,                 // float
        LayoutA,                  // RowMajor
        kAlignmentA,              // 1
        ElementB,                 // float
        LayoutB,                  // RowMajor
        kAlignmentB,               // 1
        ElementC,                   // float
        LayoutC,                    // RowMajor    
        ElementAccumulator,      // float
        OperatorClass,             // OpClassSimt
        ArchTag,                    // Sm70
        ThreadblockShape,        // <128, 128, 8>
        WarpShape,                 // <32, 64, 8>
        InstructionShape,         // <1, 1, 1,>
        EpilogueOutputOp,        // LinearCombination
        ThreadblockSwizzle,      // GemmIdentityThreadSwizzle
        kStages,                    // 2
        kSplitKSerial,               // false
        Operator,                    // OpMultiplyAdd
        SharedMemoryClearOption::kNone,    // 
        GatherA,                     // false
        GatherB,                     // false
        ScatterD,                    // false
        PermuteDLayout            // NoPermute
   >::GemmKernel; ^cHge6Wg4

using [[Attachment/Canvas/CUTLASS.excalidraw.md#^Yv2mWISj|Mma]] = typename cutlass::gemm::threadblock::DefaultMma<
      ElementA,   // float
      LayoutA,    // RowMajor
      kAlignmentA,  // 1
      ElementB,     // float
      LayoutB,      // RowMajor
      kAlignmentB,   //  1
      ElementAccumulator,   // float
      LayoutC,                // RowMajor
      arch::OpClassSimt,        
      arch::Sm50,
      ThreadblockShape,    // <128, 128, 8>
      WarpShape,             // <32, 64, 8>
      GemmShape<1, 1, 1>,  
      2,
      Operator,                // OpMultiplyAdd
      false,
      SharedMemoryClear,    // kNone
      GatherA,
      GatherB,
      PermuteALayout,
      PermuteBLayout>::ThreadblockMma; ^eOmKusgo

static int const [[Attachment/Canvas/CUTLASS.excalidraw.md#^I0MofIIM|kEpilogueElementsPerAccess]]; = EpilogueOutputOp::kCount; ^VvLeLr3p

using RegularEpilogue = typename cutlass::epilogue::threadblock::[[Attachment/Canvas/CUTLASS.excalidraw.md#^GWNxRu9jlglVP27zOgDIR|DefaultEpilogueSimt]]<
      ThreadblockShape,
      typename Mma::[[Attachment/Canvas/CUTLASS.excalidraw.md#^Y3T1VFk0|Operator]],
      EpilogueOutputOp,
      kEpilogueElementsPerAccess,
      ScatterD,
      PermuteDLayout
      >::Epilogue; ^MxYkrQG7

using Affine2Epilogue = typename 
      cutlass::epilogue::threadblock::DefaultEpilogueSimtAffineRankN<
      2,
      ThreadblockShape,
      typename Mma::[[Attachment/Canvas/CUTLASS.excalidraw.md#^Y3T1VFk0|Operator]],
      EpilogueOutputOp,
      kEpilogueElementsPerAccess
      >::Epilogue; ^dkB1S6MT

using Epilogue = typename platform::conditional<platform::is_same<LayoutC, 
       layout::RowMajor>::value,  RegularEpilogue, Affine2Epilogue>::type; ^2Tu22l62

using [[Attachment/Canvas/CUTLASS.excalidraw.md#^MLhw9Tg5|GemmKernel]] = kernel::Gemm<Mma, Epilogue, ThreadblockSwizzle, SplitKSerial>; ^cCxqMGS4

LinearCombination ^6O0vhfGn

    typename ElementOutput_,                            
    int Count,              
    typename ElementAccumulator_ = ElementOutput_,       
    typename ElementCompute_ = ElementOutput_,           
    ScaleType::Kind Scale = ScaleType::Default,         
    FloatRoundStyle Round = FloatRoundStyle::round_to_nearest,
    typename ElementSource_ = ElementOutput_ ^VWSJWe3b

成员： ^jf8Og7Md

模板参数： ^FvqUw95O

static int const kCount = Count; ^I0MofIIM

    typename ElementA_,                // float
    typename LayoutA_,                 // RowMajor
    int kAlignmentA,                  // 1
    typename ElementB_,           // float
    typename LayoutB_,            
    int kAlignmentB,
    typename ElementAccumulator_,
    typename LayoutC_,
    typename OperatorClass_,
    typename ArchTag_,
    typename ThreadblockShape_,
    typename WarpShape_,
    typename InstructionShape_,
    int Stages,
    typename Operator,
    bool AccumulatorsInRowMajor = false,
    SharedMemoryClearOption SharedMemoryClear = SharedMemoryClearOption::kNone,
    bool GatherA = false,
    bool GatherB = false,
    typename PermuteALayout = layout::NoPermute,
    typename PermuteBLayout = layout::NoPermute ^8qbR0NKV

using [[Attachment/Canvas/CUTLASS.excalidraw.md#^rXcyRXBW|MmaCore]] = typename cutlass::gemm::threadblock::DefaultMmaCore<
      ThreadblockShape, WarpShape, InstructionShape, ElementA, LayoutA,
      ElementB, LayoutB, ElementAccumulator, LayoutC,
      arch::OpClassSimt, 2, Operator>; ^xWYklhEm

using [[Attachment/Canvas/CUTLASS.excalidraw.md#^55QtlXqv|IteratorA]] = cutlass::transform::threadblock::PredicatedTileIterator<
      cutlass::MatrixShape<MmaCore::Shape::kM, MmaCore::Shape::kK>,
      ElementA, LayoutA, 1, typename MmaCore::[[Attachment/Canvas/CUTLASS.excalidraw.md#^Vt3xjzqg|SmemThreadMapA]], kAlignmentA,
      GatherA, PermuteALayout>; ^v6pm2k8M

using [[Attachment/Canvas/CUTLASS.excalidraw.md#^55QtlXqv|IteratorB]] = cutlass::transform::threadblock::PredicatedTileIterator<
      cutlass::MatrixShape<MmaCore::Shape::kK, MmaCore::Shape::kN>,
      ElementB, LayoutB, 0, typename MmaCore::[[Attachment/Canvas/CUTLASS.excalidraw.md#^NR3xyHQV|IteratorThreadMapB]], kAlignmentB,
      GatherB, PermuteBLayout>; ^vwSwb8AV

using [[Attachment/Canvas/CUTLASS.excalidraw.md#^WZGr9W59|ThreadblockMma]] = cutlass::gemm::threadblock::MmaPipelined<
      typename MmaCore::Shape, IteratorA, typename MmaCore::[[Attachment/Canvas/CUTLASS.excalidraw.md#^Vt3xjzqg|SmemIteratorA]],
      IteratorB, typename MmaCore::[[Attachment/Canvas/CUTLASS.excalidraw.md#^dtuuzCb7|SmemIteratorB]], ElementAccumulator,
      LayoutC, typename MmaCore::[[Attachment/Canvas/CUTLASS.excalidraw.md#^8EuN4PbU|MmaPolicy]]>; ^Yv2mWISj

DefaultMmaCore ^rXcyRXBW

模板参数 ^zhCQSpta

成员 ^88agWQfu

static int const PartitionsK = Shape::kK / WarpShape::kK; // 1 ^uT6M5AJZ

using WarpCount = GemmShape<
    Shape::kM / WarpShape::kM,
    Shape::kN / WarpShape::kN,
    PartitionsK
>;  // <4, 2, 1> ^1RLNUFN7

static int const kWarpSize = warp::WarpSize<arch::OpClassSimt>::value; ^PUtQdBAG

static int const kThreads = WarpCount::kCount * kWarpSize; ^rcUTgrHL

static int const kElementsPerAccess = 1; ^Im55vjK1

using SmemLayoutA = layout::ColumnMajor;
using SmemLayoutB = layout::RowMajor; ^aQ2KGYLf

using IteratorThreadMapA = transform::[[Attachment/Canvas/CUTLASS.excalidraw.md#^PjeIdGpB|PitchLinearStripminedThreadMap]]<
        layout::PitchLinearShape<Shape::kK, Shape::kM>,
        kThreads,
        kElementsPerAccess>; ^6XwNDBdW

using SmemThreadMapA = 
      transform::[[Attachment/Canvas/CUTLASS.excalidraw.md#^Qeeklpwa|TransposePitchLinearThreadMapSimt]]<IteratorThreadMapA>; ^Vt3xjzqg

using SmemIteratorA = transform::threadblock::[[Attachment/Canvas/CUTLASS.excalidraw.md#^X_UvMe8tYN9TY1L2iAE8_|RegularTileIterator]]<
        MatrixShape<Shape::kM, Shape::kK>, 
        ElementA, 
        SmemLayoutA,
        1,
        SmemThreadMapA>;
 ^ua0KoCbK

using IteratorThreadMapB = transform::[[Attachment/Canvas/CUTLASS.excalidraw.md#^PjeIdGpB|PitchLinearStripminedThreadMap]]<
        layout::PitchLinearShape<Shape::kN, Shape::kK>,
        kThreads,
        kElementsPerAccess>; ^NR3xyHQV

using SmemIteratorB = transform::threadblock::[[Attachment/Canvas/CUTLASS.excalidraw.md#^X_UvMe8tYN9TY1L2iAE8_|RegularTileIterator]]<
        MatrixShape<Shape::kK, Shape::kN>, 
        ElementB, 
        SmemLayoutB,
        0,
        IteratorThreadMapB>; ^dtuuzCb7

static const int WarpNumThreadsM = 
    detail::simt_get_warp_threads_m<WarpShape>();
static const int WarpNumThreadsN = kWarpSize / WarpNumThreadsM;
static const int ThreadTileM = WarpShape::kM / WarpNumThreadsM;
static const int ThreadTileN = WarpShape::kN / WarpNumThreadsN; ^so4JyepW

static const int LaneLayout = ThreadTileM > 4 && ThreadTileN > 4 ? 2 : 1;
static const int numElementsA = 128 / sizeof_bits<ElementA>::value;
static const int numElementsB = 128 / sizeof_bits<ElementB>::value;
static const int LaneM = cutlass::const_min(numElementsA, ThreadTileM);
static const int LaneN = cutlass::const_min(numElementsB, ThreadTileN); ^sDxUtxRD

static int const kPaddingM = detail::simt_transpose_padding(kWarpSize, 
    Shape::kK, sizeof_bits<ElementA>::value); ^ZbDpW4K6

using LaneMmaShape = cutlass::gemm::GemmShape<
      LaneM,
      LaneN,
1>; ^3ujhKBOw

using [[Attachment/Canvas/CUTLASS.excalidraw.md#^2w5AvH7x|Policy]] = cutlass::gemm::warp::MmaSimtPolicy<
      cutlass::MatrixShape<WarpNumThreadsM, WarpNumThreadsN>,   
      cutlass::layout::[[Attachment/Canvas/CUTLASS.excalidraw.md#^9QEMVvPRaIAspyuazPGZo|RowMajorInterleaved]]<LaneLayout>,         
      LaneMmaShape
>; ^fayFJ7jc

using [[Attachment/Canvas/CUTLASS.excalidraw.md#^xQxBX00n|MmaWarpSimt]] = cutlass::gemm::warp::MmaSimt<
      WarpShape,    
      ElementA,     
      SmemLayoutA,  
      ElementB,     
      SmemLayoutB,  
      ElementC,     
      LayoutC,      
      Policy        
  >; ^3sqAmniw

using [[Attachment/Canvas/CUTLASS.excalidraw.md#^QL8ScnW3|MmaPolicy]] = MmaPolicy<
    MmaWarpSimt,
    MatrixShape<kPaddingM, 0>,   
    MatrixShape<0, 0>,
    WarpCount::kK
  >; ^8EuN4PbU

PredicatedTileIterator ^55QtlXqv

    typename Shape,
    typename Element,
    typename Layout,
    int AdvanceRank,
    typename ThreadMap,
    int AccessSize = ThreadMap::kElementsPerAccess,
    bool Gather = false,
    typename PermuteLayout = layout::NoPermute ^tubPcGsZ

成员： ^AuZa1aiQ

模板参数： ^VERbGDD4

static int const kAdvanceRank = AdvanceRank; ^5bP6eFJq

using WarpGemm = typename Policy::Operator::Shape ^MY5hoMSR

using WarpCount = GemmShape<Shape::kM / WarpGemm::kM,
                              Shape::kN / WarpGemm::kN,
                              Shape::kK / WarpGemm::kK>; ^rDjDol5x

static int const kWarpGemmIterations =
      (WarpGemm::kK / Operator::Policy::MmaShape::kK); ^MzOZtDy5

static int const kStages = Stages; ^cyS6bW3Q

MmaBase ^f6cnvpDs

模板参数 ^H9O250Dl

    typename Shape_,
    typename Policy_,
    int Stages,
    typename Enable = bool ^cY2IKoer

成员 ^WjhYG2lv

class SharedStorage: ^E0ie0GTG

using ShapeA = MatrixShape<Shape::kM + Policy::SmemPaddingA::kRow,
                           Shape::kK * kStages +
                               Policy::SmemPaddingA::kColumn>; ^LkH4M9Y6

using ShapeB = MatrixShape<Shape::kK * kStages + Policy::SmemPaddingB::kRow,
                    Shape::kN + Policy::SmemPaddingB::kColumn>; ^Ozn9L7PW

AlignedBuffer<typename Operator::ElementA, ShapeA::kCount> operand_A;
AlignedBuffer<typename Operator::ElementB, ShapeB::kCount> operand_B; ^U4bBm1jw

typename Operator::IteratorA warp_tile_iterator_A_;
typename Operator::IteratorB warp_tile_iterator_B_; ^dcq6LJrR

Gemm ^MLhw9Tg5

    typename Mma_,                  ///! Threadblock-scoped matrix multiply-accumulate 
    typename Epilogue_,             ///! Epilogue
    typename ThreadblockSwizzle_,   ///! Threadblock swizzling function
    bool SplitKSerial  ^sD0bv3VI

成员： ^hp8SwWrR

模板参数： ^x6XmhjiD

using [[Attachment/Canvas/CUTLASS.excalidraw.md#^rDjDol5x|WarpCount]] = typename Mma::WarpCount;
   static int const kThreadCount = 32 * WarpCount::kCount; ^2Yca17Pw

union SharedStorage {
        typename Mma::[[Attachment/Canvas/CUTLASS.excalidraw.md#^E0ie0GTG|SharedStorage]] main_loop;
        typename Epilogue::SharedStorage epilogue;
   }; ^0LCvbuE0

struct Params: ^RZFlekfR

cutlass::gemm::GemmCoord problem_size;
   cutlass::gemm::GemmCoord grid_tiled_shape;
   int swizzle_log_tile;
   typename Mma::[[Attachment/Canvas/CUTLASS.excalidraw.md#^TZvj5oBi|IteratorA]]::[[Attachment/Canvas/CUTLASS.excalidraw.md#^xEzyjmes|Params]] params_A;
   typename Mma::[[Attachment/Canvas/CUTLASS.excalidraw.md#^TZvj5oBi|IteratorA]]::[[Attachment/Canvas/CUTLASS.excalidraw.md#^vV4uyWOg|TensorRef]] ref_A;
   typename Mma::[[Attachment/Canvas/CUTLASS.excalidraw.md#^v04GDWjv|IteratorB]]::[[Attachment/Canvas/CUTLASS.excalidraw.md#^xEzyjmes|Params]] params_B;
   typename Mma::[[Attachment/Canvas/CUTLASS.excalidraw.md#^v04GDWjv|IteratorB]]::[[Attachment/Canvas/CUTLASS.excalidraw.md#^vV4uyWOg|TensorRef]] ref_B;
   typename Epilogue::OutputTileIterator::Params params_C;
   typename Epilogue::OutputTileIterator::TensorRef ref_C;
   typename Epilogue::OutputTileIterator::Params params_D;
   typename Epilogue::OutputTileIterator::TensorRef ref_D; 
   typename OutputOp::Params output_op;
   int *semaphore;
   int gemm_k_size;
    
   int const *gather_A_indices;
   int const *gather_B_indices;
   int const *scatter_D_indices; ^MzCvSv4p

typename GemmKernel::[[Attachment/Canvas/CUTLASS.excalidraw.md#^RZFlekfR|Params]] params_; ^FzT9ZmaV

struct Arguments ^t5kQpjzB

 GemmCoord problem_size;
    TensorRef<ElementA const, LayoutA> ref_A;
    TensorRef<ElementB const, LayoutB> ref_B;
    TensorRef<ElementC const, LayoutC> ref_C;
    TensorRef<ElementC, LayoutC> ref_D;
    typename EpilogueOutputOp::[[Attachment/Canvas/CUTLASS.excalidraw.md#^fDdkynrs|Params]] epilogue;
    int split_k_slices;
    
    int const *gather_A_indices;
    int const *gather_B_indices;
    int const *scatter_D_indices; ^q1O7W68u

DefaultEpilogueSimt ^sg4AFKY7

    typename Shape_,
    typename WarpMmaSimt_,
    typename OutputOp_,
    int ElementsPerAccess,
    bool ScatterD = false,
    typename PermuteDLayout = layout::NoPermute,
    conv::StrideSupport StrideSupport = conv::StrideSupport::kUnity,
    int Rank = 4 ^wehejmWa

成员： ^Jzy0mD1j

模板参数： ^ogJzTixM

    typename Operator = typename platform::conditional<
        (platform::is_same<OperatorClass,
                           cutlass::arch::OpClassTensorOp>::value) &&
            (platform::is_same<ElementA, int8_t>::value ||
             platform::is_same<ElementA, int4b_t>::value ||
             platform::is_same<ElementA, uint8_t>::value ||
             platform::is_same<ElementA, uint4b_t>::value),
        cutlass::arch::OpMultiplyAddSaturate,
        cutlass::arch::OpMultiplyAdd>::type, ^WlG1oURJ

    typename Shape,    // <128, 128, 8>
    typename WarpShape,   // <32, 64, 8>
    typename InstructionShape,    //<1, 1, 1>
    typename ElementA,        // float
    typename LayoutA,         // RowMajor
    typename ElementB,        // float
    typename LayoutB,         // RowMajor
    typename ElementC,         // float
    typename LayoutC,          // RowMajor
    typename OperatorClass,   // OpClassSimt
    int Stages = 2,                ^lJpkbM0M

    bool AccumulatorsInRowMajor = false,
    cutlass::arch::CacheOperation::Kind CacheOpA =
        cutlass::arch::CacheOperation::Global,
    cutlass::arch::CacheOperation::Kind CacheOpB =
        cutlass::arch::CacheOperation::Global,
    ComplexTransform TransformA = ComplexTransform::kNone,
    ComplexTransform TransformB = ComplexTransform::kNone,
    bool IsComplex = false ^uIH4hkCh

using Base = [[Attachment/Canvas/CUTLASS.excalidraw.md#^f6cnvpDs|MmaBase]]<Shape_, Policy_, 2>; ^NHTbmMHJ

using FragmentA = typename IteratorA::Fragment;
using FragmentB = typename IteratorB::Fragment;
using FragmentC = typename Policy::Operator::FragmentC; ^h44J32BJ

static ComplexTransform const kTransformA = Operator::kTransformA;
static ComplexTransform const kTransformB = Operator::kTransformB; ^vIId0Z54

[[Attachment/Canvas/CUTLASS.excalidraw.md#^WlG1oURJ|Operator]] warp_mma;

    SmemIteratorA smem_iterator_A_;
    SmemIteratorB smem_iterator_B_;
    TransformA transform_A_;
    TransformB transform_B_;
    int smem_write_stage_idx; ^Y3T1VFk0

MmaPipelined ^WZGr9W59

模板参数 ^tpDNbGjE

    typename Shape_,
    typename IteratorA_,
    typename SmemIteratorA_,
    typename IteratorB_,
    typename SmemIteratorB_,
    typename ElementC_,
    typename LayoutC_,
    typename Policy_,
    typename TransformA_ = NumericArrayConverter<
    typename SmemIteratorA_::Element, 
    typename IteratorA_::Element, 
    IteratorA_::Fragment::kElements>,
    typename TransformB_ = NumericArrayConverter<
    typename SmemIteratorB_::Element, 
    typename IteratorB_::Element, 
    IteratorB_::Fragment::kElements>,
    typename Enable = bool ^po0hx4Qu

成员 ^M7FLaq53

using Shape = Shape_ ^pJqjtqu6

using [[Attachment/Canvas/CUTLASS.excalidraw.md#^v6pm2k8M|IteratorA]] = IteratorA_;   ^TZvj5oBi

using [[Attachment/Canvas/CUTLASS.excalidraw.md#^vwSwb8AV|IteratorB]] = IteratorB_; ^v04GDWjv

using ElementC = ElementC_;  ^nvMYEfgD

using LayoutC = LayoutC_;   ^aBecSyFo

using Policy = Policy_;  ^FT2UTFSQ

using SmemIteratorA = SmemIteratorA_; ^sRPjhnOA

using SmemIteratorB = SmemIteratorB_; ^ERIe2CYW

using TransformA = TransformA_; ^Lm3Uviez

using TransformB = TransformB_; ^WLHj5I4D

using Shape = Shape_; ^8wuT8Kbt

using Element = Element_; ^2KWAdSbc

using Layout = layout::PitchLinear; ^Tw5OfA1O

using ThreadMap = ThreadMap_; ^DxHHudDN

using Index = typename Layout::Index; ^ISZY6z95

using LongIndex = typename Layout::LongIndex; ^0TlkDzqO

using TensorRef = TensorRef<Element, Layout>;
using TensorView = TensorView<Element, Layout>;
using TensorCoord = typename Layout::TensorCoord; ^vV4uyWOg

using Pointer = Element *;
using NonConstPointer = typename platform::remove_const<Element>::type *; ^mUfvxNiM

using AccessType = AlignedArray<Element, AccessSize, (AccessSize * sizeof_bits<Element>::value / 8)>; ^E0hzLn0U

using [[Attachment/Canvas/CUTLASS.excalidraw.md#^q36Texzx|TileAccessIterator]] =
      PredicatedTileAccessIterator<Shape, Element, Layout, kAdvanceRank,
                                   ThreadMap, AccessType, Gather, PermuteLayout>; ^L3ST0X9I

static int const kAccessesPerVector = TileAccessIterator::kAccessesPerVector; ^pgePHkB8

using Fragment = cutlass::Array<Element, ThreadMap::Iterations::kCount *
                                               ThreadMap::kElementsPerAccess>; ^rTVEKAaT

using Mask = typename TileAccessIterator::Mask; ^JLDNi5fH

class Params: ^xEzyjmes

using Base = typename TileAccessIterator::Params::Base; ^0TWWaW8h

friend PredicatedTileIterator; ^MgCltVbb

typename TileAccessIterator::Params params_; ^xMnLX1T4

using BytePointer = char *; ^SWpF1mVo

using Shape = Shape_; ^HmglHiCn

using WarpShape = WarpShape_; ^M2mSwwng

using InstructionShape = GemmShape<1, 1, 1>; ^SnM2GJbT

using ElementA = ElementA_; ^XTo9WV8s

using LayoutA = layout::RowMajor; ^e9VfBKyB

using ElementB = ElementB_; ^x8LSxgmp

using LayoutB = layout::RowMajor; ^anZweFKV

using ElementC = ElementC_; ^j09P9MHS

using LayoutC = LayoutC_; ^sGuT3y0w

using OperatorClass = arch::OpClassSimt; ^Ob65JXhr

using Operator = Operator_; ^4iGWLS4e

MmaPolicy ^QL8ScnW3

模板参数 ^tRnwZbta

    typename Operator_,
    typename SmemPaddingA_,
    typename SmemPaddingB_,
    int PartitionsK = 1 ^GlAPN72J

成员 ^5oLwzWb7

using Operator = Operator_; ^0GSSSEKr

using SmemPaddingA = SmemPaddingA_; ^FQk8xsU7

static int const kPartitionsK = PartitionsK; ^ZMzRa9Ew

using SmemPaddingB = SmemPaddingB_; ^hPBCjoUg

using Shape = Shape_; ^bnrYvu6O

using ElementA = ElementA_; ^m9MY1U8B

using LayoutA = layout::RowMajor; ^hvQ2z2oa

using ElementB = ElementB_; ^NQ3OshOm

using LayoutB = layout::RowMajor; ^YdqL7lFP

using ElementC = ElementC_; ^ZuVMejNz

using LayoutC = LayoutC_; ^sWTPtlEw

using OperatorClass = arch::OpClassSimt; ^VWDMagCV

using Policy = Policy_; ^cbwSQ6cu

MmaSimt ^xQxBX00n

模板参数 ^PkJpmfrC

成员 ^7NEOra7d

typename Shape_,
typename ElementA_,
typename LayoutA_,
typename ElementB_,
typename LayoutB_,
typename ElementC_,
typename LayoutC_,
typename Policy_,
int PartitionsK = 1,
ComplexTransform TransformA = ComplexTransform::kNone,
ComplexTransform TransformB = ComplexTransform::kNone,
typename Enable = bool ^sQbUKTYy

using OperatorClass = arch::OpClassSimt; ^4Cb50as6

using ArchTag = arch::Sm50; ^S3twoffy

static ComplexTransform const kTransformA = TransformA;
static ComplexTransform const kTransformB = TransformB; ^JuC77XMF

using ThreadLayoutA = typename platform::conditional< platform::is_same< 
        layout::ColumnMajorInterleaved<4>, LayoutA >::value,
        layout::ColumnMajor,
        typename platform::conditional < platform::is_same< 
        layout::RowMajorInterleaved<4>, LayoutA >::value,
        layout::RowMajor,
        LayoutA>::type
        >::type; ^6Sv4rT3f

using ThreadLayoutB = typename platform::conditional< platform::is_same< 
        layout::ColumnMajorInterleaved<4>, LayoutB >::value,
        layout::ColumnMajor,
        typename platform::conditional < platform::is_same< 
        layout::RowMajorInterleaved<4>, LayoutB >::value,
          layout::RowMajor,
          LayoutB>::type
        >::type; ^VYQZvKDY

static constexpr bool use_dp4a = (platform::is_same< 
        layout::ColumnMajorInterleaved<4>, LayoutA>::value || 
        platform::is_same< layout::RowMajorInterleaved<4>, 
        LayoutA >::value) && 
        platform::is_same< ElementA, int8_t >::value && 
        platform::is_same< ElementB, int8_t >::value; ^IrzEJL2w

using dp4a_type = typename platform::conditional< use_dp4a , int8_t, bool >::type; ^FcY0aeWC

using [[Attachment/Canvas/CUTLASS.excalidraw.md#^fa9A2GzY|ThreadMma]] = thread::Mma<
    GemmShape<
      Shape::kM / Policy::WarpShape::kRow,
      Shape::kN / Policy::WarpShape::kColumn,
      Policy::LaneMmaShape::kK>,
    ElementA,
    ThreadLayoutA,
    ElementB,
    ThreadLayoutB,
    ElementC,
    LayoutC,
    arch::OpMultiplyAdd,
    dp4a_type
  >; ^LcR5yo4n

using ArchMmaOperator = typename ThreadMma::ArchMmaOperator; ^asn7maOc

using MathOperator = typename ArchMmaOperator::Operator; ^nilghuQ8

using InstructionShape = GemmShape<1,1,use_dp4a ? 4 : 1>; ^sEjBl3bU

using IteratorA = MmaSimtTileIterator<
    MatrixShape<Shape::kM, Policy::LaneMmaShape::kK>,
    Operand::kA,
    ElementA,
    LayoutA,
    Policy,
    PartitionsK,
    Shape::kK
  >; ^6tZPlfGN

using FragmentA = typename IteratorA::Fragment; ^4PzPNpPJ

using TransformedFragmentA = FragmentA; ^iAvqWpqR

using IteratorB = MmaSimtTileIterator<
    MatrixShape<Policy::LaneMmaShape::kK, Shape::kN>,
    Operand::kB,
    ElementB,
    LayoutB,
    Policy,
    PartitionsK,
    Shape::kK
  >; ^cVE1RgXV

using FragmentB = typename IteratorB::Fragment; ^odYrnwo6

using TransformedFragmentB = FragmentB; ^gy6MV1G9

using IteratorC = MmaSimtTileIterator<
    MatrixShape<Shape::kM, Shape::kN>,
    Operand::kC,
    ElementC,
    LayoutC,
    Policy
  >; ^msMImhtW

using FragmentC = typename ThreadMma::FragmentC; ^Tg53eNlm

MmaSimtPolicy ^2w5AvH7x

模板参数 ^ZXryHUJ1

    typename WarpShape_,              
    typename LaneLayout_,             
    typename LaneMmaShape_  ^nzmjhexj

成员 ^7cFJmwWQ

using WarpShape = WarpShape_;
using LaneLayout = LaneLayout_;
using LaneMmaShape = LaneMmaShape_;
using MmaShape = LaneMmaShape; ^lrRCxqeq

    int Interleave ^T2YVnTuD

成员： ^R18y5szW

模板参数： ^2h03sMXx

static int const kRank = 2; ^LF4LaqiN

static int const kStrideRank = 1; ^Ag75puVU

using Index = int32_t; ^Ua6OHUKo

using LongIndex = int64_t; ^TXOSO4qg

using TensorCoord = MatrixCoord; ^e4XZoRRs

using Stride = Coord<kStrideRank, LongIndex>; ^lPl7kbpJ

static int const kInterleave = Interleave; ^75x34ttm

Stride stride_; ^K9EWMtoX

RowMajorInterleaved ^NTSVucUw

PitchLinearStripminedThreadMap ^PjeIdGpB

    typename Shape_,   <8, 128>
    int Threads,          256
    int ElementsPerAccess = 1 ^qNPW1tj1

成员： ^mIZ8LkNm

模板参数： ^UWaivDJa

using TensorCoord = layout::PitchLinearCoord; ^VXWBWJLN

using Shape = Shape_; ^yAKlni7a

static int const kThreads = Threads; ^L8E0vKZ6

static int const kElementsPerAccess = ElementsPerAccess; ^dv68F6T7

using ThreadAccessShape = layout::PitchLinearShape<kElementsPerAccess, 1>; ^G6OcQrAM

struct Detail： ^aP7Nu9wg

using ShapeVec = layout::PitchLinearShape<
      Shape::kContiguous / kElementsPerAccess,
      Shape::kStrided
    >;   // <8, 128> ^h3w7TT6L

using Iterations = typename platform::conditional<
    Threads >= Detail::ShapeVec::kContiguous,
    layout::PitchLinearShape<
        1,
        (Threads >= Detail::ShapeVec::kContiguous
             ? (Detail::ShapeVec::kStrided + (kThreads / Detail::ShapeVec::kContiguous 
                - 1)) / (kThreads / Detail::ShapeVec::kContiguous)
             : 0)>,
     layout::PitchLinearShape<Detail::ShapeVec::kContiguous / kThreads,
            Detail::ShapeVec::kStrided>>::type;         // <1, 4> ^tQJT1Rld

using Delta = typename platform::conditional<
    Threads >= Detail::ShapeVec::kContiguous,
    layout::PitchLinearShape<
      1,
      kThreads / Detail::ShapeVec::kContiguous
    >,
    layout::PitchLinearShape<
      kThreads * kElementsPerAccess,
      1
    >
  >::type;  // <1, 32> ^D2bhoVYH

using StorageShape = typename platform::conditional<
      Threads >= Detail::ShapeVec::kContiguous,
      layout::PitchLinearShape<Shape::kContiguous,
                      Iterations::kStrided*(kThreads / Detail::ShapeVec::kContiguous)>,
      layout::PitchLinearShape<Shape::kContiguous, Shape::kStrided>>::type;  <8, 128> ^GSWgYIap

    typename Shape,
    typename Element,
    typename Layout,
    int AdvanceRank,
    typename ThreadMap,
    int Alignment = sizeof_bits<Element>::value * ThreadMap::kElementsPerAccess / 8 ^OjDV07DA

成员： ^B2uMm5Sn

模板参数： ^dpIWWwum

static int const kAdvanceRank = AdvanceRank; ^NdgVoNug

using Shape = Shape_; ^nSpC3nhg

using Element = Element_; ^gitQ6MXv

using Layout = layout::PitchLinear; ^GFguaodc

using ThreadMap = ThreadMap_; ^gTVdInP3

using LongIndex = typename Layout::LongIndex; ^jKDjdTrr

using TensorRef = TensorRef<Element, Layout>;
using TensorCoord = typename Layout::TensorCoord; ^fVNJnMpy

using AccessType = AlignedArray<Element, ThreadMap::kElementsPerAccess, kAlignment>; ^10ASQ5h9

static int const kAlignment = Alignment; ^mqKz5SLU

using Index = typename Layout::Index; ^KgKfYkoj

using StrideIndex = typename Layout::Stride::Index; ^KJYtIQcD

using Fragment = Array<Element, ThreadMap::Iterations::kCount * 
      ThreadMap::kElementsPerAccess>; ^OcA2dCbC

uint8_t *pointer_; ^fjxWgKUR

StrideIndex stride_; ^ov9qQTGq

Index increment_strided_; ^orRtH6B0

Index increment_advance_; ^8Hs3KTZt

RegularTileIterator ^1AgPcdOz

TransposePitchLinearThreadMapSimt ^Qeeklpwa

    ThreadMap_ ^J2vmfCHU

成员： ^SI2yB2Bn

模板参数： ^nBOuTgTh

using ThreadMap = ThreadMap_; ^nwKS3Vyf

using TensorCoord = typename ThreadMap::TensorCoord; ^IR1Xj9wM

using Shape = typename ThreadMap::Shape; ^T6q5Cz1x

static int const kThreads = ThreadMap::kThreads; ^f2uqePOL

static int const kElementsPerAccess = ThreadMap::kElementsPerAccess; ^lJAnnC1T

using Iterations =
        layout::PitchLinearShape<ThreadMap::Iterations::kStrided,
        ThreadMap::Iterations::kContiguous>; ^DrhU6Nux

using ThreadAccessShape = typename ThreadMap::ThreadAccessShape; ^bxjGxaxR

using Delta =
        layout::PitchLinearShape<ThreadMap::Delta::kStrided,
        ThreadMap::Delta::kContiguous>; ^RkHAx2Oc

using ElementOutput = ElementOutput_;
using ElementSource = ElementSource_;
using ElementAccumulator = ElementAccumulator_;
using ElementCompute = ElementCompute_;
using ElementScalar = ElementCompute;
using ElementC = ElementSource_;
using ElementD = ElementOutput_;

static const ScaleType::Kind kScale = Scale;
using FragmentOutput = Array<ElementOutput, kCount>;
using FragmentSource = Array<ElementSource, kCount>;
using FragmentAccumulator = Array<ElementAccumulator, kCount>;
using FragmentCompute = Array<ElementCompute, kCount>;

static FloatRoundStyle const kRound = Round; ^XrYPZsUw

struct Params  ^fDdkynrs

ElementCompute alpha;                         
ElementCompute beta;                          
ElementCompute const *alpha_ptr;              
ElementCompute const *beta_ptr;               
ElementCompute const* const* alpha_ptr_array; 
ElementCompute const* const* beta_ptr_array; ^YfFWylpc

ElementCompute alpha_;
ElementCompute beta_; ^qpTREHxK

using Shape = Shape_; ^MoEpbFnV

using WarpMmaSimt = WarpMmaSimt_; ^nBnhb5cq

using OutputOp = OutputOp_; ^Il4kl3ye

static int const kElementsPerAccess = ElementsPerAccess; ^NbLJttah

static const int kPartitionsK = Shape::kK / WarpMmaSimt::Shape::kK; ^PY4l7ACf

using ElementOutput = typename OutputOp::ElementOutput;
using LayoutC = typename WarpMmaSimt::LayoutC;
using ElementAccumulator = typename WarpMmaSimt::ElementC;
static conv::StrideSupport const kStrideSupport = StrideSupport;
static int const kRank = Rank; ^9a6iAgpA

using OutputTileThreadMap = typename 
    cutlass::epilogue::threadblock::DefaultThreadMapSimt<
    Shape,
    typename WarpMmaSimt::Shape,
    typename WarpMmaSimt::Policy,
    kPartitionsK,
    ElementOutput,
    kElementsPerAccess
  >::Type; ^vekBhcLv

static bool const UseCUDAStore = platform::is_same<ElementOutput, double>::value; ^xWeZoj3u

using PackedOutputTileIterator = cutlass::epilogue::threadblock::PredicatedTileIterator<
    OutputTileThreadMap,
    ElementOutput,
    ScatterD,
    PermuteDLayout,
    UseCUDAStore
  >; ^6gcH7dLY

using StridedOutputTileIterator = cutlass::epilogue::threadblock::PredicatedTileIteratorConv<
    OutputTileThreadMap,
    ElementOutput,
    ScatterD,
    PermuteDLayout,
    UseCUDAStore,
    kRank
  >; ^Hy9DV6XY

using OutputTileIterator = typename platform::conditional<StrideSupport == cutlass::conv::StrideSupport::kUnity,
                                                            PackedOutputTileIterator,
                                                            StridedOutputTileIterator>::type; ^xImT2WDu

using AccumulatorFragmentIterator = cutlass::epilogue::warp::FragmentIteratorSimt<
    typename WarpMmaSimt::Shape,
    typename WarpMmaSimt::ThreadMma,
    layout::RowMajor,
    typename WarpMmaSimt::Policy
  >; ^1G1FqZWY

using WarpTileIterator = cutlass::epilogue::warp::TileIteratorSimt<
    typename WarpMmaSimt::Shape,
    typename WarpMmaSimt::ThreadMma,
    ElementAccumulator,
    layout::RowMajor,
    typename WarpMmaSimt::Policy
  >; ^mR1Yq4J6

using SharedLoadIterator = cutlass::epilogue::threadblock::SharedLoadIterator<
    typename OutputTileThreadMap::CompactedThreadMap,
    ElementAccumulator
  >;
 ^8J8yuX5q

using Padding = typename WarpTileIterator::Padding; ^J5Imy75y

using Epilogue = cutlass::epilogue::threadblock::Epilogue<
    Shape,
    WarpMmaSimt,
    kPartitionsK,
    OutputTileIterator,
    AccumulatorFragmentIterator,
    WarpTileIterator,
    SharedLoadIterator,
    OutputOp,
    Padding
  >; ^CFgQsAXL

using Shape = Shape_; ^dMHeogWu

using ElementA = ElementA_; ^HEftQC7G

using LayoutA = layout::RowMajor; ^ROJB2CAL

using ElementB = ElementB_; ^gMvOagel

using LayoutB = layout::RowMajor; ^h7bNkyGx

using ElementC = ElementC_; ^D8TFSEIe

using LayoutC = LayoutC_; ^gIAN4SdZ

using Operator = arch::OpMultiplyAdd; ^3SoGSTKR

Mma ^fa9A2GzY

模板参数 ^VIoH4Q47

成员 ^dJ1Am6Dw

typename Shape,
typename ElementA,
typename LayoutA,
typename ElementB,
typename LayoutB,
typename ElementC,
typename LayoutC,
typename Operator = arch::OpMultiplyAdd,
typename Enable = bool ^APSDDfXf

using FragmentA = Array<ElementA, Shape::kMK>; ^NCiMgbgr

using FragmentB = Array<ElementB, Shape::kKN>; ^UZhLAQbx

using FragmentC = Array<ElementC, Shape::kMN>; ^2D7ZUzbK

using [[Attachment/Canvas/CUTLASS.excalidraw.md#^LM1i620K|ArchMmaOperator]] = typename MmaGeneric<
                                    Shape,
                                    ElementA,
                                    LayoutA,
                                    ElementB,
                                    LayoutB,
                                    ElementC,
                                    LayoutC,
                                    Operator>::MmaOp; ^7aXX0brX

using Shape = Shape_; ^i6SiwaEi

using ElementA = ElementA_; ^00euGUp3

using LayoutA = layout::RowMajor; ^vGHqmPLo

using ElementB = ElementB_; ^cKjqXCfD

using LayoutB = layout::RowMajor; ^ZbStHiY6

using ElementC = ElementC_; ^l985x7MT

using LayoutC = LayoutC_; ^zjdogCIY

using Operator = arch::OpMultiplyAdd; ^HdMZEd6o

MmaGeneric ^LM1i620K

模板参数 ^qLcuXEaY

成员 ^YQZy4DnY

typename Shape_,
typename ElementA_,
typename LayoutA_,
typename ElementB_,
typename LayoutB_,
typename ElementC_,
typename LayoutC_,
typename Operator_ ^lacPGsU7

using FragmentA = Array<ElementA, Shape::kMK>; ^jakfl85M

using FragmentB = Array<ElementB, Shape::kKN>; ^IvG8iMCG

using FragmentC = Array<ElementC, Shape::kMN>; ^8kcwBQrb

using [[Attachment/Canvas/CUTLASS.excalidraw.md#^PWXl1Mo1|MmaOp]] = arch::Mma<
            gemm::GemmShape<1,1,1>,
            1,
            ElementA, LayoutA,
            ElementB, LayoutB,
            ElementC, LayoutC,
            Operator>; ^W2yx3kmJ

static bool const kMultipleOf2 = ((Shape::kM % 2 == 0) && (Shape::kN % 2 == 0)); ^MljewpFM

static bool const kAllFp32 = platform::is_same<ElementA, float>::value &&
      platform::is_same<ElementB, float>::value &&
      platform::is_same<ElementC, float>::value; ^MIoWAmFw

Mma ^PWXl1Mo1

    typename Shape_,
    int kThreads_,
    typename ElementA,
    typename LayoutA,
    typename ElementB,
    typename LayoutB,
    typename ElementC,
    typename LayoutC,
    typename Operator ^8WfJzw1c

成员： ^BmVpKMW4

模板参数： ^b2gP4ALK

using Shape = gemm::GemmShape<1, 1, 1>;
using Operator = Operator_;
using ElementC = ElementC_; ^NM7HWf65

PredicatedTileAccessIterator ^q36Texzx

    typename Shape, 
    typename Element, 
    typename Layout, 
    int AdvanceRank,
    typename ThreadMap, 
    typename AccessType, 
    bool Gather = false,
    typename PermuteLayout = layout::NoPermute ^ucmnk9ak

成员： ^7tNWFTD6

模板参数： ^phMrEuLt

static int const kAdvanceRank = AdvanceRank; ^KoxfNFNG

using Shape = Shape_; ^6hzA8oMq

using Element = Element_; ^8F7PTWdr

using Layout = layout::PitchLinear; ^kXsvxyA5

using ThreadMap = ThreadMap_; ^4cpR3Qn4

using Index = typename Layout::Index; ^xFBHBA7t

using LongIndex = typename Layout::LongIndex; ^xVXzqmrc

using TensorRef = TensorRef<Element, Layout>;
using TensorView = TensorView<Element, Layout>;
using TensorCoord = typename Layout::TensorCoord; ^3YPlxWQs

using Pointer = Element *;
using NonConstPointer = typename platform::remove_const<Element>::type *; ^AZHmysIZ

using AccessType = AccessType_; ^wtwAyZnT

static int const kAccessesPerVector = ThreadMap::kElementsPerAccess / AccessType::kElements; ^1ZfNxo5Q

using Mask = typename UnderlyingPredicates::Mask; ^OCs951Tk

class Params: ^hW9cpD6n

using Base = PredicatedTileAccessIteratorParams; ^SAGtTaBV

friend PredicatedTileIterator; ^j25nilDK

typename TileAccessIterator::Params params_; ^qskH5g41

using BytePointer = char *; ^D0NkDWfk

using [[Attachment/Canvas/CUTLASS.excalidraw.md#^_atUGs6LLl6w_dCvr5ZOz|UnderlyingPredicates]] = PredicatedTileAccessIteratorPredicates<
      Shape, Element, Layout, AdvanceRank, ThreadMap, AccessType>; ^rPYpOfRf

static bool constexpr Permute = !platform::is_same<PermuteLayout, 
       layout::NoPermute>::value && !platform::is_same<PermuteLayout,
       layout::InversePermute<layout::NoPermute>>::value; ^ORwnQHtX

UnderlyingPredicates the_predicates; ^wc1gF584

Params params_; ^PsJanLtG

BytePointer pointer_; ^M4cYXe3e

bool is_residue_tile_; ^wXDwXpcJ

int const *indices_; ^GgQjxHtb

PermuteLayout permute_layout_; ^E3rs6OOV

TensorCoord coord_offset_; ^e1F36wwh

PredicatedTileAccessIteratorPredicates ^3cWpwBb8

    typename Shape, 
    typename Element, 
    typename Layout, 
    int AdvanceRank,
    typename ThreadMap, 
    typename AccessType, 
    bool Gather = false,
    typename PermuteLayout = layout::NoPermute ^OUYmKi5T

成员： ^AL3WY6nD

模板参数： ^sPJObqVu

static int const kAdvanceRank = AdvanceRank; ^6gz4HkoA

using Shape = Shape_; ^fcEsmbQJ

using Element = Element_; ^Gi58m4eR

using Layout = layout::PitchLinear; ^TZUhPS2K

using ThreadMap = ThreadMap_; ^1om2w8bK

using Index = typename Layout::Index; ^hEc9aJ21

using LongIndex = typename Layout::LongIndex; ^kRR5PMHv

using TensorCoord = typename Layout::TensorCoord; ^vVjWWpvH

using AccessType = AccessType_; ^OC0eeJNI

static int const kAccessesPerVector = ThreadMap::kElementsPerAccess / AccessType::kElements; ^6rSXn9ip

static int const kPredicatesPerByte = 4; ^C1ADYdkL

static int const kPredicatesPerWord = 4 * kPredicatesPerByte; ^nvuka2hT

static int const kPredicateCount = ThreadMap::Iterations::kCount * kAccessesPerVector; ^wxzrgQaV

static int const kPredicateByteCount =
    (kPredicateCount + kPredicatesPerByte - 1) / kPredicatesPerByte;
  static int const kPredicateWordCount = (kPredicateByteCount + 3) / 4; ^uTDIjNnE

static unsigned const kPredicateMask = (1u << kPredicatesPerByte) - 1u; ^9DVEPOJz

using Mask = Array<uint32_t, kPredicateWordCount>; ^Rj8ofheh

uint32_t predicates_[kPredicateWordCount]; ^Csvv5SEI

TensorCoord extent_; ^enxTTGzS

TensorCoord thread_offset_; ^ScL8UrbT

int iteration_vector_;
int iteration_contiguous_;
int iteration_strided_; ^pEcViqi6

TensorCoord residue_offset_; ^zL4QEH3c

## Element Links
vuQmMRAB: [[ThreadblockShape]]

bRqog8nu: [[ThreadblockShape]]

%%
## Drawing
```compressed-json
N4KAkARALgngDgUwgLgAQQQDwMYEMA2AlgCYBOuA7hADTgQBuCpAzoQPYB2KqATLZMzYBXUtiRoIACyhQ4zZAHoFAc0JRJQgEYA6bGwC2CgF7N6hbEcK4OCtptbErHALRY8RMpWdx8Q1TdIEfARcZgRmBShcZQUebQBGABZtAAYaOiCEfQQOKGZuAG1wMFAwMogSbggAcQAFXAA5ACEeACUARXSyyFhEKqgsKC7yzG5nAFYATgAOFP5ymG4eaZ45

4sgKEnVueOnp7QB2VYBmADYDxOP408SU0751qQRCZWklte6Ia2Vg7g/y5hQUhsADWCAAwmx8GxSFUAMTxBCIxHDSCaXDYEHKYFCDjESHQ2ESIHWZhwXCBXKoiAAM0I+HwAGVYL8JIIPNTAcCwQB1LaSHbzARA0EIZkwVnodmVIUQHGvDjhfJof6QNjk7BqRZoeIpVUQbHCOAASWIytQRW6kGcxsIABkeAA1AAaxwAjlBlPRJO1gQBBGCO8ZpIWQA

7GgD6AFVmCkoAAJJL4ADyzrt7uOrQo8ZpEHWAF1ZTTyNlTVUAGI0+KEFIIR2EZ3kABamHB9DtIOTzhpcGphDxWCqxEmcAA0gArGlGacz2rUnF4xXMc0cIQM2VhBDEHbxY7jRI8M4pA6yxgsdhcNCrU6npisTgNThiHbTY4HybjXXjWWEZgAEUyAZtzQGkCDCWVNGEPEAFFgmyXJzQKQtHiEOBiFwICdgOU4jxSHgjniHhEllIgOBBbhV3XR5oUxL

duFA/BwMeAZMCGCRIKhVBGR8NRR0ZJgrHwVAAF5UAY8D50oAAVQYqg4oTuKIKA+IEggRLEsCEGoakaU4KBGUIIxxFQa4iz08tcH0eltVQb9mMGP0iGUS90GCGkhlvUgoHMAhHJeFzoHVak9FyXB+yYMs0Eo/BZRhF5+wIGTWLkthOMU3j+NIQT1PErTqVwIQoDYVpwkM4ygSELTqPC+NnleNiTO0HhxmKABfeZSnKSoJFaTQDgQZ141aKTqV6Yza

RLJBZVGNBnFOFJbllGzr31TZiG2HUbn1SQ6reK8eFWWVvklfUuVFAkYXhTRrupdFMUNXF8ShS7iXIDgyQpHIPMeOkGXFSUIGlbcNxFXl+SWEHuTFFlxqB+dhAVJU/lijUtR2PVZQek0zUKZDPmLSyEEi1Bop/AcZvQYcx0nGdZ0deHHqXFc1xix5N2Akz8NuRJ4gOcZjk8+8XPGaZiMeM8hcfDhnx1A5dXuU5TnGE9Hl/ADgkwkDNIgqDiFgrIvs

QvHylQ9DNZM7C92OSYeBmY5jhVz5SPIqKWZIthaI53LZWwIRAQMP8MNwbhOvKAnsguolUFDgFQfGuE/0SRPE9RMP6XweE/T/LOs9TyAOEJyP4QOP0S5LvMrVa9Z2seAvsiHBAzGfNqOtVjmvgoEFx2TCMKGwAApZwoztYgEHaQhHTgR07QFWUxqqQJsCiDgfimx4KbmvV9WWpJVvBtBTkmSZtCP0+z7P2UdpePbUGt7QlZ4Xnrd3Ba31Fo6V5OyH

zueqOIARJEgDboYixAuJ6hJ4Q0hpNgSYMCdLp3+rDKEMo2Zxz5OtAUOpv5gkQVUOGsp5SSCZsjR46oMRox1BjR4WNTSIXWNaW0DoXTuk9N6X0bAAxBhDPQiA4ZoyxgTEmVM6Y3SZmzLmAsRZJrEwgJWastZ6yNlwC2NsHYuw9j7OTIcI4JxTlpkYToBDdbENdlRT47N3i3EPHLEWgsLzcBwvqCWF4pYy1vnhU4sxDyTB/P+QCdEtaMSqp8SCj19b

wTyIUehMcejwHGixb6VpIDdXQKCZgyZuLxjnPQqu3RjaQFNhhAJFscKnF5pMPmiRZgkX7C7EmbtqIezBF7TSLdiihwqO3NJGS4BZNGnE/oslppjHGOcFIJ8RZJE/DzeaDxPg70SMfSYL9LiJAOCsMpso1obVQDzE+1xbZ4VfCcQidlPhX3qjsY+GyUjTEPqM8Yj8LhzPKMdYyp045FwkAA5Ea8QkgIeniL56ASTvXJJSRJac/owzwcg4GqCoboJ2

S84UUNcFsjhQzRGy5BSkNRrAdG+oaE4zQJaT4Np7ROldB6L0Pp/SBmDKnXhkYYxxkTIkFMaYMxZhzBXMo+SJqExkXImsdYGzNlbO2Ts3Zexk1HhTCAVNdH6IMdUBmi4kamNZuYhAxTea7BmHqSYTi7z2IPvhOxD4nzGUPvEI+B1la+PVggc23tHihJgnBQ2uNZSFPNrzUpep8KPyIjUsiFEGlOyacUt1nwElVGqFkfQkkKBJQajUJNOk9IGSMjsc

Y2g7m3PuQ/J+NszK5AslZfANkznlASX5ZyVQxC5CYNSM83l3ANoClZYgxBfg+z0mFRUpAZGUC7j3Pug9h6j3HpPaes9SFZX8IlWSEhE36GTUdQqxVSq5rQBVYJ5RSIIFqtfBq8QmotTKDXMoHSUkQFaJgHg4JkxwDdGwY4xBxwAFlnC1E0KQTA45+7lj+XWgZEhF7L1XtSDe80qHzKWOU++jzS0rL3Fs/eqBD5LPPnhnxjwLk3zviWg4z9jivyqW

LT4bySHmM+b/eEvyUQQQBWA4F0A3ofQhfA6FEokEcmwQgJFmCTJCfRVKTFRi/BEM1agfUZDNQEsoUSnE2M6FWggBSph1LWF0o4Qy7hmm+GssERy4R3LxF8rAAK8ORN24ioUeK5Rkq1Eys0fK7R1M9G03aPTaTGqcVao3LqjmDrRn2wOpalyitpjRdccZeIdq+bYSVrW5JfiNYxu1u63W4TvWkuifQ2JfRiRDOK50ptDRyz4CMMoHko5rNgFyfy31

aEikcwDbhYNhFqNHtqRGsxR7o0tKCW029bcqs1bqw1/ppWQXlc+BvKYH4lpIemMfB5TzyMLXSxAbZomcLaEuBcY4G2biiwdntoj57rk8Hmncj8pGDwfxgyqITHG4QIBSFcK4wD7rscY69Uk4Kvq8aZDCjFgmEWihExDGHODIeSeh58QhJixN4vIcpkyCHyjEo0+SxhVKWG0vYZwxloZmX8LZUIrloieUSLyVIoVDmqyisURK1R0qNFysHBIJVNN9

GtHVcQdHpM2ahawq+DboyLjRYcbY8WpqrXS0S7qa2ttJgC1Vpll12Wgk6zCV6hCPqULtf9ZbeDPXQ3VXDcFxpnt6I5bjau9AgBCK0AP7mgAh5UAA6mKa01VE977rNuQc2JfzYWx7W3S0otpOZSy1luB7frU5AKblIWQHbT5fAXaqg9r7WByAIUojhRHe3R9z7X3vs/T+v9AGgMgcLxAOKy78AB4kEHv3W6iolVYHu1AB6w0nt2uey942SiTfYn6I

QtROBSUSEIeg1QbRQG/d+04uB4jQUmHN8aUHaPDNmvB7eSwDp7wwQ4o+J98Pn0viP7g6z9S0fewjiEQP0A/KAaxgHusOOgu42DkWAgkjoDFJq/nDlgq/hJqASjuUGjnJgpvijZLqKpkaLQlEpptpsTjSmwvSlwkyiZgIuypyiImIrypIj9NIqzvImKkoiolKuorKqrFovzjooLrTK0P5o8GAmLpGgCJLmgG+FUsavEMGPLmgOsvEPFtatwNMGRoe

IkDzHtmrP4qNkxCEnlsbpEqSgKn6nqpbkGgRDbk7ANvblGo7oEmEOPneu3PEA0JIP3DyOCI6K0AAJp+gcARikD9zQTKAwANDOApDxi74LwIBLwH7rxjDH5raCFbyYYX4HxX43636Eb36CHHykY7arLvyPDP7yYfbv7/zMZN53SgK/6FH/6g5UhAF8YAz4LgFYZSFQEgH1Go4IyyZBb5GY5KbIG46QD44YGfB/i1DJiOjJi4AAQ0jjiuH0DQR6yJB

RgVLYBcCU6EE07mZ05kGM6taUEs4Vhs5OZ0Gubc5MGfD9ieasHeYqqMgi68FDYCACEWx4SXDjCfhNGfDOKcAP66jSGq7owba3B7iPxOqqFO4G65ZG4Gwm6FZWgxLQAQYLbJShjJLtxQBNjVDtCSCuE8j9xNYtY2ZtZmz6GBp4RGF9aQDOyDbarDYWEaRjbXqtxnGonomYnYm4lzwInQCLYjAjJHx7Y2QUYpBLLoZrIbLvHlAHbcBXCHC7ByyTBVJ

ESPxyHXZpEmR3YPbFqobPKvZfyv6fbfa/bxD/ZlGPR/5cZVEZ60jAH8awqwGoqw5Yax5nSI42lQ4oJtEybo7ilqhIGEqYxqboEwlDEjFjETEIBTEzFzHQQLFLErE8JrFmYkGWbkFM67GljUHs7Ob0FuY87MEXGUxsE+YziMhqoBai5ybi46rFJXCnzHiXDiGoByw3hK7ngq5uJJYPxkZlLKG66urO7lAep6xaFGyEkdZYQknW7kkQCUlmE0nNJgn

qF1qu4QCAAIRoABoq/uy565Ie+kZUeaBaGpT2WpGu5aUAlaieaAyeDkqeVQ6ebaTAHavkN5Eg+e/ajwxeQ6EUth9hjhzhbhHhXhPhfhARQR1ILeCUbeW5G53eO6fe5UpAlUQ+p6lyOoY+DJ7Sk+6A5Y44mAo45Yfozo08zo4I+A1Q0wxAxwUA4wygo4f4IRkGYR0Gb5S2URcRjwApC05+OyOG1+yRR8d+Z6O41SuRn87yBREC3yxRxpgK4CL0IK5

pn01RP01pdRYB9GiKjR4mLRalcB7R6OiBWOvRqBqEgZFoPC8Y9AfouAHsBw+AcAxwOAQgEYKQbo7QBwMwiQBBLKRBtOpBDO1mtmVB+xNBHOLmXOjBHmfOBZVx+ijIXBHpgWzM9xgMjxywKwCpvMDZH4U5nxHACW7wFGkwZSREBGZxvZ+ui5aImhUJ2hFouh5uxJ3WZJYadSlZc5FVCA1hmFEAjyblUY2Amg5Y9FiJlpy2hqMR7i808ROyiQ80BaP

MZwRVywxq9wKpglEhx8FGqy6y92Ra8pOpYlephRn+LG7qbG5REl8lIOillpv0EOrpyO7pscGlCRvAWlD1MBT1kA8BnR3pzevpKm/paBJKZlmmFlVlNldlDlvszlrl7losXl1OiZFm9OVmFB+MQVEgjmtBnODB7mvOCqAuRZ04I0ZZdx1JDxeqn40wn4Is9sDZSWpkLZksMhss+4LxOEf1KhWWahh6VVkJESI5ZuRJnWBhpJIaU5M59SyVNE85lhf

NXJyUEgfs/YygqAMk70MIJUNIfo6kGtggpA2tAAPPlrkLrSFICNQKgHaLgDAMIFAH6AAHwADcAAOquKwCvOrTkAbdrU0HrT7VreGSbVof7RbVAFbTbXbYVE0C7e7SrV7frUHTSOCAHZrYbcHabVAKneHZHbbfbeCHHR7ard7endrX+Gnb7ZnVoeCHndHdnS7ZuUregAnWrUnRnTrZXcnSHTVebZwJbdbfnYVE7W7cXYnYHR3f7aJO3cbVnWHf3RH

YPfXbHaPa3aXVXSnV3R3T3REjnQvXXQXUXWvTPeGRXdPRPbPTXQfYVIXc7TuWHvuZHpqdtnIaeeedWknnPNef5LeeGZaVnp2s+SCkFAOqFKXsTG1WqEuhBe3i3Z7W3RfeGbrefWXdXb3agLnUvfbSPfHfA+vcnVPfg9vXPRg/vVgzHUfXgyfZvSgxvTvV9Hve9IvVHYfavVQ4gzSGfUQ5fTVbXeQw3XfTBb3nufughQrceshTfBes1F1UyVUNBMc

BGJgFZcaM6KcPoJoI6PGMaJoH+HAM4B+jwMNXKIxRESxbNNMifleI8sfDzAGmsikCLOldNYdn0U8OtbwJ4gdXRs9T/JdUUYAqdf8j/qaRUQpTxjUfdapXaYDGgppc0R9a0bpZ6QgSjIZX6dQgGSDWSuUBGNBEIEYLUIkMmPGBGDAIQJgDANBFALUBQEILgKcHaEyqEikE0MmCkDSOMDSNBK0OMBuk0E0I6KQCCPTOjWHJjVhQcTjWFXjbmWcSwdF

cqvolGLcRWXwZTZ1rNatecFrg2Thr8e2Z4qMgeG8SCTzQuQrYOVnULZ8HoaLThK8XuPuGcHtlLZA9OSNhc7I11O3P3JgK4RQBQLgH6MwDAPEPoG09MNBK4V3K4UINgMY/vqJbBiMrvBNYeG45KQfG4zdlKckMsMrEkAS3LO+N4y/upX43JQE78tJYDv45UTdeDtAUk/aWDK9X9c6dDIkzpd9Xpak90RQjjsZepoMQOcIK0+0509070/04M8M6M6m

RjXsVjVM6FdmScZFYTYWSqlGNMKs50e8xYleLqM1G+ErA2WsjlcrnlazSZDcLzEKbcKVV1OVbzYbp6jVQThNkkvCfNorRnj81UH+NgPEBwJsE2EgDkmMwUg1fc08085cOa7bq1esx87SblN8yiUGyG2G4QBG8Y/GofqgM4IsvhAWh5YkJ+LbNbJW+ixRvsISweDTSS8sFOVixbPmu5Z4sas1HIRtncAJShRbAkE2428S0kM2TRsi+S742CJ9lJd/

iaUCmE9dRE8pbUQJl9bEy9ciu9dE1uz9eaH9YpoKygUDSZdkzwi020x010z030xC3KyMwFczumcFZmUceFfjXmVFYqtq8s6WdwcYms8lUa7wHcFrg8wcI7OULldwJW7HrlflZebuBRvbPWTrs6n2eCRoQLQVnVaORbg8/zK8Ym686YdLRTWm3LXSZVf60G+GQVPgFAOuvoJCBwHSMoCIBhPYgQtJMuZMUxyx0mux5x9x95F8aeQ/TqMcDKaOy282

6IW/Qnh/ZeV/axLnhIM2gMLCJ5I+TnkAxAK+U3h+eA78/84C8C6C+C5C9C7C/C2BdAwXJBc3RAIJ2uMJxuqJy8OJ7x7kdusI/3oPrbsPh49I1es1oyYGxIO0OMK0P3COPoM6E2MaPQP3K4bBMoE0FJJMDSE2Ii6Y8i0WxMGi+xUsOh01Ip/J0kC4w4ji6qW+JV2O1V7bGS10RS3O8dQu2dSE8u/S+E4Aeu1E5u/Ch18JvE2N8yzy3KHy50QZT0Rk

58AMUGWK0IBK7e9Kw+wM0M8+9G4Km+yqyFVmccRFQTV5ks7TFGPFck4lVSSFnqsVfhHcntnByh39Uh7a4/LNSsLbNB2c3rm6xCR64LaKxPj6/PGVkiRVvevgE2PGPoLgOWBwE0HiXt3c+OfG6Ry8y1Xdw7jRxm+hd69F65HDwj0jyjxyX64W5ERY7uPyeVweAWs12OzBxsFhvuAWr1mdkVVRldoO1IwcCO+O9V7NW1x8lDPqT9ruEaYuzJWaau4N

/jCpSN5yHE69U6XHFNzE4e7ip8Ce9jme5k8DV6/zet1K/e7KztwqzsUqwd5M0d5+7M6cV1As3+zFZdxnGTSB1R2B8cF9ykElrW8zWarshtgc8ZH7ygVUu5f99h3R1c8Oabrc7Gxj481j0myYXbpR+7Om/2T0MuZ3k3emoX1JyIyZLJwp8zzYsp1WjWupw7YZ3eXp9npp+gMZ8FIOmZ1ULF/F4l8l6l+l5l9l7l/l7FE5yuq5yX/5z3ruvBYhSF5I

6PjI4T2D8TxAE2PoKOOCDwE0NMKIpoBGGRpMJv1ZYQN+uCAV+EUVzT8W5Y3W54toFX0SzkZ8O2/Bvz+es1AWnuLcoCWcLuDzDF7iUqWJ1EoudVCb9cFeSlJXhu1tJbtOWEBDHJN20ra9Zu5oebqezcbLdQauHM3nexlaPsreL7NMvZnfaHFcaOZZ3skld5E0VU13XlozG973cOY2EXrI/Adh7NFC4fHcDWS1x7BnWGWLDh1XdZDlPWoPOEvenoBC

B2g+gb9K0D9AU8KsEPEaqj0VYmwU+ssYjgm2x7Jtce5hfHq0hX42EqgUgmQXIIUEFtuSkAZbHTzrZkZH+lfBTm20dK3JH+dwfmMeDKRyxlSH/dGHEBa4BC4sIlN7O11nZv5/GoA2lhdSpYMs12MA4bnANG5hDEBGvNFCgIPZoDde5QfXkZXPYisVupvG9ubwIHbd5WxA23qQMO4fsKBGrM7pcQu4zhHQgHBKuWQNapswOdqRWA42NQvdrWSeIqtw

J1DGoKM2EVYKzwqCusLmIg65kn3UEi1U+JHZ5hn36xZ8IABQAoFJEkCBBcAxATQLLUZCSBcAiAfMPmGpCy1hB9kVzqgAHxxI64CAVAK+iYAYQYQJFUIMwGoDu1rhY0O4agD9CiBJAUkaIB8I4BfDbhhMVAFnT9BW1PhNwxAD8LnrQiQRsInIOCKzq10YR3w1EVoT9DYBfY+gNcM8N07cF+OVw5ET8MeHkAiopAV4cuGBGgi4R4Iv4dgABFAiMRYI

7IBCOxGIj6RKIjkQiNQBsiGRfIq+oKN5H3DIRuIoQPiPwCEj76ZfXcHJ2F7js9sukCtCpzr6XCG+P9CQE3xbL6dW+gUKgRAFM7DoZEG/Lfjvz37HAD+R/E/mFHP6Od4oznWBhAB5HkjEAlIl4TKNpGiifhTIlkcoDpFkisRvdbkcGOFE1UmgYYzERGN3pBiYx4o7EZKOlGyihGs/bgMF0z6hch24XTNpVgkDOBsAxARkOMCaCjgow+AEEDiWghwB

xwBjJsPhFOCX8mKTeGwYtDK5XhFkyQW4ErEIjzRgwL2R4G/3q4eNGuSogIXtjyLi9KWf8SIbLzpYxCBu0AqFAkLdJJDWW43dlnuxV7SZsU6AtJgt0BpG8L2JvCANe0lb4CtuT7a3gSRIHCpVWx3L9nMxd75k3eDQ6cI6HoEzdGBbQ0Do8WEJTANsHA4Pl8UvL3BBhofPcNB3mhc1Jh8taYYnwKFwllB9HZEvmPQBYBEgkgPkE2G3BRs1BMbeYZoM

x5LDyOWfd5ucN5p5j70mE7CXm3XG+t4kVgiADYIwwdjeAwYOIBUncp2NsIDjbXK/0dKC8exj8eUulSVJ8xfBssIXhOJJaTtXk07UIRuMl6GkohEAxcVANurK9EhqvHdqJlSGigteGQlJr9QPGYDhWplHJoUIvGbdLeZQvbnZnvEO8ahp3H9lq3d4zgeQ+rJKj70eJ8kKkeEemiBOFhh9gpyHC2BcHmjQcpgsfC4bh2B74ckIhHRqmn1Ik49ZyFJT

5vBM1FVBtyfHVNFBTlH94FRjg5/oSxVHx5a+n9TUQaN1EfEHyLfQzu31AYl5TR7cQscWNLHljKx1Y2sfWMbGOjW8LovKdP1gpl9MxKw7MVIzQqRcMKcjCQCkGqCMhlp0EUcESLjSclqe5jO1lY12ScVau+0cZMcjOxn4vuosKcriyvBP5FJ04zrhEO67BMl2slP+LEMV4rijJDEhARNzCEfSsUHRfcQKwN5YCsmp4qSE0DYDOhMAW4b9DSAoAwBM

A+gZMP3DgAkAYApAQxDwmUBGBHQboY0FhJBDVA/wzoVwrUBgBSQow36TQLUD/D5AHJEzWRA+Md6UDNW53dgp5KMZe9fxvkvVJWwqSASrgDNDDvVNbI2s/iOoJLCLCPheDYpgPeKaIJB46FkpcbVKWR3SnZ88ecUpcq5zXoUjCR6kXWVSIjCCNiRBU7WXgwNkwh9ZHowkUbKKmJZ5JkAVUWeXVHVSXcGnRvn/XvJeRGp2o4BkaJNFfk9B2Q8fi53T

Q6zrZVIq2U8MNnGyaMAXdMaI3n5ZjF+OwGaTelX5Zssa7QEENMEwDMAowBwSwVD22nXBdpPYriqJjQ4nwFosyRxseGmCnIpJ4HIAUdXumBMwBvXZ6f0CXFaTYBa43SQ6S3EJN92DEnXjOx9LpMjxS3EGaDwgBgyIZUM4gDDLhkIykZKM4gGjIxmaYsZOMvGZIAJlEySZZMimVTJpnlDxmyre3tUJmbMy6hizNmdOB5CeVOZPk5gS+EcbXBXiFqYK

TsCD7CyWaYsjibMEPgHgBJLrIQbLNW4JToSBHYWmOWIkqydBmfFNjLSym0cFa8aZWng0ZDZB9A9QXtKrWQZcRcF+CxwCvD9C2z8pLotejgqyCkLCF6kWhXgp2FkLlAFC2OWHGzTyiHZceNUVVLU41SPZ7kL2fqMM5FR/ZnfNqUHKgZOiJ+Yc7BSQpYUMLRITC+heQsoWjTAuc/cRjVFVK5ijB3VJsN+iMCtBcAkwaCFQEp5MTi5PJTaGXMIgVy8W

yQJLPdgOC3IPKfPVIh40OjBDdSY3edu3LUl9cNJYKRlpE1+lCZEBHLTXukNHmZDx5/1SeUKzyGWSeE88yGdDNhnwzEZyM1GejKZQ7zcZ+MwmcTNJnkzKZ1M2mQRP26VCr55Am+bULcmsziaRgHkE2JfnSKUqxSdykVQoyPMGaoyCCbsBwgfgbYQUsqhAqmFA95ZiU+qkRJKQkTVZugjKdR01n59XOgIHjtgFQD9goApDJhqgBBD1BvZEnd6KOHUj

HLvIpy5gKOA4XfUSR6aTZR2h2W5B9lgIQ5ZcrUAXgblFyikFcu+W3K7ZOwHhU7Pfoai3ZWoxtDqM9nN9AGvsw0R3zAZSKVl4FZ0cuSeXmAXley8Oh8r+VfL+65y0SJ8uuWAq0xcFDMWIyQp6K05UXTOegEkC1Amg4IccGwCjDKAi5o1PNJtl5iKFg0UvH7GAsgA2RbgG2Y7AGmWT7h+Yb4A6ZNXvhEQcIDsftqtSbk+Kp2IQ26eEJAEPSBy4A4JS

9J7lMtYlA8tljsmiVpDuWqAkyQDL14A1klx4/ITgPKDpLF5y87JWvLyVbylu2MopfvJKVHzylp8qpTbwvl28GZzkhpa5PmavjaB+iHkHqw6UrLfeDsC4Frn4o/ydQg4/+S4k+6OMHYe4PYOMO5oA8plcsmYYrLgVEdFlSClYSgqo6USS1Ws+RSXVUVKKV4hDFtQQrbUaLUcDyqoDQsUWdqsujCgdawqaDdrOFoebhTXwvK2R6+tUmFXqJ9lQq/Zi

K1qYHJRUhzqFCiuha2qHUqKR1qtMdXcq+DxzyVicnRYqBTmoVl+s0onnSoNAHBhwxoZMAgCISaB+4OcvYOCH7hUy7QX4lCUixgxFsHFE1KxI4sSK4Y+KAg9xjmLvhQaj4f1KccANnHaq0QuqrucDlCVxD3pRqyJd9I3ERKgOVqnxhPMPF2rp5xvWec6syUrycl68zeQUu9V7yD5pS4+RUrPl0zL5Ya6+eq0jUvjf2Ma2mDyD9DeTOlYHR+HcCtiX

BBVDAPoZtEeRDKpgZGEWHzBk1Fq4+lzaqgrNgXJ95lXWRBcsIpIUcKJaCgnreoznoSIAxoO0NUB5A0gDgmgY4E0GwAwAYArQUgIo3oC6QoAcZDaX60A3MVbFnMXaYqvA2NkRxQ7EjMeWWRbU3w0GpDa3K1WBL5x0Q/VZpMNUWr4BavU1duJ0m7j/pJGxJWRsN4UaTxs86YA7VbB2oIwboDgP3A4CkA28PAJsHaGmBSQqB/RJjcUsPllKT5lS8+Y7

PpnY01WJ3b9lGoE3/shNF/BNerKrJhZ7sjiG4NBte445VsYU21m4utiix7gamuCegoQliCkJSgzaVYLX7tBWtjIZYjyGOCqDg1hE+BQsoM1kTa1OfAwfSXM3GCYuF2q7TdqsWDIbF1g+Dn7wSBPdK2fE0uUzUQyCF1kwpWLaKXux/V22fMQ4IoXrZrJrgXbaDZdLVIFpdqUeZ7JOJunIamMyWnrk9Pl5Ya3pjs7Sf3Lw3q9cttOojXuKyGkbzJKS

y9ppgq1+gqtkwGrXVoa1NaWtbWjrQaC62+qetbGwNQNpqVOSeNo258dQOjWTbPJzQm7q0NfkS5ikihCpIRGagM0J2EE3aosluQNyZZDa/mtAtqpJSK1KUxYUsuQWdL612UiFVUG/QI9Z8RAFzUX1d3u7kEXu0vsVIjyHlo8R/EFZVJnVXl3Z8K7Tq2lhVPl4VzU98pIvXXoBrNtm+zY5uc2ub3Nnm7zb5uDmyLQ5Pu3AB7vMCLAyV40ylQv2pU3r

05n2kFK4VwDtAcAf4EEEIGqDXQeAxAY0MaH7inAYAkgdlX9oYpX8gNN/EDexLOx2oUM22GLa/D2xI6ItxGDItFpFLxaidiWlDaTsely8V2lO5cdTr7mPVPp2W0TGasMm4amdBWhJTkMW544Z5BQiAFzp51876tjWqSM1ta3tbGNu87raxoDX9bONoa4bY+Kd4sz6hD81pcaFE2JrHiDc+aGMgD766/eEEnCDTWVhyxYJky53VApmUwKrJyEk7QDr

X5QBWgYbJsJoCiC3bbxumh7fprt3VqjN5E1Nk7vQXUTUSZBigBQaoPD6VBwGvXZPpWCbZMic+v+RKSwxHYTslwc7Lz0kleKcx6pPaiHrEOQAEt/i46gaWl5BKMNV1ffb3NXHH7jVm43dsPJ3FX6vSZkoGRZI52fBn94IarbVrf2C6v9Iuwpcxr9W9b2NQamgyGtqXcb6lvGsbfxvcnvjWljWGbYa0eIXA3FKwa2DJpW0dl4j1rcKTMBuA7NgJEy0

EjgYt14Grdcyug5bCe1qyTNufHDo2sDze4u8Jsl0VP3xhcLA9B5JQyIbD18KI9c6oRf/Qalwrl1RnEgAXlXWfky8/QRvc3uwCt729ne7vb3v72D7BpMDAvpUfyinrK9ScyaVesai17aVlm+QfQFcJQAKAkgMmYQEZDXRcA9WhABGBpDEBRgvBkxqPsC2A6dQqqhYFKULQyq3F20PRc8dUMb71DbcmlilvUlpa9DGWkeUYaiUM7DD+W/SpYdyH2rU

lnOyrfYd52OGBdH+oXd/spxuG/9/qvrRxuqWOSMyAR+XUaPOITaPJj879DAdm38Etd/MbCCbuW1yby+hEIZWgZpozBDwZu7I2eK02JSis4PIgwG3vXVAc8tQBoEcFxL4S7tEAdHggoYOGbpyxmlg6ZsMEfbuqopv0OKclMcqUWTx+noIWe4yqMWYqm4I2MIhbVxh2O2YKkC3h2n7TiG342EICUAmydu+yASCfCWX6xuEJ0w3lvMP8sbVSSkrffso

2P67DDh/ne/s/3C6f9Pqljbia8PS7CTZA6ZoEYV0VAaByux+cmGpORG9UC0HaoqV6Eiyk8XJ9bYAutgzBlkRqbk/tumVlqdNcwgo1oPT7PbHdqpso+svTTXC3R4Ii2aQAjBBi+zHIjtawooXDnwx9wsc4eqHMwjrhuy1AMSoBXqQZe1R5cr2anMPCI5MIOc0iJHPTmD16iycwmOIU7rB1Y6yc4ueXMErVzQKzaLaftNPmFo061TrOsEXwq6psHLo

/Hp6PiKBjXfHqH6F2P7HDjUkY46cfOOXHrjcxtFaSIPPbno5u5k8+yMPPnnxze5zc6eZnNdqrzrym82crvMV6guVe5OTXoi517uqyYcYMmGIDTA2A6JGkPhSki4yEAf4YgD2CbBsBmxZjILRPqh23xxqQ4rDB8ablRbZ9a+luX8aS2umd9C44EwAQP1Wkj9n1E/XpJZ3bsL9mWuJcRpv22qQz/RB/Y6qLyEAIwRgQDPoEwApAow0CR0AcCbDWjv0

7QGAOODjPuGJdAB/EzKZTNVDiTT40k1mYpOtLPVDA27rAb1SiE7gBqDI9+dLNPGvGFZ9sq8RFjnBH4PZbA/WdLWITjLhBqnqdvvXjA2AdoCgK0r6jUH8jlaoo8sppOZTSjVhAxfNPQCFXirpVwubca2l8WBDAlrbbDsow7VNkwl16tKW8FykFSB4WUmtQUO47g9BOqS86Y0NS8/sgJvVd3PS1entL4Jx0pCdUt/SLDgMuE6VodVWTjRpl8ywjKss

2XsAdlhy5oCcsuW3LOJzw1LqAN+GQDTMxpeNpCOQGeQNxCI+0MeJEtbYGyU3ems5huKhl33ZLE6zrOxpcDjZ63bQcqsKn2zKy1g7De7O5ToK651ziNLqOTqGjT9I8rPpaPOz+F75iFfOuEVx6DOCevow8eNHJ6hjEgai7RfouMXmLrF9i5xe4tj9C9w0rG3HJn5nqB8pFtY+RfYNVA4A/cYMOCH0D0BMADK+MOMCbCt7XCxAereMHIi3GAtrYsYA

chC13Awt7/eQzfFuCPnnzeoTKr4sOrSWt9slnVZ3Ip2KX9DhGn0/hs0sukwT0JwM9kP0vAywzxls8YkEwD4BJAPARkGwHaDYUw7o4GAOGXHDOgoAmgJlHABgBNgmg+Ib9PQGNCjgtc4wUcAgGTDfoyZf4YXJTnoAcB4gjIN0PGFOCaB4wPAOAOCD9DVAoANIU4FAHQhGBkz9M+sOwAODjhcA0EZzb4VwCtA7Q4wZcH6DhbgH75LSnkF+J4JMDNdL

Ao4LbCVhsSs1oE7DG4w+6ALCW+ER5OMvAVZHMrcN7KwQeO15XiD96t0FGA4DMqCmVJ6Uz4fu2VW4j+qYwjWo7N1XOqDVtfnfYfvjgn7up4rkkAoz3xFYB0G2FUhrnQaOKcsY7OOMJY8L22B0C9JFkraiwiqQpTxecj0WC8yppU0XtbcK2csXTX+N0/JdWuemhurt5IVtb9OM6PSzOyAkGeK3+2ytj+zQMHdDvh3I70dngLHfjuJ3k7lOVO+nczvZ

3c7e4Au0XZLtl2eEFdquzXbrsN2m7Ldtux3a7s92uNfdtgAPaHsj3lAY9ie1PZnt3y3xkB50Hmf+vFIPwfMPUCRwbJTBEOyR21gaiIgNy5DmR85jyYT6Hamzb9lKR/blhf2mDL2jWZAoxtrok0poL6FqE2HbDdh+wzYNOHpvkBTZ6aVjvE9yCJOthIQFJ00kZBpOjA9Np2dJ3L6KjZJSnH6OHrfOR7IVAUGPetNiuiLabvaemwHKZvoApbMtuWwr

dqBK2VbIINWxra1uLo+by5HJ6PDyewAknhTvYcU9Kf02CoQtlYxeqmlL8KLWx+9JgESBuhudf4GkGZeKaTA2A0EXACkAlBGBNApNZiJyR1t6ni2+tialcExYSGl9DUM2xbbtNW21Vfi+a/8codyXUtND526CbMNu2h5yAja97bm6wm79hlgO8dZ4ch2w7EdqO+OBjtx2piojlO2nYzvggs7OdvO3I+LtSRS7TKZR9Xdrv13G7zd1u+3c7u4Bu7L1

mRPo8MfD3sAo98e5PeYDT2hAs9qxy0udCq7Qr6usTQDbOC2wn4JZoWAritYizwpdqYMB8aqRYHT76N3k3h3wMCnNMKEjq41YgAghsAb4fACkBrDlWlZqfUJ71mKMqnf7EtiQKa/NeWujM4Ga+5ytmjXBan3Vxmkg5qeoP2eyQBuURG54XYTsk1gXjJKf4kOAXNtoFyAM0NLWqHYLzDRC/Wte2Gi9Oph1CYDOmT9rSLg0EZdRe8OMXAj7F0I9xcJ2

k7BLyR8S+kdkvC7FLql+Xcru0u1HDLzR8y50fsv24nLwe9y95dmOBXFjppRAdFeL3gOXMt+VdP5jvh5SSRuK9hiFmxWAFSVy1gtEWTpWtXefHV5bpubNn37WuT+5LWVOoLf79fCo8HioULHb3dT/G4lgr5EPX3FU1ow0/aOfmF1ws9pz0cT2fBunMifZ4c8wDHPTnyYc55c+ue1Y7nsFuRTe6qOC2xpJF1Y0wa2epzNjc0tfkZD/CFW6LLlYgA0A

GozFpgjkfO3RW1uFcx9205wK88n2ldBJr1Y2/g48Y/PfnW8R0+quJ2SVt9Dt8nXvszd0PvTDDmFz9JE8MDWHSkorWzvhM2GBy5b/h1i5xciO634jwl1I9JeyOW3Cj6lx29Uf0uNHTL7R6y90ehrB3RjnlyY75fmOhXljwTTOFcK2O/x1ZVHfSc/ANl1k8r7NYAtSOodobmHPd12YPe5GTeEg9uJoFaDvplA0wVcHnEYn/ahgL9iqyE9PdhPz3zBy

929vqvqnjXkX6L7F7s8POvXzzuj92TeeiEL0IvcdsG+Y+C86afMfcKe62g8LsdvMWN8Q54VqGk3dtkF/x/dMhKhP8Q+hxuJSHbWWW34qT8ez9vWHTxaLvh5i8EfCO8XannhBI6JckuZH+dnT5S8UeaYaXBn9R4y60csu2XBJ3u4QH7tDvjHpj/l4K+FcOfpwrhadz+I11zadgZ2ZWNH2/lb2XIfMIIb9/ClH2iIB4OWDDf3cBPtNCN496l6uDpe1

ZawjYQU52FLPMQBwo4QgBOFnDOzdHTBegAXOvKGgRF7Gz2axWoAifokNc3jd3LFSX31XxTu+9JttGPzPRr85nh/M03/3dNkzozZkS4f8PswN0ER5I/0AyP+ACj/B6L0SACfeyinyZCWPrPUPmz9Y/oty9r8eA5YJLBwEmCkBqgeoVwkYGdBwBv0DQRQvoB4Cbpive+aj/TY3j0f/XwlJj9xS+fwdxkHHy2+MO6/KSuufHtDY7cE8WlIX/p6Fzlrz

c7X4X1q328Gc4dHWeEf4ZwMoAOCtBv08YB2J4WghghywPAXAIoxkH1uNvTb7T/I9296eVHdLo7z25M9nfvLF3q75Z5Hd3fx3n15pSqlH5EawrNVrpRzD5lPJeY3n7e94IglFUe20vXbRle1eQ/+TsJK+9YuFOWbwQUYGEHaBSDAtrXNu5WXa/CdKnMvdanH3/bV/3qF/S/lfyJvavMS7f5Xhj8skDdP9avyKYSXcFEljWJJ0b89IQ/p+NsuvTp73

xEJTdU/+v1Dhm6B+WblC6ieJhrC7ZuLDtfpICUfhw6zes8vH6J+yfqn4HA6fpn7Z+ufpb6aY63pp5be5Lrp7tuZfl25GeJ3n27neejpd4GO13lZ63etng97ZmRgOiTOe3MiwJxGH4GdjLuCrhISOoiVoljLAaRvhCb2J9n45n2ORvDYpeG/ml72u1ViUbZeGCoVJ3uONgLYTqNPs+7VOT/Iz5gqrso2qU2nRt7LdG3aNz4AWyKugAa+Wvjr56+KQ

Ab5G+Jvmb4W+kvvzYK+KHtopUqYXDSrYe96spARg4IG6DjgdoMaAQsTYC+qkA9QM3pQA0wFSZUe9xrrY+u9wCFpn4Mqix7lA2OvdgA+Cktx6b6JOvbZ++Anh6ZDeOGnC45uofhAGgBkntAEYCVhuzqnixwM4DPeUYMQCnARCCCDgycVL3B+gxdhwA2O6ng26bezbsX5tuSjvp7l+3bsZ6neZnn4YWew7tZ6ju93vZ4MBTYC94d++Zp1hHwcWs1Dn

ADZLMCcBPnm4giwUvMP7g+wXpP74GEgba5SBW/m8yOucgc67oAd9hRhNgpwBwBDUZ/gDosSetnsFvOREGFqKE4yOzQ9KswN9w+OyQaqSrA10hkG22WQX145BA3gpbABwnoUEh+Z+uN7TcY8tJ636U8qGZcOgdgd7DBJAb26me/blUCTBN3jZ5juRXs36TuKqMHB/WLnp1hbBP2Clb9+LkLbBuOyrrazykJVIWiFqe2hP58mJwTa4IKm/hl6RO+gm

sr0c0vqgAYq2yqxyQgMIMQCoAygC6gRg3kMEDEAEYMwCHCiAAAAU85qgAyhqUKQDyhcAMCB7CWQOqFlQk5vqFyhA+PSAXGcFHhZ7KZIEpARgIIOqGe64QAACU0cO7S6hgQFAAiAIIlaGGhOofua9mWocaF2AcEOaFGQ2gPoBahXoQADUNocEDRhCALGHxhxbCZBehCgMmF2hZUOmEehk5tcLhhJoVGFwU2gBwAZhSYSqF5hMYZWFehzgFmGoAOYT

WGphFYfGFFhkoTxBQALoW6HmAnoaPTXCrUN7oShUoXqEicBofKGKhPYTWFqhGoRj4hhm5kGFGhpYWaH2huocuG5hqYQ6Fdhzoa6HMA7ocwBehwAD6GhhfoQGHjhXnJOGLhm5qgAlhkYWuH5hcYYmFbh5Yc+GZh8QNmGvhT4R2G6hxYRGGmh+gG2H1hqANWG2hwERmGNhn4c2HfhdYb+Ghh1wk6FqAvYQeH9hR4YOGoAw4QHr2yr5uCo6BHRiIpLq

AUP+YtSgxhAypsqKgh6jhUQM8qbh04cqG2hc4ZqEIAN4dcKbhAEWWEWhG4ROHWhrYeuGhhi5khE9h+4YeHHhp4ZubnhpAIGE8RwYX+F3hHEY+Exh74WBEphb4ZBFNhLYeBFqRhYXJH3hgERBEvhfEfmEgRUEV+FGRcETpEIRu4chEiRaER6EYRWEZooJyItmh7b+GHteo7OHgZZrJgTYMwDYA36BGDGgPYccDxgGEMmB6+TYBGAcWuZlEEtipXvb

4vGOoG8aDW3FEkTwaYlivoSWcOi/zpBgLj/4yWUIWeLoan2LgDjAdFlAhB+zDmAFIhYfhN6ohFQTZDGoVQbPK0WGXMwD6AxAKGyYAx/DyA+A0wDaC+wc4JTjfo44MS5RglTJgDOUtQLZqOgQgOWA/ATYNMARgTKNgC1A7QNBBugkgMmB2g8+N+iOgEYOMBrgUkCkD6Ah8OMEculAVy4khMwU37BGLfvoi4AiwZK7hWnWGMIbY8pGsibBeEBBLJWH

4Au5j+QXvHx8hVuvq5+as/vF73oIIIyDlgzllGD9wyaMl4ChCymUgrAr4HyQOuWXh1Q3BJrtDGwx8MaA438dHlcD5oLzDYgeI6BgaYmQj2Pshw6/VrcgyqxqIcBXAlwE44OwuoPtQm2t2NNZNGWpJmq5RibvlG9eQTAAHpuuhvkGH6BhuH5FB+ksiGWqU3oi5oATUXJ6nirUdBDtRnURwDdR+gL1H4A/UcaCDRTKCNFjRE0VNEzRc0QtFLRK0WtE

bRW0TtGJAe0QdFHRJ0WdGEhEgMSE0BpIbMETuc9iqhiO7fs9Gd+HQmsijIS1H67ruIfLMDvc7joAq6g6yKLC6goUr47Fq/jsDFHuwTpIF+8nguMKXBmMdE7ih6AO5zMcbutSEk+DHKBAecxcfeYmQQejzGz6seKCouyAihTaGcLToREGBeeEYGkRgFugA+RfkQFFBREYCFFhREUVFE0gMUZM5DSAnIxwVxCPE4FaKFKq5ESM4tv/b3qmgHsDjg/U

VJB+gygCkCsQ44O0A8uLoJgDGgTeABo2+MQS85xBE1AaiZRaGLFoL6EhmlFQaGUTPp3xlGDlE/G4IT16QhwsdCGABYsXCHDeEnh7bGGNUSUHB+yTFJ4NR3AErGHWCJp8Cqx6sV1E9RfUQNEz4hsaNHfo40fDKmxC9ubGw8lsZTirR60ZtHbRu0ftGHRbeM7E74rsegDuxDfnQFzBQVpoDiu34ksF2OnWFtQ000vEyYruosCahshgCs1AVIlwEljQ

a6mmKHHBIMdP6CmJXmhL3obALgCWQtQPEAUga/ojaw+YwhsEyBVwVjErxlmoonKJqia04lY4MWA6X+AlnahCkj/CIboYiQbJxSGEbrIav+VyNzH46vMbHhe+ICSpJaGy1joacYa1vCGQB1UfDjgJVUWUF7W7DoKywJmIbH6aYiCR1HIJ2sagn6x6CcNGYJ2CZNEpA00XgnzRBCctFEJ1saQl2xDsZQnHRp0TQnkB5npdHUBDCWSH0BzCU9Hk0c7n

axxGj+GD6g2r4FHGCJ7ZHXKjIeEF1bCBycaIEhe4gUjH6acRlonZxF7rv5XuOUh3iLGigcXyLJj7qoGP0M1tFr1x9TnhHdmugW3G/mhgZ048+SKinpni68ZvHbxu8VAD7xh8ZDInxDgfe5IeryMsZK+rgTmLuBd6pZqTAhAPGCSAdlIkAggf4DwB/gNFKQa4AxoNBDOATYM8FW+oRNEHxRV8exJ2o7Yk76HYT8XxRNys1GCF5R3iT77ZBRUf755B

gCQUHBJo3u7acsI3pN7lBCsagAxJyLliHHWCSRrFaxOsXrEGx6ScbE4J2SWbF5Ji0QUk8IxCTbFkJ9sRQlOxFSedEDuNSfX7TBjfuSF3RlIfogIsNISwEfe6HNBwNyTITAngSvATAm7gKWOriHBQMbq55G4yYUaTJWcRjGzJ1wfon3oY4DwDxgzgD0wpAwQPGDxgXcMaCuE/cBGDxA/cBzKwpI+nFHmJ3xhADLQtwEbbopyRC/G2J2UVOReJ5Dni

mFRpRDCHguxKRLGUpX0mJ4EawCfVE0pdKSW4ouPCPrEggxijwBQpdoNCDb88QFlCjgNIJIDTAnvDwhGxWCSbHcpuSRbH8pmmIKnFJ5CY7FUJ4qbQkQA9CTKmMJ3sSK4qoV1swEtJPKtg6YG3SVwGoAPgjqloAXiGsj3YgAoF4iBvIcalheM/ol4Qx7cKQC9IrQNUB+grQPQDqJMPhnFTJlqa9p6JB/pZqHpQ0CelnpBMbR6+ulMV8G9W21GKQyqw

1rKTIp4khNZNy0+jcjuJTyHzGfxOKfGm/+i1v/5/xosQEm0OQCQiEhJV4LLHGS8sUW6Kx8AY/pFpJaWWkVpj8NWm1p9aRgmcpWSTkmzRvKYQkCpRSbbE9pZSdQkSpRIVKlTBtAfUlMJoRjpbsJtIcCp3IqwDTQ/YmwYshDKR8O+BrIQgYIKAxmmtumzC6cWcGZx2ENMk7+t6XnF4+q5MoH3KWTpjZVxohI0ZgZMeLhHaBuyQRHU2BogB7lAQHu3B

2pDqU6kupbqeOAepXqT6l+pevJuoKBTkcLYTS6Hir4fJFmvehQITYHlwccf4IQDjAkgH+Djg5YAcDlgUkMwCAse3p67W+8KUGm7SIPuGmQaGKZzHwcwaXGkMYwLr/EEpuQYN6ppylpLETeGacUHieKGREk+2rOtjh5p2AsdaRRc0DSDxgiQP3DfoFAE5rzRyYABBnGZSGRnNpXKZRn4JfKVbEkJ9GSKm9p5SS7FVJEwaxnXRsqQ0lcZrCUvazuK9

lcjNs5wJtoM0JukMp7g1wGaaJxQyRpoHaUPqcGChWuNek6JucV8w2p7cHvyReKQA0Cjg/6kKYIpwacgQ4OiQRzxFUSWPAYiSLiWgAHg2KQLG4pBWR3LFZsIWEpBJpQSAm+mYSfm5QBMJlhm0pOGYHZdpU2aUlipc2TX4UBdfmxmext0YrrkmXGU0nL273rLDqpnFLwnzpZ2GkHs+PScZDPIiyIdniJPIRD6pxcmbKYaCyMRUhnA9wFUg3pUTubr5

xror2anmkIkOa3hsubeFKAYkNCAYQuoaeYsMw9DLly5muc2E5hrQGwAUA36LgAsqpALqGLmIIF2gRIUIlrla5CufEAq5qFpyKRiGudbk5hNIErlQAduUKL3CauVACXmVudcIm5rymbmp4ESFGIe5Yog7kW5yYgSKGyKFp7n8M4IJhaS59uQOY0izAInkIW/ooCLKA6eVuYLOKPvsLMROeaeY8gFIHADo+iAEXn25xoEwwIUS8BeDl5FxjuHMg0QO

ECx54eQOaTm8kL8JR5MolSLMA1ebrn65huZbKiQ3sLqHo+gQEvJZAMIDAAkUIQKQCvopylxCHCk+d+jT5pALPnBAFIIwor50Muvmb58+YvkXgyAMgAggUsFpC6hXedUAYQO0KQBEKY+aGFX5N+UwCEMD+Vhb25tQEwD4iAwH6De56kDKL10J+Y+Cf5pAN/kX5oYaeYgFYBU0B/5okAAX20QBWwBQFhUE3iZOLou/lx50uVbR+5m5grmu5iie7kQF

9ud7kTmOBfLk65euQblG5AeXspB5/kBbnYFZBb2Y25YefCKh0TuZrl4FbuawXgi3ub7l+5NBYcrm5X0KHlEFmBUmJ4i0echY8FHIt7kJ5bee6JIW1It6Jp5ChYyL/CWeZXlx5eeUU5o+heWoUciJeYekN5WheHnV5XIPCynKJhU3lRAioe8IyF9wh3mX5qUEJA4ikhb3kwg/eRwCD5VBSPn7a4+bvlT5+gDPlz5FIEfmcAy+Z9BBFIRVvmkAO+VE

Vr5wRRvmhFC+XACnKJ+WfmcA4BZuZP56gEwD352sM4WcQ1+XkWkAr+YUViF4ecgU/5sBagDwFhUIgXVF2RUnlx5TRTAVD0eynAUdFjRV/koFumSTZaBTcfhHfuVNourtxxICAxJ6JyT07vMlEVL74+LReHlYFTBdcJcFBBQ4X8MpBSsXa5qAD4XD5xuQJGB5whWbSMFKxSwWVFbBY7mnFcuWsXK5FxbwUdF/BVbmCFdBc5Ah5BhYmK90PeTbIfF8

eaYWKFnosoVvC/xeoXMimhb8U6FqPlDH6FGxUYVl5MJfcUci5hRVB15nANYWCFzeXYW/FThY/kuF3ee4WEiXhXsVG5OUBUWbmE+XvlJFB+WEVpFF4JEWr5++SkXxFDJVSUpF4RRwAZF5+Z3l4lJRbfkFFBuEUVCQvJS/mklApYiX3CTRb/kdF/+d0XIAwBb0UDAvxW0W1F9RVAA9FoBX0XEWLgdXpuBWHp8l7OPIDCyh20EFgFJZe6alnosWKYkH

HgCQI4yvgiyPNBa4QIZADWmYOWQ75ZBUYVlJp/8YhnixZWemmn6GlhSnZp8SmiG2qjWaW6BUXGm9YuSQRqTlfWLSqgUzub3rSbd+UqnqDuUc6SHzNQSrhu7PuNbKjppqScWdkNmF9pdn85ZGJFavgCPusJ+gMgBiCSAESAoDgg1gPQChAzZVGBSQdoH6DLS2gG4AEAJAJk6xhxAHCAAAeqQDOgLmq0DOgTQDyBY+qmWLnqZgALwbgALM7a9OsLFx

kIIEAnC6kKea+wUACoUn5ioRugn56gMk5QlJ+YXGr4CPFuUIARtHJGQlBeRj5W0cJQ3lW0yJbXlWFzEVbSQi19A7Sdh/InwU/lEhVKJSFpAH+XoiVkRSDMiJ+a+ip5BkPoCL0fAIhaAljdEsl9qeDLWX1lzIk2UtlHAG2URAC/l2U9ljIH2U4AA5Z4AUAw5WOUTlU5TOU8gAAD6blMIJj75gu5fbn7lh5cgDHl+gKeXI+uhSCCXl08UXE3lLFfeV

WRj5cU7flqAK+XSVH5ZYX150lb+WbFAFaHR/lUYhHlfQbhaBUeF4FfHmdh0FZICwVTdioUIVSFVbQDmaFasmVOkOioGDF5NsMWs+P7m05ER/QJMWAevPhzCzF7ma5yrl65QUDMV25WxWiQe5YVBcVPFXxXnlstEJXlxIlbgC3l4lbLmSVehc+UyVpeW+WoA8laiUcAGVcpUkFqlZGLqVwFV8UElVIhBUGV/wsZXwVhAIhVW0yFZZXHqazs4ELxyv

svH3pkgqcBwA5vjnKRB/qXwaExCUUKpLA/SfYnHwRqI/COIHJmdhNyjvvzHulEvAmlelxUQH6w5yGaSkI55KTEo1ZVKajlRJDWRjnQ+g2tGWMysZRmZkmCZSqh1gk6ZtlXSDqERBkYDZPdisheZejDLI+EGImGpMmYe485cpgspuK92HwIxWETlUBYVUQDhVfQzZa2XtlRFd2W9l/ZR4BDlHUWOWvE7QAeXOgboPQDzlouTybLla5ZhUFAQUUoV+

gO5aJCcVbwqeVcYukKAWRViztFXIAtQJPk+QQEOBbBAhNYCWJVt4WTXLgJ+QblAgFTA3lG0gVQgAn5DeRkXfoVtELUi1zERkWjgjtAVUMFKlWJhbmktcgDrCTCjoUG5cAMTWFgQhcHlaVnYcKV35VtJKXe5VlT2raZWCiXSg1DZbhVQ1hFZ2Ww1pFfDWDllANRWjlKNWjUY1DFWzWEi2tepBc18gMgD/4VNbxVB1/FReX01jNXgDM1toT7VUiHNZ

uYB1PNRhBZQmAALUq1otafni1qABnXS1p+bLXy1+tYrVSEytaJWBAJ+dbXg1uQJDX4V0NQ7UkVZFe4Au1VFUjWjljoFAAOU44EYBugygAxXq1/FZrXa1VtK8UcADBXJGG1luSbUdFZtSoE2VAxY3EOVJmSMV6Bf7sRHuVlmZ5WdKcxS6J+V+NXHUwgftaTVhV5NUHWU1MIKHVnltNU0gn5DNVuBM1W4CzVEwOnISIJ1vZknXIAvNanXp1ZdcLXIA

mdcWkS1P9VLUY+MtXLVyReVR0WW5JdaeYq1atbgoa1RwkPW619BfrXj1z+UbVLmCpQgBSl9dDPWqGLydqVkWupZ5H6l7cPQAUAJTmvF+g72XIkDViKQJYLaRthcBNQQpMtSvE9cqybZZy6W6UJK0GZ6VQ5yaUAGrVJKfDmVZYCdVnrVOaWjkRlBaZ2l0ZwqTjl9peOa/Yy6kqYTlLZI6RSE+x+iLmDKpLSWRgGNwaHTnZliuID4eOpMfdWHwn1ed

mzKpqQ8zmpSmTWUFAdZWDWNlENXhUEVHZcRVw15FQjWu1rdR7X4A6NZjWnCC5TjXLku9VbUE1z9VSJNAJNRgzH13NafWkgIdTTX5519ZHV310dQ/Wx1MTTCCv11wu/Wf1/NcxGC1QDX/V51IIKOCAN8VSxXANiAJyVgNVkYBWPFVtHMCl1tTeXWq10TUoUINcAHE061I9e8VoNpRRpXKl09cepoFy5P5UuNNte4121XjY7WN1FFYjUjl7teMCo1Q

TV7X71ZRfE3v1wdefVpNAlTfVR1HWI/U7NBTQk0HlJ9cU1p1pTbnUgN+dTU23l9Tb/Vn5TTbLktNy9G01W0MDeU2V1bjdXUeNddd41O1vjc3Vu1DQK0AOUMAPGB+Y3tXk2kAfTQM3D1xxT7kG16DWM1YN7Rbg3HqFTlOqrJ9lY057JZmWIrr1ReJvUbqUzr5V41UTTs0DN/tYk2B1BzdTVh1UVRk231jgNk3EA5zYi2XNRTSnUlNGPmU2dNv9f/X

VNOdeU3/1DQB823hXzfbQaV7TX82itFdT02AlyLScKotetbkCiFsuYbVYtGpQMA4t9tHg0nqivoQ1i2xDdjEzEFvjyDGgjIK5YvB3rpfFfZSwAyajV98BUj3YoyIMkulqpLNWQZ4OXw1CxAjT6WvSSlndQBl6lmw4SN8OVI17VjUQdW0Zk2Qo2ipSjZUn451Seo0exN0XKnxl90bTA0ga2cmVSueqAdD2luoJ9Gg2zUFmVtkiWIoRHAhqGtrFlki

dznlqGiVekWp1Voj6zNVdVAA11njTDUN1ztZRVu1PIOiSkAkwDyBTAWNaKFqZETbS1e06wslXFpCPHs1MtR5UmhHNEdcXG1AhAIgDHoxAJc3KtLzRU2pVOzZbkntdTd01MKF7Zq1yR9Lb8325sDQUC3tiLSi2aVZtN8VlVckXIVPtceS+27tfujAAnCprVM1myUTb22At/bcC321oLcs1+NLdWs3jt1QJO3TtkwAxUrtxceu3XNSTRFVstV9ZiA8

17uvu1BA4UEe1yRV7V00ZVF7f+3h5L7VB221tdXB1LNI7as1jl7dZ3Xd1vdW+1E197VZGPtHTae0AtTHYO311PjU3WjtrdetBCABTOCB9Qfdbgr0tmrZ+0O037TCCdhf7UJ3XtInfM3MdizcO3gtUnWs1QsQgKb7/oUYExW+6nuiB35gprfi3FS89WTbEtpmWMUHJblRIrTF5EclTb1C7f5VYda7cFVXN4VVu0Ed6TUR0f1JHQe3kdx7c+1StclY

i2XtcXSq03tSnYl38dsuYJ1Udv9XA1ZAynTrUSipVRp2/tHRXwzZdqrUB02doHY1UENLVW8nTSepf5ntwQgFJCnA36OMB+g/cG35gx5pYTGiW7Er2zfBr4G4LBgC0HsDLAjclw3NypDrw0elIbdoZO2pWZG3AJYjUGVbVkjaGXQJGIfSlxJNGMaCSAmgAgDjgUYAgB2gbCn+DVAdoGRivokgANTMZvlmmYkmK2ZAY0gFORtlU54HOsieIL8NsHb2

B0Dwp72biKsB9+v3K1wbpwyVunfV7bZekY8h8CmpFUD1bdlWpYoepljhi5jioEWPyioqVN5yjmGyVjzVU3O0OxXBlygvamyA0RmKuj0L0S5niokq8RQ0351MEfj0M9hPcT39FRmUMVL1TlaMW/urlRMWeda6jMUURPlY8oU92ylT0HKmPYSqRFLPbj1pVxhTj1E9NuXPHOR3mW5G+ZjXfXoQA8QOPYNAUYOWASmr6UFrOA/XQw1JBbPOryVsqQNz

CKw9bFMB4OwIWFxuMeWQtWQ5C3StXYaaaSt2BlMbVmnbV8bbAGyecCfJ6qG+3Yd3Hdp3ed2Xd13XAC3dydgOkxlEanGWZmSukFZD6/sc0k3VC6UkCvgjXsY1/d8buHF1tshO9GPI2utY2llgTodW85empbh8y+4EKQo2nfmjb7u6mWvRwlkILiCdFl4foAC1ARQT3foTPelWVN4tf30s9RPnj3D9BPQ0CTmUve7Qu0qxTmFG0xEG9QmQjtCOFwMJ

dB31QQ3faxx99oYf/WD9k/Qr0D9k5tK1D9J/eP2z9tPQCrz9RPTsXL9dVVbTxA6/dhHAqHPYvX+sJLW52c+a9QL1kRXlcL3UtTal7Tb9XfepB79pTWP1vNR/fL3wlp/dAOclF/fANX9uoXP0cAC/Q/0r9yFS/0q9XmaLY+ZbVZRbGutQFGBQA7QMQBNALdkb2PGxbKb2JRvAPQMW9OyJKq2ln4HhD8wK1L60waN8Llnf+EOfw3u9RKcI1e921at2

+9ICZSkB99WQdaxJ8Ca8hh9R3Sd1ndWcNH3HAN3Xd0J9J1Un1nVgVqEbKAb3SmUbMVyKpqVs9sJqn7Qz1TsHh4OzIeDoGFfVlZV95ZfQbaCipjnHI987Rspi9ZPjipVi6VWVDqQQLIekn5r5WVBG0hlVVWmVNVVACO0J+W2W+ACAJM1k9UoN4MS97yn4PGFAQ6JBBDcACEP+DRkOEOVVyAHBVRDiFbEPIA8Q5VB4t9RjhGEtC9S53L1+yb/0edxg

SnreVwA3gipDryr4OhDRkIEOl5eQ5kMFDEQ8UMmVbwmZXlDlQ4kN4DGzvV3bO2MaICdl2IPGBNMTraV5MDIaW63Bp7bBJmHAy1E2xywuDs6U8DDUHwNfxgsT/GhtCGeG0u23vdG0wBfvRt26WYZdH5JtmmOCkHdSg5H2qDV3eoOx9mg/Nmy6flmAacZkBgugtCmfR91Pw3bEY2PV3A4D31tdyHajpUmrpulc5smdD3yZ8pq4NN9sgSj3oqXQ9irU

9IIDoXMA6kGAO5AGRZ32vKAAFSHKvQ9MPoV5PVso+DxI6SPkjpedSNqlp+VyOoAdIxkNl5ZUNUNPu7/XUPOdX7tz0r1fPSupdxJge0OTxXgyyNpDtBeyOiQFI9yMggvI/yMMjNXea11dOpe8ma93VAEGvE9AOOCjgJPYa7n+YwBsPLQFwGFqZxew3cCg5G2JdjHDKQTw3SewbZcNCDJWSIP+ldw4PJVZjw3G2bdNKQZb5pDKTwgfD4fcoNR9vwxo

Px9gI0SaPd/ls90tKkgMW2vepbd34Oo+qEliPVu4BDYvMs1AdAAxaI0cFttQTjX0tmVam4MzJYTSMmo9hI28q0F1zCAVuFSoKuZJDFtSkNKj3Q8SPtj+RbiJdjlPsKNrJm0B/0NDko00MGiJEVMWC93nVRy+dio88rKjhykON35I48uDdjMw68kGjDXSQ1NdVQE3pCO1QK4R2gujX1WoSfXW4x2jU1ClGVy92I/z8Bz3AoQO9frWx6ejGqhQ5LVh

KX6Oe9AY2IM+9Dw5IMhlzw1t3kacgyH1fAigxH0qDF3QmP/DSY5m2vW2g+mYBWqfaEaSAhgzmN5oosGRiiw5Zr95IYTISq6H2XZJ+DljEPeiNQ91Y79UuDbZiLlzti5dM3bq+gCQUylgBcgAEgUohwC+FpAGwzNquCnwVcTCBcgDElMID2Nbqwk1kCcTXRdxO8T+gPxP7FQk17RMKokwpPiTkk4JPs9Yo8z7NxjQ6S3wq84x5VedgAz50i9GFbJM

cTkDWJMNFPE1CB8TAk2pNq0Gk48X2T3IzpO6jzVeepzDmHkeNa9pwM6AUADQH+AZ2XkmsPFcRwGlkrARtmWwcDqavKS7w2qax5DsAbV8D8D3o7x74p3pdcMGqIARAlkpmaWBP+9YY2jkRjTWdGNwTcYz8Mx9cffd11KqYyCOjpj3pYDXVH3Yqo00SsFY3VtYPWY2AKB4LajLASBuD0lljgxdl2Ni1O5R7BQNdv51IPbdhXQdA7SC2sdRnex2jltQ

OOD2Y1QP02zt7VJ4PpokTV7Q7NfTUQostodesJ7tUAMyJ2g4UBSDMgWUF1XkdfTScKXN1wqqU31agLdP3TpAALXitVtIf2ytsuSSP8V9hVZHXCIIJuOdjy4GB3JDspngynTA9Yg27lZ9ay26dQLQs1DtEnSs3+NazVtM7T/TQxXXTP04qAPTfNc9OKgPLcjNwAb03JEfTspSTOSAd02TN/TpTQDMy9MA8DO3hoM8k7gzmuVDPDkHY9uPMA9nTUOi

j1PkS0SjaeM5VM5q9S0OyjbQ0AMKjR04u1q0SM8k6D1qMyk2HN3TUzMsz8+Y9P7tVkFTOvT+YO9N1FjM99PMzv0/9M49gMyP3czm5rzOFO/M3LmCznrMLNiAsMz5PzxfkwePzDD2USEd1QGNx00DbwbNAxT6LMahG2D42lOm234zx4f4qGkVmCNACf6PLdwE/cPn6ntqGMQT4YzH7yDofZ8PwT8Y/VMAjqE0CPNTt8q1MMBE8B1OplL4PwmoY1Zd

W08BA0+2SM0bxGDoOD59k4NTTVVg7oSAGMzB1Yz4nWC2SdG0+0C6oIIHZRAs+07VZyB17hIDHTbk/A00zRCpR1ozl0xsLmkbAGED6zv0301mVb0xrOFOg9XDO9jCMzZNnT6kFvM6z6M841LTonatOGdk83jNjl08wgCzzcAECyYde8wfPWzBsxSDHz0Q6fOItZ02LMijk4/pOfuLPjLM89LleMUyjC4wANb1VkyvNqzZ5voC3zokPfPvQqTd01SQ

ACwgCHzrM6AuIV4C700bzprU1V+zIyUvFWtQc8rRXOo4GwDyd4RteNGuEc3QOutV4PKRhaewQ4KSEnJuNZ7AwGSilzVs3a72CDfiYt0ZzNOsjmoZoE8GVlT+cxVOFzMEzGNfDCE2oOJjjU/4bVzH1vKnaNtMI60Z9lOY3M6g9vfbCHwNNHCPjCCI+jBnYx4DbBhxUmRWNGpdE9X0MThRsjZONjHXp1id8HWx0fzo5c6DRg9AGvkVarhA0CTAUkK4

TxADoIQB+g0EEtELzqyodNVAq89gsXt2swQu6zl9eF2CV3TSVBccMoki25NShXTMQzOdYK13NwrYf0OzBPQXUCitS8pX0z2C/lWdLUhJ0v91ms4g1F0G/dfPqTaXUTX5LzAIQtFLxzd02BLmM/p3YzE87jNIdY5REtRgUSwgAxLcSwktJLPACktpLEYAxVlLBIpUus1iLTUua5tzXbOn9nM6A1hinzVyJtLmue5P10UIj0udh1wv0vnzgy6PR6Tk

s/UPSzv9Igtyz0owiqKzQvZZMdDmC/2p5diXRMtTL4dXTXrCxyxUu8t1S+bOdLVy+zMj9zS7L1y1Ty3LkdLtSy8vYMHy2Jh9L68wMta1Qy1qX6jRDYaOBT3VFC0wtcLTQ1mJfXXwvuIGU2/zLA2gJ942wNjOvbiLU3RlMu9M4j6NyLHvVTpAT61eIMqL63XnNQJBc28N7dJc7VOIT5cyhMqNPlk1MjaaY6CMtK44FmM8ZKqZoK3AAaMRNF9zIdbB

DKewCLCtsINi215xUiWnE1jSNjiMBLz80EuvzOM4h1u1BM6aC7TAzdj5zJLutCuIzECzTOEMF06q1kLhsxTMmzD9TTMXLcuZ9P01QC7bPYr0/bitvNBdZ0suzOwm7Mgz0MyLOXzMkydORrVK9GvbzqrXMujzCy+PMIdELa3UBrxAEGvEzGa6zNGzlM0mtUrKa7Llprcaw9NZrV/bcv51Ts72YFrZoGSsezgtF7NKgUCxON2sU44CvQqwK7Jr6B7n

fz2tDEK8uMYL6ADktnzOwprXVrD8zvNDrf0wmsvTyaxiu1Lg652uGzI6280z9Y61U0TrkM6SMzrJa97Oizvs6r0ED6vUQO7O7cDJ1ydZVlFMcrIWrsCCLDcifCKqUDuzEfjJwzsBnDUGXN0SrabkCYppCiypYVZIEznNcsTw0qsaLKqwoNqr3wxqt/DDU1oPhqGE+mMqo44LhMvRv8orAfgswIj0kTQwrmXWDO4AeA1t+EBznj+tE6F4/VfOYxNp

S3bSPMrTLHW/PLLbtWssbLWy/EuJLyS6kvpLoTdjVNjfnexP0t8K4UuIrGTcisIA5SyAtVLgJf2u3hWK40v2zr6zK33LcrWpUErsuSSsx0ZK3MCdLR60vJHCK9MMswr+gLpshVNa2F0zLUm7B0Gdvq82trNCm9EtQAsS8pu7L+y0tFHLxmyctor5m7euXL9S9cuy9Oa4032bm5vyIUrck602dL7m7UuebJ64utz1K6/AtArUo8gtgrqC93Hyj8xj

S1+bAWyLZnr27UisFAKK6ZtnL6KxbNWbiAEbQcz0rfiudLhW8SsiTJW7UtlbmuRVvebtC7V3+z9K4ePYxggB1lx2cAJFNcL1o5HN3jUpJnGJB0+kNMB8edvwHrIwGc71ZT6GzlOJpy1cIOATmc7Kv4b6GTpbEbCbcW5VT7wzVMUbei8hMGLifXRsGrKqBM4Qjli8YMHwkcXsC7Av3cyFb+Ti4rGeIGyKsCCb0mTY38h6/gsIerSPY2PauzYyyM4q

i5nCUNAUoqSOD9eC6GGjwJePgAn5rAIhURg9ETkPKhYMxGD6ARtMz0IAjtPGGj0Y4cTuvKpO+TtgzcvgKNh4yA2Ts4LYM9+h873gwLt7KOhY/WU7cA4f0S7wu3zMy77tPzvU9i5oru2hcvlztIDx/XACS7pIw0DSTBI0Ts67gu6Xmm70u3fPU7LqGFB07yAAzs9hzO6Xms7fM+zuc7U/YgA879kVrty71u3spC7Uu3zOi7DI2rvh7rs5rtMMVuwc

q67/FUrscjl/TAPR7FO7LsJ77yknvJOj9Qbt+7z6xnsi7449VuwLOyV/2udvPQ1umTG9eZPoLUK32PPK8u3AN27Guw7ubmNO87v070Q0ztKhLO9Mtp5HO1zsB7We83sh7re+ruuzke/kP3Cxu23ux7Y+5iot7eu8EDK7hu1nXF7Gu0vvbKK+8nv67qeygNF78+1PuFr5u3uPGQ2rowsMrG23+CYAZA5gCtAlHntuvBG8LaNOKgix+DHYZGLchxaj

pdNVTdEY2Kt3Ssi5hsrWQjc9uKLUsYiFrd5qkRvUpJG81GP62i6XN1TVGxXParQ2uhNPdoO/ojPsejVn1XAywCliOLzJgagQSWDvzBfc1E+NN9zk09jvYjTE3juabBO5bvj7ie68o20ioLUWr7CAIP2O0uyKgAAAZEIfq0++8EBE+Ah4kCoAAAPy8AqADqA77rY2T6rg+gNcxEKhENMAwRcFGwAnOmgGoDMA9DGbSTDBAFUNB72e3sqLmqh9cyEM

mh9odlQuhxGD6HeQEYc+5JhwkNKHLe1wd8HjLbh2B14dOzv9gWodYfDklubwffoge/HvsHOe5wfWACAHL7v1ARybPBHUojYdW0vBw0D2Rwy9rscHeyt4c8H4hz4dSHwh6IcZHqAMUdyHPAAocmQnhxPshHnrBofLA9h0ZCOHzh4YeQi7h2YdRHy+3UepHw5LYdNHOYTod6HBh64ex0cQ6YeJD5h9EeWHsR4qDK7iRwvSBHlYfUeC0YR4UcRHtR7k

eD0ioAkcbtyAEkdBHqx4bAaVGR1kdv9MC/8vijtW2uv1bW6ygtmTi4xZN7rje7EwWHZPvkfSl09BsflHghyIdiHeewfsVH8h4ofTHPR9sfHHCEI0daHQxw4cjHLhx0cTHHh2Ce77vR2of9Hq5oMeShcJ04ejHc9J0dTH3R6ifbH3hwsf7Hhxysd9HDR+kcbHkRzkcxHeR3Ed7HfhyfkUnKR+ieespx4UeZHf6/gOLxuikwvtV7cBQZ6MT8qODtKL

+860m9h24IQjVj4w4jBp2OpIuBt81eKv3bf49DnYbkB7hvTccqwRtSD5U19vbdkY7t1kbsY/9tIT1G8mOpmeqy1NaNY6fogNpEO+91WLt8GX0HQwPYWPcbxfUlH8r22ru6eLX1SJuYjbq7bq47Q88317+y803uU9A4wcpHKu6srtd79ID3uM7lRPvMXG5IIOpahYuxaFObr6xK3DHuJwifYiBJ+celxzI2uNxn6Q2orKASZ07spnru73vpnYQBGB

ZnrCjmcMj9mxzNFnbR64dO0SJ5VDln1PmXtXHBk45UILdx80Pbr4K0uO82Ks50P9jRI/Ge1n9Z7TupnM4SQttnu6p2ez73ZzZu9neJ6WeDnCAMOfPJeozybX7628wvoAxwEIDYuo4G0yWKkp+sMynt8Ob37YWGGdiC8nFNBw59UbjNWJzmQeqdXDWGxAfSrL26I1vbtUSiGGngfZUHKxs8igfqrAO1aeVzKY7ac1z9p21OkUDc1DufdP/ErCtznG

x2yiZv3F0l2ovc2IFllA8/4vMHLE+E0QdXtKScI8Deb4chdJ5cgCQDwrSV3zHmnUyfAiL/RbtMXatCxe4AbF0fUsn3FaF3cXI27xd8H/F7seCXVWwS1jncC4ZMzjxk3+bktDNvXtUtC55bXMXcR8XESXwXSfX4dsl3eXyXo/VZHeHM/e7RCXF+3SuWtN+zee0gttOWD9wA9kqkvn0U5ysOwHzq9QLufKxJra6L/tdtAXEISBe+jMOdqflZup9BdI

50B5AkIHRp1BM7dRc7BPkbui5acYHUZcAbYH+q7XNBW+AExuBxqVLsD3VUvAzTEXVqyq4HIBjewFUXoyTRcMHj2nRcRni0640vzMmxFvGdY5TwAUAHXfQDxgBwJgAZLLfcF641/laXouaOHRxeh1OQ8R3iX0Q7NcwA/LfsfDbd5WHsU7L5bbun7ZoHZu9mcke/VpryK5QX7F1eTpxb5jAMQBvTnx7g3XFm5tZesXzEXf2+b+NXWvSb4W0st+rrdY

NfDXo15gDEzwHfNfmXoXUteRdK14hVrXG11JdbXvu4ekL7ha9nU7XIu/iv+5VkadeyloW2PMhL602EuTA60XtH0AtQKYrGgILKnb1MhTNUBcWRyxddG5V10wA3XW4PddxHptU9fHXtl8ZevXGPu9cXHy6+XvGZle0ZM/9c4zpdWZDewZcHrWC1dOg3QXe/X4dkNyZerXwHXDdcVCN2jca7e10jcHXzAEdeY3sudjfcT510PmM3LaI1ohAt12zfcH

09ZzeG3t4WJcN5/N55mzDAcwFPYxxwMwCHOyk4QDPnPXZDxSnUc4IaBXM1FUh8rjjPNAUYggQBdTdoIXNYXD0V5KtPbEF1Ad4b2c+9u7WdWTJ4IXwfaeLIXFp5qtA7hV3acmLDp7TCmlErpCOunwPfbC3IuwBYNqktbaLJA9P2DTQFqtVx4s0TlYxiP0TYm34vhn39sPNPz3V96u9Xv15FtjlmAM3pNAzoHqAcAE11GfzJMt/5XFxoQ5QsK3+x0r

cDDUN2ZWXNXO6cXgNjy89dWRLm/+UO3BW45sn3zmzNvfNF972Zoi1xSV310fDLeFyRa15rkwiZa2xOQdXq/MvBLa0+/MrLo5dPeYAs9/PdWduAOvdQAYN3h0Q3O9yreIV+94XuH3zTcfdc3N98VuvL2BUfeFVb96fe33CrTg9oPvDE/fc3L909fv3wHZ/dIiKl4501bGl5OezjZLf/3Nbys61uqzq9wjzQPsD4HXb3wQ7vfRDyD2nuoPDy6GL4Pm

D7ZPYP999cL8i197eFn3GlfmeyPV9BI+O3pXZQ9WRH93Llf3vJ27drbgc0KdVApneZ2aAKzBBu0ewdwJZhp8U2NUNyohDhDQcEmVjpfGkV9/FJ3YB/4k3DlUUovFTwY6VPwHu1fBeyDGV1ot/bOV0Xc0bcukVfYXDAfoBlXywb/LWwnNFbCPVypxusvVl5HzDJT/As1curom7X2tmEm51e43Da/jdAPbtedrTAl2hwDXai96GvlG4a1E2VdZevE0

tPLmq/Vr3/g7VW6hCNwmeDq2dSkAY3+ZwjftNQz5ObqjMtbo8fXv96Pf/3PqxPf9Xo5VU81P12pA9rXbT9Z1l6nT9w/dPEdL09ZbpTf0+sKgz8M8HPfNQ0sjbYz9zOTP+ddM8C3tlY7LbJwtynii31e/ceNbjx2gv6XHD9kuy3AVVs9zXQXe0/rXvT7s+ZDPT6GF9Pq5201nPUL4c/Ct1zxM+cjO/VM+0Pej/3hX7Ap65dGPEgA0DxgUkJoCyC8Y

OyS+X4+uk/LQLi7+k2w81GJI/7gIcDnTdCbqqcgH83cncATqdzqcxMep5ncR+hWuiHpXJp5ldGApwI3p/guh9UDQtxYFGC1AxACjLTArhLheU4TYG2VZg5YJMDMAzoAcBSQBwCjLMAozjWALQxd7Rs4HxV6Eb56Vd5Dtd+w1RcA/B7c1aun4AiZk/gcyyE2wapeT1WM+L/d6UiWNIPjJpS0XV3M3zP4902tLP7dssT0AcADTL1PS88vcQAOS00Ch

A9wqJAblCPIm9hAb09YVLmwHTLk8A39yJeoA6b8m8WgI90G/1rAD7Jt/XazeG/4VUb8wCQPRb5m/6F2bzZ25vdD7UNqXFe68+aXYtyw87rc5xPG/PTT17RFv6kKm+4Ajb+bNZva1228YvFrYQOCnxA2vySAihP3B+8TQKS8B3/VSXJxzDA4qTfBjPGN2oxk3fHMNQu7yqfSLap8nO++qc2G0FTcOUVMbVJU6ouBP2dwK+VTpbjwgivYrxK9SvpAD

K9yvhAAq9KvPCCq9j2FAOq+av2r7q+/gBr3qCeUUT8CNYXZd21McAxqwHGJPB8MITcS9i9W1ruTOc69pYggdyFCbPd94vODluL6+o6zEwdOsTBb+WDkAygBbnsVceRe0n5DH9EARIrk6gAcfTHyIUsfZhe+3sfjH1x+4MJdLx+70Anz8JrXxlUoXCfnHwwzCXIA2rQSfWlVJ/gibH8gCqfuQNx/afPuep9IlQn1p8ifX0Lp8mfuQKnQhVH+cB2yf

gJfJ98fFn6XuqXdlQCs3HrkLLMbr8szOdNbco+w9wWynzx/mfDtAZ/3Cmn3p9mfCnzq0hfmVUZ/hfYn17R6fln1uYyfow3J/GfkX9nRzvzlwu84vS7/erZ2poCkBNg+4OHMUwpcuiyjdMqqRzf8gpJITcDSp24+J3177lOPbHLxG1p3CVxncwXcsalfBP325++aY37xMS/vxwNK+yv8r4q/iukAGB9qvGr1q86ver3B9GviH0Yt8a+bQqm0wHAAk

8cJeaLHEKqwJNW2SZGTzxvmobiiSwdJTq2Ln5PIZ74s+vayELn4f80z/axvYazGfbKkIPoA+AWAMQtnryhySPbzRCgOYZFP3wUugFfoEocffX35gAg/ky+fV/fMPyHWEMQP6fkI/59U0BKfi588qQ/wQND/bz8PwD9Rydnyj8A/EPwYBQ/qP6AX4/Z60j87mpAMD/bz6P38suf1x4w91bzDyZMS3lLZ34rjovSyPY/333j++DFP/oCA/tP/T9nr4

PyieoA/P7j+/fQvwz+E/hIuL+g/ELFl+rbLl9ee4v6AK4THAUkPECOg5YCCAeupib10lyFL263nvn5+rxX4lbErDvgniO6N6K6T8AeaqbL54/yLcV1G1Bj4jSGOPv0gznchPQrzBNDf4r0W1/vAHxN8gfmmDN8Qfc39B+LfrhIa8If1pw92YXxi+t+mLM4DzYWLLp/hdxa8cQ78Nkv2D9HYOewY6unZrbb3devhTzK73ffr56tzP5bws+hvG0zyC

kU8QKyrxcMb/iM0t6wgOY7lLOxui4Ao9OPljLgJbrTtRZoWoBKFEYOwqj/sK0oX+0k/0BHT/gJRGBHquocL+60F07P9Gym/wr87/G/4cWOhuCr3BZQAwOqG2FFxiQCYAGP8PdfXYW4sst/YS23/VAHf1GDxcDFf39sVg/wjwj/B/WP9fapKFT/qv8bZHP8AAQv9ASkv8QAYi11/nv9Qwlv9Otir9d/hhFrhML9/aIf94AZuZBIqf8KAOf87Qlf8I

wDf8nPvQ8hbpz0Rbj293ntOcHjnXsnjlLch3jLc+/rT8B/p7sh/v/9ySoACqRBP8YATP9wARwDIAYSJoAVP9YAUf9NzIgCd/nwDezOgCkAbD9QCnADUAWT5l/mf9p/pf8W8kQDrjGr8XIq1VF3sBt5GJa5vsNUApIKwkrRq/tgVCFofgjKoDkIcBX4FtRUdHuAOYqe93gAncBBm79QXGBd05p79AxiaoffgE9FVr18ZBv19ZGp8BxwAPpHUi5QrI

DWlGNvQAvQAcBCAHUAnTp8BHCI/s9hNhRh4NnINfH6BkwJoAQQBwAAUgYsh0uxkvYrE8grGwBtvrxkkol2QqzM3clgLswl0rsgltHb9LVl3daDtRd+5m1cJkjbA7YHNN3BvjtW+suRsACoV6SluBmQDCAW8sgBhlgMC3hEMDixFSIxgUz8nnh+4u3t/RKAUgsPnrXsKWnpdufvutjRIMCKSjMDRgYqFxgbSt1fjl9Nfnl9LNB2B2st+hJgK4QJTl

u8bxiXIMpgKQrSvKdLyHsgG7mIl7gEqpY8NjpLfi79fxqBdwDh4DOXvFduXoldY2n784LgEDjTj9tggaEDoIOEDCAJEDHQNED6ALED4gUygkgb1B8AKkC7QOkD8KFkCcgXkCB0gUDicnm0U+mTlIDEaJ1skYNrXleAHYErAA+Ed8VtASwh/Eu5lgJ94PXlX8KPvY1Oga+BugQ2MWDn0CC3g3kiFAjdVdkmEUvjhY2FBkVB8mSttir2ZxWnyNDlJi

VwgKBFOlgqDZclKCjzDKCeRk5NlJvm8AvqKD1IOKCR+qBEW3mXoRajqC/QLKC9cvKDNQUqD+RqqCyRgmENQZqDNzNqD0LIQoqRvqCMBiQCO3sz9xzlz0mHlpc/+v29njvOcGASMs3JsxExQQi8RthKCLQS5orQV6DyFLaCKAPaCFQY6CVQVf8XQW6D3QdcJPQcwpB1DaC9Qb4ADQZoC1eledDHucD70MmAjANr4rurUBdtvcDuFmV8ngVKQG+okE

v+P/xXFO4pLsIzlkNiDlGvi4CMNm4CgQb6Uluh18wQV18krnVEoQQH9AgVGNNMCEDJAGEC3QBECpiCiCYgXEDagAkDygFiCUgcd08QSCAMgYSDcgeRASQYtkc2stlcDrTA4AOh9q7vhcobD9hhhNUCrwOX8CPid9aUkrBU1M44xppX9yPgPMNcF0DlMiKFaPoxcjQcxFCGKaCWlsqCoYrmDzQUWDazk0B0wZmCtcuf1JQTZ8/6jqDUIWWC+JoaDr

JupNoISaD4wXeVswQhCW8i6CkwTAAUwcWDR1GhD8wRhDKmkT4sITZ06IShCfQeWC/QfMDeFEz51LhOc2fqGCFZj58lZpCtpbtGDOZjBCyIaNscevBDnQUhDsIdKC8ISCA5QUxDnlixDFIexCcIamCsuFxCCIZWCANtWCPbm5cowIkBNAE0B9APEBxwP7czSoHdnnMhg4pgLk4gu+BX6JPp5oKG5bkG4pHkMGAPKA/Egrsw17WElg/eGG5GXs79bt

jItXASLF3AZOCcNqCCstLOCIQeEkdqm+8ZvEgdA7KuD1wZuCogTuCMQZThDwTiDjwfiDMgdkCLwfkDrwXUkigSh8GAlPA8LnSDeAPbB7AcppHqjHw6gf9UQoY6UuQUBD2gWak+QfbAwIc98e/umgu0FuAmgEIAoEEwAjaKeZkfspVRQT6Cu+gId1QE8I8QLP9R6CNDKBuNCaQJNDpoWL9kAPyIG8ipCuRotDrZCtDGfkyN0AOtCxoRNDSAFNDk8r

tDZobGD5obkBjoctC1QpL8OAJdDNodtC7oWl99odBCnoTENUAEtC3oGqEzodZVnPgsD+IUsCo9CsCQVjXtOfpsCWtv58qgJ9DrobdC48jNDHlnNC9QQtCgYSdC3oWtDU8KNCvoTdCdob9DHNgdCAYS9CQYev9DIfydL1EBsvIvehiANgA3QI0x+4IbRSvi+AL0PHE/eM1A+YLqA/qAKQ0sHyseVLK5eYNBwB2K8DdkI/AC0F5DsIOgYGXpikRwdl

Nmvg9t/xrFcQQV79vAbActLK+8EXIgdELo/pMoQiCNwUiCtwaiD0QXuDMQTyBkgYVC0gaeCCQaVDiQSn86EhVDh0hxkzXpAY3QHVCOhPaNdwDbATsl+C/uq1CO5olgbYBcAJYQWMAIc6tPXjyCrYH1CBQSpkhQVNdlyGTCifnksB9uBFQAYbJwAenClfsgBdNlnCUwjnDdzEephlvnCqRCflM4Z7tWwqXDBzHnCfoRnD32qgBi4df8RAeOpIYaOd

AwQJDgwUJDe3hz9WHr59xIVGDK4TCBq4XCs24UQDYAY3CMYbtCi4bXDs4R3C6YdoDcvroDlaMaB2svvJwQOCM7Idu9jevaM62Ok8dhisAT4ArDzgOcBlYKFDgMn8CIoVe9qWOrDNTuBd2vly8Eod79dYbnNIQeos0rh+8UXPlcFstm1KoSTkKQRdV9EG6AygaatV3AWpLWI3dRCFYMfTo2QZgKwJG2l1Dgzn3ca/pwMgbP1CaPovMhoVUAciniVt

KimI+8gPkGbn4U38m/V9jiMMWysyJC7BHJj8sgBRwOcRpfg2U6EcgxOlu/VqEawiDZAwjRTHYACAJOZOEUUMaETtAeEZwAT8kwi8QCwjaEa+gp6BwiqEcIjuEfQjxEVxdoQOiAYoLqEZfsL91aAT9RIFoigtpkVFQJOZ9Eb99pAXoiyfjj9hfpyUsityVOIMaBmADL9RSmEBhlgQjOIEQiwKkSUyEXEVR8mSVKEVJcuETIjlERyVGEcwiREWwiRI

PIj/EYojAkdHJeEWoiBEbqEhETBUeJkojYkSojJEfKEwkbIiIkbUskkUZUUkTEjKRHEj+ERojQwiYiVfjoiJfupBykbIDQ6oYjmitcIakSHVKkSr9CGE0jdZvUjbEUJB7EY4ifEUEheIQ3EWfoJDbjuz9tLkPCxIS8cJIa4jXCup0WAKQizbuQjfEYU0FEckiskUEiJEaEilEewjckSsj8kWsi0kcEi+EeojBEbsiT8vsiikekjNkYEi5ETsioka

sjUkRcjDkfEjSkZuZ2kZT9EAeYjPvpYiDEVyVNERYiBfqYiFfp8jyfj8ibEYKVMqg4j/kZgAnEU3g6Fv+t6Ye5ENjIytjXFLZfAlAA3QEIA7gXvCHgXxZLfstAuwdLDrgPsADoKIl5SHsF65CqoVYXds1YRqc05rFDPAVnN34RIMX3n4CgntCDBXrCDygCH8RvmN9APsB8pvuvxVXrH8oPgt9YPon94Psa9onqXcM/uXcZwCYk2Ehh8dvsOC6eAC

F3wXax8+i3dn3EVQy+oWZUEWMkeoXd9RhNR96LhBCtNiKDmIvT0LjDM9iIRj5LUR2lZ6hDC+IVLM3PtOQPPgAw1gQjC6AT89kYYZcYwbajsehj57Ufg0LzloD/Jh5FsYlJAVXuOBCrE0BCAFzCM1Glk5TqikpSHqBKuFrg7gC156viCEqUZFCxwdFCJwd49CpslCeXt18MMv4DFwTCCBvp8BuUWH9Rvv+9xvkB9JvkygY/pB95vjB99XuKjlvq7D

DFmn81viAiC2nKjHwVa9feI49sqP84HXrLB4duFIJNJHdgeiR8MdpX16Dh21YelR9HvgG9SnhW8+rhtN6AJ1VuqhEFu/lkth3mrR1hHe0guhe0jZNcJrUcejS3n21vrk/9QlsA8d0V1UeAD1UEWnx0z0Yl0L0egAHnk50gwRQCQwQPCxkeGD6AT6iV7nvV0uh+iial+inLicDANjoCmYWQ0FoITIeQOOBz0hY8+LOb9rGNytHSIzE0dmw01XNfC4

7jmj74XOJ3flKsX4fFC1LEyj5VnAdWUalDXhulDjrDWjJXnWiI/o2io/p8AW0XH9RUR2ik/pKikPun9+0Rt85URAiWkoKQHsE8gGaEcBUDHYxUjDasY4Vd844VNMhckai10RRxA3rejH/o2sH0W7VyGpQ0yPI6AD0XR9OHuBjF/vE16Wp3DSelfMZmn/cm/iG8dMa3U9MRQAqGo6A30VADzMe+1LMQ50AwZDDnUaz8RkcJDvPl882HiPDQMfG9/n

vl11IBZiV4WGikUdjF8Kt+gMuDSBlAM/tWwftsdpOiwPzmg5eVg9gm2H9EOGj8Ds0c4DVYQ/DaUXe9AkmtUoLolDffslD/fu+9NFqeJmMeH8G0fyjm0UKjW0fH8xUXxiVvr2jk+udUB0dOB8gAQcoRgAJnmJHwGaL1Mw4eORPwN91agZd8U4tyDlMauj/XoKCGLmaiAvmiJ1IGiIL0VejVOkl9tsUT0Bkc89yAd28AMVQDxbuMjd1pGCwsWvRNsa

JADsd+jXbvuMDHiZCtfl8AmgGERGQDABywNn80saYC7FJljtho6QDoE1AjgIoRZcHIR2YhdIisTN0vRtSjSsYCCvHve9KsY+8S0XODYLt/C+vpWiggVyjRXsN9a0byjI/gKiuMSKj20Ut9k/uhcbTqAZkPjKi2pswBRMVn1rgCKoRlANYSLkSiIJEcBpkLtR50YGdMdiakDUbX9VMStjk4WtjWDgW85CupA5CtBjzoZJCJcaJApcff0jsYsCXnss

CzsasDqAZ89aAd88tga8c16HLi/iorjjgaGj3buGi3LrFkeAJ2VywIyAQrAl57IcBo8UUsAtoJYDoOE1BrgPbBlkMFCJuoVjvFMRjWXnmj4MjFDC0Q+9i0eCCasb48UoQbCf4Q1jZ5E1jWMS1im0cq92sdxiycZ2iKcZgdjqia8YntVCgrJaQaQXhNhwaJIkgKHCJ0RbBm7iq5HkCNMHYCoYJhKR8vFmgjq/rWMVMQ99hceBDcEYejN+l7QP7kSo

c3odiZcWvQu8TRCdsT+iGHsMj3Puut3URrj1gbpcvUTriJIf3jqHt3jW3r3insfO84MWvCEMXghWgFtMiEMmBT/GS8d3mllFYA6NrgKkA/eK45oOG+AkNh6NisfDjSMeOCkcRViRGqjjQ8b4Cv4Z9sscRyiq0bjif3gTj60XyiE8aB8k8aTiE/t1ju0cDtTXsUDQjCxxfYQDYBVL5D3XqDYHWEMoFoLzBlSEJkFMQtjuocujiJE3j6/iai28cZii

IWvMBAVwDh1CQSD6pZjwOkaDOAQfUyCf5tP0f6CJZj3DoYU05+4edi+3rOcIwYO8bsTps4VvupyCXflLMXCi+TqvCzgevD0AD0wT4s+hsSPGiMsQN1PEMfj9gHzAnqpWwksCNZGXqhsg2rfiU5nlNA8cjin8SHjqsa/jasQuD6saRtIALHjCcexjicUAS20SASJUT1jqcYJj+scJjpwFAAGcR907GDtk5CBqjgVBqjwpIsgj4DFMKkHqjWrtgTkY

sticEZktCCb6jcli3D+CfQTF/pQT4Zu1t4iXESkiYwTLjswSVcTDC1cXDCPUZdiB3m5ldcbwTF/nQTosTBjjcS9jTcW9i7QPoBjgOstCAAgBu7OhjaBuV8BuhsM0HPcB74HsB7vj60NCb7jXfv7jb3vlNH8aINXtkYSWUW/jy0WYTGMV+88caH8WMVYTWsYnjwPh1ieMeTj+Mat8+sXoNIDOSC88cxsnjHqARlEoQpMVOiNtFcB27pJpQiW0DwiV

1hcCcaiIzniN28ZJCPkS0jakewpdsW8St/l5jxZlkTfMa59/MWPipzhdjgMd6iqIh3i26Loj3iSHVPiUbiqwdi8xCRviJADyA7QPGBo0XjJUsdii2wWYD0WFY9xDOrwlYMdhbkMGBXwBmVCMY4CrpDfjc0R4978R78tYV4DQEh/DCNnRjI8R/jf4cuDq0QsSeUX/iicW1i1icnj7CV2jKcan8nCX2iXCZn9pwEIAPCa6cLTD9xazIgTEdtHF2yDz

BK2F2IONhX9Y4YtiBcQ8S1MSLjTUWLiAvmYiYSWj9kidZiqGECiTSaAVy4cPiyAZ/1TsWwT1caCTOCSBiISa8TLSegChCStsqiRr8aweISIAPDwfgPGBCAOCALXrbj94bQNpTvYpj4Y0QjgNfhDwF45ZXAHxBlERjqSSRidCa19NYRRjtYUyTmUQqtpiWyiK0Z/icccXNzThE90Dlqt/4VXNesboMsJpAY0MTn9aQUHERlD0ofvCXjUOIboJZCgR

XiOjtecYujbGgLjB5kPdIzg08YnJCTOZnai7/uOTTLiYVMiYLdO3jkTWCQFjAMWGCXSeCT5ipJCZyYXkYsSbi4sW5dv0Bb4SnBQAV4LISoyaBp2icmjOxCTFFCImwbgOw0rtqmTYcT+NFqojj6SdmTGSYjkkoeHi6sWlCjYYHYC7uWT9Fo4T3rOKTdiS0ol8DATikCsAeEsrBYEQd8psVdIw0lA4Azt3c68fqi7iQPcmDk8TdEi8T2+oXtD9rOTd

sVzsCKduSlcVDDFyd/12CYPCwSbPiowXhS09iRTA0TuTqiXuS3sYyB+JjwBqgD+p7nH9ig7m+ceYNhihrI8hH+IoQdsmRhY7pSSmXlIs4cTSSaUa+TyMbcNGUTrC8ybRiCyfRi4AnMTfttlcy5hWStiTWTMJpSDwKTKT8LgOJWNtok2cfCNlSVqi+xJ0CUKS0CWrrcSYeowdinsOTniTETxyVlUvyv6ie+gLUS6n5TCIbETPKYpVvKZZcjaH5Tn+

u28mCQCShkX3DlyVRSgMWuTaKTwSS6EFS0ShajRIKFTwqWv1mKb6TXsbWD24M6ApIGwAp2o6BpgENj98QfD+KfHdpYWWMZSERAA0BwNrYFLDJKZoSWXkMTaSfmiH8UhkDCeHi0cV+TkrrVk2SeyiOSaadSyTosdKUBSwCSXcacUJjJSUYBbIZa9c/vVCCWErAU1HBT4EZqiXwNMhUjEHCa8QuiJpgOSMKUU97dK5ScKe5TJIZCItsdiIzSeWs1aJ

dT7sddS5yY88nUYCTR8a6jx8Rz5nSaJCrsdwS3SbdjsRFdTe6F6SQ0QiSGYfBjSGlUAEAJMBHQDSAyxDABFBLxTXzmXIEgjVSH+AGgvvGd8peFmifcWmS/cR1SA8QWj9CeMSqsdRj9TuBN38cNTo8cgdwnhNTAdsBTTqgZTQEbTAKAMZT6oXIR+YQLk4Kd6dNqUlEoHA7BhhDcSl0U5T2roPdgaqjYl7q99ZcXZMtJg5NvJrtj5JpbNuJnLTbSQu

STsarjHSfkTJ8Z6jtcUjC/qXgwFaWmtlaSvjsvmvikSRDSJAJgBpgHaBGQJgAmPkaITAXxTkaTGT1eM+NLgPVTxun7w+NpSjcae1S5KTFctTgySlKbmSaMXrDWSZH5KaeYSsrmWTaaWhd08QVdM8dKjZqbKjpwDcZGyfniTIA48bYP914dihsyJraxFVN61WBILTDqcLTxNidSxaSOSXvo09xySQwHqY7kpyRdTQ6IDSQ8r8ToFvOTsiWrTciRrT

PPqCsp8ZLd1ybdTVOoQw56MDTfJj6TTgX6TkSegBrAE2AKAAgBywG9lTye/sQcrtRjTIfBH+C8x4MCLB7eo78caU+Sk5gjj/ac/DFKRMTSaby8C3OHSiySNTMrgBSY6XldX2GhME6TNSJScnSjAJgAh0UtSwOLIYXmFzSDsv2Ij+Ed8JElqSsCaXTMKS5SK6W5TIIUQT+GIQxDaV4iG6XriPJjLSvJvAyyKX5i3qWz4e6fDDCiVwTiiXPj9aUgzF

adpNUGfCSjIYiTJ6ebT0AOOAhSLUBJgCn5frBVTIycvSZYYJTkUCDiBMjzAKkEfAAQlaYYccy9L3njS/aey8sySfSSacpSQ6Z/CTCZjiI6ZpTVVtHS0DpNSRSbqsxSTsS6yS0pMAKzTfeBq4orMyCyDkqTmclchkUtdkRMhgSRktd90EbWMhyRAyzqVAzYiXdi9sTdSf7l7R7GQdi0Ga9TYqcCTRkauTvqUUSC9PgyS6C4ya6KPT6FqDTEUar58q

XghqgC11jgDAAUgAtTwyTiimGfxTBSMaZrEqulQfGcBAcs1THekOxWqQIzfaYfThGQHT3yUHTPyWHiBqRHjL6bMS/ycdZb6Qoy6aVNSn6c4SwKSqhy9GnTDibSkXFisBi8cHCXICe86rrawbGFMhIccXSsdkdS6xriMbGetjoGfriFcfLTSupLjSuq3Sl1s9TBkX+iHSXFSnSRwSfGbgy/GXRSCGS/dFmS/dgmfCjRCRQzjxszZNAErB+4M6AthE

vTkmUDihrCfjHkPxl9wON0KSTkzeBoMSAQUfTgQSUzT6eIyyaWosKaVfSqaf+SaafUzY6VWSMLiozayYZTWmZoyAbF9wH4FcA1UYRBSDgYz9oJyYvWr2TUKUGd0KaAzjqYwYnvuLTRyeLlw5EoVU8upARhiUNxhtEMEGebJaflSzRIDSyxhsuAzKk9Tf0b3D/0d3SJ8V9TgscPDJkfsyS6CnlBgSyyihrSz2WfSzKiaEyNesii1+IkA4gaiTGQIk

BT4h9lopskznaWwyjpH7wTpKsAzpFwJHyfwyZKemSb3roTCaWMSZVmIzg6UCz9YVUzfyXnckLhCzKNooy46Y/SpUc/SWmSqhIKSwIiII8xVqfro86UIl1cNA5lkKMz+ceMyrGSSzK6XgjYiQOZFfjHJdsfGzRIAOZlmd3Doqesz1aZszNafyytcSFihWclSvaMmyUKjbIcqRPS8qf6TSQbm1jGHZgEUh2DNBGFo10p8ZvFIfD96cBcP8NdA/YnSS

FKT48KmXKsDJJIzvyaYSHWdBNXVjqt3mC/S2pkYAP6U2THiOcAwdG7jfCRtRuaSq4LgEfZ7YEWNTGZD168fHDMEaBCoiZNc6OL7B/YPoBA4FQY0ADHAalMCg4SJyx4QMnAk4M/IKsHdRM4NnBX2fF47hJ9gy4KXARNC/Z05PnBCYP0ADNpiA+UBEyJAI0AeQDyBkwL3oGgKeTAJFBstWaJhlYL8FK2K/AyUUQdGXox5pKc+S3ekUzj6b2z07mfTS

0R9sZiSOzQnmOz6ZL1B+oINBhoPRs6BD6z0YI8gGQl0kGaEWUBmfvYQ0H69URniy+ca6tbvgnCsEUnDW8dETbGW99WRvGcFISopcwUod1xkM01PqJA0Wu9C0etWdaCmi1CGKpzpOcpzkGm8UGGFtiUZNCAuOIXZCoHABCoK+gAYUocu8r4N0oMpBMoNlAVFN2EVIFlACAKT8vkQCiKkfL8qkcCjvkV1tT8uflnOSCi5fmyNLSW8i6kb5zsji2NLO

YhDJOVRCNOcud0hgpz1IApyYuX99VOQlztWj7kkub4M0Wvti9OWwADOcmAjOSZzchjjCdPlL8LOcSMrOQ5zbOVxB7OTZynOVL9guVT8VfkQpgudYjFQH5yvOW5zAudT9qkVCirET5ysipyyR8R4z3qSCTtmQKyJkddi3SUpzYubQUJOVxApOVL8ZOfFz5OWlzFOeFziRilyVuSg0dWhlyNuWlzsufSBcuZVB8ubIBCuWZzSuXiVLOTVzVIEJA7OU

pBKuXVyiTtL9euYL8uuU1yeuS5zZfir9WuYScxwg1z3Oa0iPuf5zvuf1y2uTKyyGWDT18ZQyAyXmwyIOOB8KqeToiB0TWGYdhRkHKpc+spoV0q15VSOFDzhqOD8aSMS9CZazILs/jJifmSpGSCzqmY6yQzjqsH0H1ABoENAeKdnjQjGU56OdwFIccSw0niuyPHIbY9gPyDw2bxzvXvxz92ZJsb0ctMtMeU85Nq3UmwEVTbYKKZpgEZiROeFjFzLW

U0uQy1RIFeV9QhxwfOI8ijaKKzgSlbRM8tEAZchqDliiPTiqrvQZcoV0dKjbJyhrJydWtpAZcSryxeT1cfrs/9gHjLy2AHLzdYgxVVOfE1NeROFteVxxdefrzlwDLkjednl8tpfcgaRbyRClbygmbHyv2kV1BzHbzVOY7zwYaQDVafaSs2Z4zAsTQCNgTPjdaRuTVys7y/eUF0A+VeEg+b5xOAHrymWSoVw+RoVjeVHyH7tdTE+T7l4+bwwO+ZHl

k+RGBU+Wrz0+ch56Fli9IeWbSLmaYF2gHWVSFrUBCmM4BxwA0BXCKQAGgHlwUgI6BMAF2zsUU85iuBRgHcfSDHmTshW2ZJTceWhtZKYUyyMREIoEDAg4EEWjeqS/ipiRTzoAtN4GMTUyeEKcA2yk0B4ZOWBkwDAA3snr19Xs0TmALUA0SUyhnAMuB1oHAA6zsaB4gDABS4MoBxgJClQybZQDFqCBNAMoAmPsQBcJN7d8AABgtGAeBP8sb8k6QLh0

+s6dZ2TzJupvuAltHsxLKZizb4H7x7Rh7iBeeIJoeO3Bv0DPAKAPEtYBfF4HaReksRsjFoNgHDsmdYy7svLRsYiwLJAGwKpIBwLWiTwtnAO9VTgEg5uGaZSzsOZS93rQLH+FV9/BCtgPjBHDxKdDjvFITo8eSVi78Z1S3yaIzSedRiB2SyS1KYW4o8ZHTX+RO8P+V/yf+TGAwQCYBABcERKcCALmAGAKIBVAKYBXALnAAgKM4AOlkBagKOohgK3Q

FgLSADgKeAHgLnujF42eeXx7WMWg1UUmi2OW4hBMmG5CILiz7KeYyG8ZVZT4HchVCQeyJadXSM0Buhhlqxwq4sGk1mdyyNmegBW4nnzejEckaKRAAeABPyBgLUBp+bUBZ+fPzF+cvzV+evyZFBJDKhaQyEUXKzsYoyBEeFJBRwH+pXCE2BvCkRApbE0BJgPQBV+Xvj7gZvzCYtvy0smGl3jOk82vDdsDBdoSzWZmTimaYLDCYRz0cT19IkuySwWc

dY7Be/zMAJ/zv+Y6Bf+S4KABUAKPBaALZAD4LoBQcBYBfAKOAIgLghSCAUBWgLwhZELohbELQRsoAZ2enSnuCVQbGJsE5pkjsTIDzAjgH2wNSc0DAIfXjQYmvx/wCkBNAPQBjgI6BoDGhIuBYjFByQUK7yX9QegSnCcvKBypQH+BCRcSLSRaeS+lB+lbgP5DuKOcBKuP0zPxulNvmS+TfmfSjA6QCzg6RYKDTs8NH+RpTn+ZpgHhQ4KXhW8L/+W4

LgBd8LwBd+hIBX8KARQEKgRUELu0SELwRWaAIhdgL2sjELvsHELEWQ9xoKZFgsRcd9t7BLIMWc69lYOg5HHHZScRQSyeBfppqRUUL8CcJzpmRKEELMXEOCkwUlAAoAAAIQAnQjoggEAV6ARADyhBHgXPVADSibyA+AGADOADEClVe4QbFGsSHcgzkhizgpKASMW5i/TmVQDYortEpyGQMpyN5ZgpFiqMXFLSUIrOEug0gXEDZVcFEVc2rlCQFxGL

FH4TBi+25W5MMWRildqxipaEJi+pbJijzj7tatAZi79rZi8UoQiHLn5i/sW4FOsUlio7kIAcsVAcqGIrOGsWL9CMX1igSqNiqsWkQNWgti6WCnKdsXXcxzldilWkd07Pld07NlYMgoktCyYWxZGYWOgOYULCxIBLClYVrCh5LwWDpoFinAqDig8VQlEcXxi5MXjilMVTi9MWZim3kDAfM5S5RcWVQICWy5ECVrigzmbi9lpo+HcVO5ECUrtI8XpO

ZsWtii8W4lNKBXi7KDg8sYWMw6HmlRQgCnAccCOZfppsLCMClUwgDz4VwjJgZgBECjfnnxUrzbCir6h3UTAH8z5nfOIUU4cs/ltfc4U38snmqU+/k3CmRlyiz4AKip4WOC14XOClUWfCnhCeC7wWai3wX/C/wWBCpAWgi0IXoC40WQis0XQir2E/ABIVA2WXC+uTYIKaOoFZCxWB2LbgZAMxTFV/PEX3qSQBwAap4UAHkCcw8kUfZSkWRsn0UeeP

0WHs/fyMiqQB+SkpyBSxLIm/O3FbCxxgfpdgYyqcO52mCVSPIcSlX4vhlYcg+lfYWDIiioPEo4i4XiMyUXk0h/nKrWRnlAFSXPCpwV/81wVaSzAjqi34V+CwEXAig0UmSo0WYC00W4Ci0WgjXeGLUkgWdYb7qvg9wSeeFAx1AhVT3AJx7ui4Bk7sgebhS2kWrYg0nCg9NDrkQABY/8MttpW4yYqTyzHxXyympJ3EdmSeNxgPRLGJcaBmJWwBWJdM

B2JYkBOJdxL/xZtK1yDtLRhWcyK2VPSeqDwA6iVWADgLjIDgA3YPQA0BN4WwBRwCK8eLNfxaPAJKBukeA9hc2yh2EfytCSfyjBQTSuqX6USeeVKbWefSUcnJgZRUH1R2bPJ6pWpLlRc1L3BdpK2pXpLtRYZK9RcZKwRWELzJf1LzRfgLJ2WwQ60rZLCzMcwUEaDYjmBDY0sKmp4KZqTPJd4tvJZZpMAMFN9AJIBxwIQAsSYjTQpYSz7AdwyaRcUK

nXG5cJZc6ApZTLK5ZQkycSbNAORXYIwtDaZOPD5C+2FwNvaW2yorh/g//CVKiaVayzBRVLcZSlcFJaCzbBW/zFRY1L3haqKvhV4KfhdTKOpbqKupUozm8D1LGZX1KohZZLBpV7DMxgkLd4IWY7gJ+D7RcLAbgEMpTsCjF67gwKbvkLyVsKkZfRdhShBQGL3cJUZ3pRWci5b7gS5SOdHUTUKWCZRStmR05+jIlSfpX9LeYIDLgZVABQZfGBwZZDLJ

uRuTO8BXLzzmPTZWTRKx+Q+hoUsEAQQDSBEpbrL0sc4Bo7mll8ScwMz9EkB74FYgXiNHcdBRbLjWdhzQDt2yU7v8zrWWUzjCUOzpGa7LapdaAqZVqKA5UZKQRQzKzJeHKoRVHLICdi4rRRzA7yeshn4J55BwaiLOhKsF+YKzjhZZgSlpVSLlZXnLTqQXLDSZ0NPyjT0SwPIAwuVArjlJZBYFbeKM2bUKc+SNyvGSJDxuT9S8GVGCLCkvBoFYgqjg

cbTYMcZCaiTFLxTlGBQorrEemPgAHUoQBywHpijANMAEuFDKaPMb1YZQw0CUZeTGyPsLVSB+d/gcKLcOX8zpJX2zb+eTzj5ZhkbBWfKtMBfL9JTqLr5d1Lb5RCLmZVZKn5ZIBuMoqjygbZAw0osgvPHswYpHUDHsDeSMDJnKLGfkKQFRFL85R4N7sm9jjFG2BGQPQBvxeyK2KAw0RJYvL4ONhBUgBdgtgocMnSoy94ZZbL3HkIzJJSIz8OZ19Lhf

1T5wSfKqeUTLH9DpK/ZZfKDJZ1L9RcHLDRWHKTRRHKBpazLdiX8kEhemi3mdk99dNzzAFGDj+MjFpTFXkKUpCtKG/mW870dpiCbsA9I0WaMY0YQBFeYXLwsYrcZLjJEVwg+EgInBRUAZ0rOLnREsoGqFZwuqFmIqgDBIrhL9OQxFggKgDlWqq1T0bGsKQIgqdyuChEFatD5zAsrumksqiFhwwdyoEATnO9CexeCJi4osr32icJllTAq1lSsr9AGn

l0flsrkuucqzMfmBVWtQwDleGRaYY8rMCshLf6idzjOVAA0tgXCEFXcrUAOsq7ld4F5lfbkMJZVBYKgVzAVWZsC4dQxUAIcrIVd8qlir8q4VadyEVQNsifiCqyRuCq08n+AoVT8q8xbCrihvCqgVVXDkAMirUVcSqnNtND4VaZz6arcqyRvbQAVRGB1QJMraRmEAEeLH0WKtyq9lDxUUIkKNdQvOZ1xjSMTHKUVd/ucQ0IoKrlDpKr0GnADZVd7N

5VTioaRn5EMIDpwIwH+AiAXiA5VRMCt7l0qrwtaEFIn0rRVfuZBlaHVhlSQBZlVuBxlRj55VfFljxRcYZlTWESVfR0EeLWtbMXUrJeVW8xyk0ro0WwBY0W5jfapcrZlj6qJeYA8peWs1MANBAjAC5ZsgPW98VTcqYFZsr9zNsqN0c38HMWs1A1S0rQ1VwDw1Vmr7MQ0rdMY6AF8DABIOb3V3lWxVUVccqgxV6qI1Y39fVdGr/VaOV6AEhi/wChj6

AAWqYQAM1vVc2qo1ZW9J7iA941YmrwgMTNWVamqNlQ8qM1U8qm1bUqh1Vuiwlh2rEgMhjUMb2rdmq8qF1Zpi8bq2qR1asKK1VWrMOvsra1Z8rZ1Scq+RJiqKVdiqqVePCWVTAqwVayq0VXOrSVaWK/lZSrEVdSraVZ8rwQB6r4RNer/lYVA71XT8H1Ygqn1WmriVeiqANWSqP1beqv1feqf1Sc56VdBr+zEyqiufiqgYfCrOVXAB5VRqqsgEcJJA

AKrxVa8phVfuELVfI8xOe8pFVdKqKFCqrwgGqrqejRrb8sqr9VaqqSNTNy+RpqqZAEwAdVXqquWgxr9pZmyHxbnyVyZgq82YKye5TvUVylaqT8uxFVwuaqjIAMqjVUMrulQqERlXaqmIo6qONYRLqxRGA3VbaF/1acrG1SeiIMVcrVlWxVCVemrL1fcIzlTsqzNXsrUGDSAPlUcqjNRyI7NaZqXleZq7ldOqIVReqG1bgBnle5jt1cu1T1Sirz1W

5rxRIBrP1birgVayqINRsq/1ahqr1bBqsVQCqQNSfkkNS+qbNQuLUtTer0tQhrQNZhqrNVBrX1Riq8tUBqcVU/U0vllqUNWVryROhqb6vFr2VYVAcNXhreVYRriNfuZFzGRrUwgoCdNeqqpVSxq6NWxqGNQNqmNUNreNWOp6NcwBGNQcoNVdHVtVbqqZtWWzTaecytejwAIwN+hQsj9iaQOxTqgGwAjAFvhXCMn50ZMNKEmZsKYZQIs62HvzhJbw

qvxj7SfmUIr/4KVFyoleMypTJKIleUyolZIrbhZHSbaA0B+4O0AIwKcBHQKOBNAE2BSAAyohAHGp2gGcALXpABoWEKR8ANDTlAK4RbpcwAhADwAeQJoBnANkD9AOyp6aToNGaazI40cNjZST9g10uizuaUng3IQhS3TuDiPfBUqxZfegvQGvy3QBQBWgG1Zd0vZCFZV6LLcOUhPwKMJVZdak3sWzrNABzqudeyKFVI/xS+pHc4EdKo4ZWWxuRWfo

y2OUgRCBaYa5MKsWqfoLj+aayWvhrCzhWEqZweYKnZYNSj2DVKlJeUAAdUDqQdWDqIdVDragDDrpgHDrHgkygkdZMAUdY6A0dRjqsdTjq8dSCACdXpTYWSTqIDEB9Y5afBoOK68XHMGlURf0k2YsshshR6KwiYrKvBKmoA+LHg6RaLiNpWXEhOCMLS5W5xhKp5xK7nxDKnNULjsfeKlyfUKvoLHoxNS+RTpVgqZEJtrttfQrdDvtrDtcdrTtZiQX

pbnqPOPnrB+aczYseEz/STwA9AOvhsANUAmgCCAoACYB1bKOBoIAgBF8IyAeJUlL0AJdr2FddqBursLpYW4qhwbZBb4UcLUZRmTDdXhzr+aIrZJaHSrBRbrDYdTzA7DbrgdaDrwdZDrodbDr4de7rXCMjrUdejr1Qn7rcdfjrCdY0yPWc0y6yXEDY5cGASONHwadRIQemUnLwpAWpdgOul5sWYzPXizr24H+B9ABOVjjPoB3tQa4QpZXA0eELy09

U1TOGlYrege9oYpegbMDTgocDavrEmdIK4jJyLHGL9lkgP9ldgEzjH/Iy8uFQVL22afzd5VJLjdW/DHZURys7tYK/tdIr79Xbqn9Y7rnda7qEdRAAPdV7qfdT/rsdX/rA9QAbg5eASs8bTi2CPXNydc+CDoI219wPrp3FrAb2QgKpjUKMoKlfHCiDSIRM9WtKCCUryMCksVW+RsUSCjnlTcgpzfiubzXDY8V3DUcV++TmKE+T4bjmV4aQKsQjiuv

OLQ+UWstzBHyISluK3yrCUUHhsVUqTlVvyjmLflZVrX0HEbsJduKXVTYUqIV0jqufdzOxdiVafmf1AiokUYioflaShEU9gZUbkirEVmSpSUqjTSV0iqDyGkagBcinyUYUYUb9Wj0b2xYtqmAFwwKEQhYmin+AVSrKV5Soa0OjZAUsGjg17aJ5N1SmAUlSti0JjdxMpjcsacxQXBTQupB6AGwAD2PDMnDZcULciCVZCpA0/DSpzVuaEarisEaFWhc

atOaPURCtcbLeb8Upcc8atKrMjSjZSyVCr8VYjVhLoxQkb5xQfdkjTXkFKmlTnyukbYNZka4ANkaATTuL8jViVLxcUabuV8bASuUaEioyVYiuyVpgfUbqSt4icTZibqjW0bOkeCiJ6v0ayJUKVMWuSbpkVxBBjaQBhjUsjkvlg1xjV8ciGQ5MNjSgUVjdMb5jYVBFjXKUkClg0uTdAU1jeJMOTYqUtjbgAdjaJA9jVuxvMVFSXqQdK6hegrGhX3S

ufq0Kx9acAJ9VPqZ9V4L+4PPrF9fQBl9T3rAxVuYsCrcb1cjuF7ef+VAjTcb5xXwV7jVabdWjlrtsa8alme8ak+fBKIjUcb+zHXy3hL8bG+YGJ/jcUtATd6bDCkkb5xSkaQzc6aMjehrYTcGb4TRiVcwYUaOxSiaNijiVyShUbCTa0a6SnUaszfibczayUsTTUbgkSSaKTXqF0GvyVwIKSaqTX0iqzWWbLtFqqhjdSactWMaRTeyaBTdMahTSgUe

Td3001mKaZjR/lVjaya+zR2bNjfOLoINsbggLsb9jQxJhCfo9cqWQr/SYeldGM6BNAHQzi7KLhsANSJywJoAIUhcBWFbb4xgDWw0sj9kd9fdqkZeJKd5cYKe2WfqCOYCyzdZUyNLDEqyObPIJDY/qHdS/qXdW/rKcAoav9b7qVDQHqg9UTqQdl7D6QAkLDhrmoArp55pMW1C4pusFRpkgbt2fDZUDVUBjgL/M3QJ0xpgF0EedSoI+daGc42DYaM9

SLq70jFK0LRQAMLSLBsLYjSt+XnZo5uDZpYZlKt4NlKY7nlK96VvLCpTbLntaVKeqefrTdcIa+XhINnzUH9TxG+b7dc/qnda/q3dT+aP9Z7q/zcob/df/rg9SBTVGZSDzFsQL06aIQgSB+ADkPoqNqeFJD4AMldwG2TsRYtLPRfhbYeuUhiDXYb9SQ4b2lXtKZcfZaM+T5iFTcJqq9cqa69W3wG9RJrTksuaiZGua18DABNzdubdzdBB9zVJqoKP

3Lg0YPKIeWEy/Mlr1fAhQAX1Hmw9GHh47QMwAAZU6A6sGklrxuvrIyceb7+GFpd9Q19HtYIqQlUbrbzeEr7zfxaL6U+bSOcJbXzY0Bbde+bxLTIbvzTwhfzd7rv9ZjqALYpbgLRATmeYxL41O0zyrlTROyBRhOBE6Lvwb65DwP2Jq8R5LAFchaZEppg9nGd0mgJZRjQPQJsSbJA8LXxzCLSQawFdYrhBerLVreta2Vqb8N9Ud9KXvRbuFcbLLbKb

KYdI/5N5dwarZf/BOLWVbT9cHjPtUIarhWWiXZUJbOUZABRLVIbPzbIb39Z/rOrf+aFLWoalLQzTnukatOZWUghSG4pdgDHqAelZSdwAu4iqD4SrDcpjLLbYbiLS8S+5cMsibcgqXLagqRNe5b4qYckG5WdKJAAlakrSrYo3uMA0rRlbHQFlahor9Te5cXKqJZ9LFzd9KYAFGA4dc6BpgMBh5SKYpT2VoxN8NgAXKAeaL4rPLN9d1YXFQST9+eea

b4DHMSrRJK+DaEqKrSbqqrT9biOX9a6rQDb5DTJbFDV1bf9YBb1DW6zqySHq4bS/LL8JxQfWmjaV3MrB/CbaxFkJVci0AtKRZUArxmXtbrLUJyopdjFC7Jvw/YMoBfsVtb/scWwNcAbZbtUngzsNb1DUErCpuhYDAlU19eDdea95SIq7zTjLqrXjKhqafKrdYjrTbXJburVDagLYAaBMaBS6yeOB9wQqinwfVDKkARMOAkX86dWkLEsGN03wPa8T

LT7azLbtavWqLA+eTUrd1WU991Us8bWtrF7WuOA2lRAqj0SW9sOkF1QqlJd8OtMsI6leVi4pc1lKov1FcusVyHqSsVxbsUvEXJErTZblierg93iuQUd7XcVZckBU0JRQUFkQcUQZmnzaxdcJbciQ9u+Z6a9Ktvb8Ctfb1HhQ9tigrkdJnJFWWdVVaqjQ9ZciMMcFGIQHyvEbpKi/awqcsBn+og6F0o7Q5IgfcyCgrkjaCgZV3FbRpgKg6rIplTn+

hFTiHrLk+AHJEO8gA6cwq+hv0JOK0xX6Be0HJERjYqDMzYWb58qcUFcvUiRmnyUMWqM1OwlPV66Hw6hzbg1MtVuLi4gyzZnouq91cOrx7fQBbWlPbIHvE0l7Qtduthk117QjxN7eg9bioQUb7ZA12HffaBJsfbPDdva37WI8L7Qfaf7do6/7UQ877YfaH7YY7++S/aTIOfaPjcnzGClo7n7gXRlxTY6gHVBUJWWyzmAGZV7bsA6ihlA75treEKxX

A7t7Qg64sJzAYnXg60HSg8MHUv1sHSnKUHePUk0L5SiHWv0SHbeEyHVZEKHWcUqHcb5aHdWh6HcQBGHYybPliw6WjV/bt7Zw6CHRWaeHbfknTR6C5jd7lBHdMbjWoVByhgF1h/kJrybW5bMGcdLqKYlSi+dJquHrgBFHRxUVNRfUtxTFUhOBvbnHScUX7RY6PHcPQ9HbY6DHVZET7dgVziqY6njZfbVnXvbXNt46j7ds7n7dvanHe/aXHZ/a3HS7

luCsc7s6F47L7T46IHX47QHYvQdHr47kkaE7OwhE7UqvA7NDkg64nfg7Zcug7gJck7kKqk74nQQ6MnaU0sqS/0cnZuY8nbLkCnaGKinTQ7mODBKynRU6xSs5tqnQ0a2HQfb6nXq1GnVw6X8h06wCj2bKXSgUunTEMRHTkaxHTzah9XFbuqCiC7QKd0PNPbT1WVsKFbQwMzWEbY9wMdhOGclYuhI9ajWc9aglRnb0ZSYKBDVRi9bZEqMcZTyjbV/j

i7eDalDWXbVDRXaNDdNTgDZSDwdmroG7WBxelGX0PqogTjhqiL5SDMBgFEKQcbTqS8bURbReQ/8pHcurgHsaAUgN+hdDj3pv0DPac9Zgtpucod1hFDNfld+slQCcIievdiYzdirmVZqMd+g3SA3Tipi1W7yc1WOV3XZ66aQN66GKsG7YNaG7lwOG7dOVCbGtcVyoAINy7SdOM8iU+KtaTgzXScXyVygm7qekG6YVQgBc3cwB83ZG7C3dG6iubG6u

+qtrSFaxSYpd+h/mCCB0ZNUBuddRbeXZhjb4LA53jIRAT4PcBRCHFN0OTfDLzVFCZXTebPrbxaFXd9qlXSRyn+bfrjrB1aNXRbberZXbtiXCyvrDnJY5YWgPwBmU0WVW16dYUYSSWGyt2cJs+7YQaHXftaK6RpjxeS67FnhtNbNA0BH9kIBJgLXafgI6BagPhAjAMmAUscaBWgL67U4W1s8GH1tSAE27ovu/UEANerV7T1sryk26T5hltwnbA6IT

VZFM1QUBv/gBUo3QCqsjcfam3S27Owg2aeNfSaaXQMAWTfXQ5IuUMm3eI6vaMh7UPVZ848uh7MPXM6d1T+7R7dI7/3TyBAPa0BgPaB7SrhB6DgFB6YPa0AGKjh7flXh7Lmv87EAJ2ESPc66RPa663ajr89fgb8jfl/9mAYWBwGhR7CuZ2Fs3e+raPXJF6PTpw/wEx62LN7k2PSfkOPf06a5VXs65QlTabUlS3STktuPb8q0PfscMPXlqsPYZsCgM

p7YNap6YHTkaozaGbbNSZrSPSZ7yPR27KPTCbqPSG6hZsOMf1nR66TQ5737sybnPVZF2Pb8re3eQyvpdDziAM0Eq7G10mebQa9ZdHa+XUNVBCE7id9S74AcWnb8ecEqtbeVaN3TnbD5XfyJFbu7ZRfu72rSXaIbfJatXVbboWVTjlLee7WZATJY5Q7Bg0FFI0WcoLemdOiTdLK5cPohbX3Snr+daUgP3QHaQai7yx7sm7S1a3V9Pfr9DfikB4Pbj

5tNiXQ/QFAhwoDwAePVuYTrsF6BPTkb5nR5xcPdEMnvXSBFQKYoyIA0BLmii6CPbF60jcR751UwD0Vil731dCbLPTR6svVuMf1i569oaV7dsQD6XvW97TzB96pLiF731So6IupF731WZVsfUD7rAGfkwfX87CPRp7KOjD7TvcG9zvRU9Lvbr9rvUZ6yPWZ7UvRZ6MvTm6UfTDNmAOj63PaTbq5RRTPPTmyxud5bsFXsywsTksKfQgBXvYF7ePeHl

8fVxVCfeuLifSUtSfeuLyfc97KfSD6afTF6ATVD7ZciR6ufc01zPVAAqPds7kfZ7NsvUqBhfZj6jccPzYrUaNjXDwApIFjqeAPgB7gOyKbYCFpZgIVb2vXtINbVea13Vna5XZtZn3uIqKmT+S93bErA7Ie7zbT1bobX1atDQQK2CPgdhrZh8niEkBRSM212ySmT6dV+Au7aAo7XX7ajvQTbzqbdjlfVuYfABhBCFiFBHAKcoCAEbQG/W3ZdZr+B1

QoTAjaFp16ZnAyH7QSdsCgF7YNYbz9fYr6m3eUMxoJx67qXX7TzB36m/ZwAW/ReA2/Yv6u/WnlmAL37+/RDNB/QJNh/dcJR/e+rx/YD7J/b8rp/XEhS3Vnzy3byzPqVL6C+TrS/PnrSAmfP77cuv7WWs378VM5x2/b3lCFt36t/dkA+/Ro9lHgzMlaV4iD/bsUUthUsm3Sf6cfef7Typf7Kia77xhW5dsAOCBMAG6Bv0EtJH2eO6rtZO6lBUbY7k

NYCVsC8x3xoODr8Z17DBclh4gNgBXMpnb+DTrbBDbnb9bSIb7WQn6XzY/pk/ZDapvTDbidc91Z5gkK+2MlZo4Wzj73e3aCqG8RYdjzjuOf2Sxmanqq/U67I1b+73eW7VRBeILYBXd75Aoh6omqxwC7FJEggPE0wQPoGXdqxwRWj+VflTSdYvfCaijRlAbuQFTxyUm770Rd61mmoH2BeMAGKroGmAIqB8AIYGvA0EA5NUmgzA7lrj/aBLUnHkabA9

Zy7A1f67xTf6jpXf6RnT56xnQ96l2gUBPA8YHfA8YGAgxugggzAHQg8s5wgymbrxctsQaTFaUA29i0lXfKMlQ/L8BShJa2cVwbHuixV6YSjH8ObYLbN7icxLvqBFREJO2bbLiedOCmA2N487c7L1KYTKOA9WNx2amw2ZdTA67QcSRrSsElNAah0CSRdq+HUCvgs8hu2BX7FZTnLChZYqDrWQaj2X7AioKeyg4CHAeEHZhr2RVhb2d8h72SnA0JM+

zvkDnA32WhIP2cdQv2eXBf2bKA7hFUAjA94GQOf6SUgHaA2wJoAhAAiDnFZO6nqg6Mr8BiLG2l444HIBcw/au7CeRazuqcTSHZcwHFXdcKRg7ndE/cdZ4lRqLElfIq6ZTfLTJcorMlSzLnurrFY5VsxJZGMJPPEGy3EMP5D4GMhvbQtb9veZaEFNUrFA4OrlAym7RygiCmiYtIjAZoHozuFjcQDmbAiiMDGPvcITwrUsSPXsDJQy3kdygjx+wPpr

UoLhrOlkhK8tfKHZgYqFUABr6DOagDWoAgyOAOKGoigqHdQzKHNclp6lAzp6/3WEt+QwYCjAX3UJQzqHWKpBKVQ9CAuVRqHoVdertQwcD7hPqGujr2YjQ+57xfW88vPd4zG9bsyhhVGC/KiaHajS6H/Q96FZQ4z6/Q1KGlQ0OhVQ16GUw2+rNfWe1J8uaGAw6V75zCGGPpSy73fer5XCHgBeYLUwwQ4mjLfjsNmGv0kREoRBENrvSLzQiHhieayM

ZVODX4fK70Q9u7MQwXb/raq6ZFb7L8Q3IraZUHLrbV0hQ5ZUGLJVkqKQ3CKOmQu717NSHhMj9E0DJWUdvQArkDdqSwpRYrVpasJHA/UrWfWs16TeOBxXuL5xrhpts9Qh6TMVE11RlM6AOo2r1RqgD63fGcdCryNRIH7xlQbc9u3Tp9dsSeG/VSOqLw1eHxgMDcnw4vb51W+H5zB+H0hl+Gd+upBfw3SN/w1yNogygqPPeGHJfQkGowzW7xnfjUoI

9F87NbBH9zPBGVRvxVvw7fAqjqhGUXl30zOcy7dycPrvpaQBv0BzrCAO0AGgA6BwQEYBywMaBCoLgBG9N+gsLbLbSvAu5dpHNAGwyJZVbWJLOwwTzuw7K7GA/2GBvbH6ftdVKb9TiG1vPgA/wKK9hoLUBpgPGALFLUBmAJeGeQG5QAIAYseAA1ojAAeB6ACCBiAFviLccfwSZMwB4wFGBnAA94mgIyAz8nkr9wGDjjwJ55q8d/KmQS4pLbMzqlrc

a5TgO0xvQEW0EdfV7trfgbqlHxy0+MlMfiJFK9/NjEooykAYo9UAwyVwLiuOAb80JwMRCGRh3cYOCbIHNBhKSrqsIBehXFlsEtmF2xtdaJLhqnJHuvfQHtbX17KrRKKHzaiECZdiGxg8dYfADpGTtVJB9I4ZGKAMZHTI+ZHI2N2irI6QAbIzwA7Iw5GIPYsR9AC5G3Ix5H7PF5HOwLHLrgNpbdsI3dvuhQdPBL/xE5fNa9wyAyDvZjxUo/qAs9et

L7w1UBgFtSIDAPocC4Kcphlk9GPvq9GeOJJwnLU4Cy3autq9RbcMFfXrmhY3LWI+xHOI9xHeI/xGogEJGRI+FbXOJ9GXowlB3o2WGmI6y7jXG5Q56R3qCKLUAUgDkDjgEYA7QIm94gBBzRIwVG3wBJGPgmebEZVIwD9XrrBGdK6kQz2G4oTmSVI3JKhvYbb2A/VbH9ENHdI6NGDI0ZGTI12rpo5ZHrI7ZH7I45HVo+tH3I55HvIxzb1LSuGHSgnL

LgEuzaUqY1xAyDl7ShjzpAzkKUDRFG1+I6AfrI4QEANaJOBXgbugPiRd2SRwbo9X6jrW9jjY4yBTY+bGpBcthzBhNQJgMcN22LTRUgD/spgLcgTkO0GpGMjK2qU9r3rcIqo/XTpwAhiHfrfjLLdaN7sAtpGBY2NHhY1NGDgBZGB0nNGFo0tHpY85HjIxtH5YyCAbcbMHc/ZFYA+FMhdg70yPvLa66gVrgEOKw0k9aZa2Q8lHbY3Yxbo/Yb/RbPaF

ighYs6JVrUJQWDBClyNnnffcpcmEawKhGBm6V9B+45zcbTbvQyfigVJ43XSIkDPGBCgf13AAgApIHEgNkVIiGzVOaVFBvGt4wz0ryv2LdQjVgCCrrlHoIghD7Y9B1IOfGMIJfG8QIggT8mAhlQrdLWZuEB9nuOatCBHYRAGIAl46p1+492Le41oRV4wWCvndgDXlMPGmCnPGbneEbBzFPHcgGAm1HiAneGAvGL/ognrfdhqvHePlD49vGQkbvGN4

4wo8E8fGi9afHQwvfHSDLrBr44/H5QqJBKE7Qnn48gBX40VAIwB/HLaLAnQ8MIBRABcZME0AnRfRXrYg6JqqbeJqH/d3EIANjGEALjHnQPjHCY8THSY+THEY6T5UEyvHsE+AmIE72ZFzNAmyCpwm1Oj3y+E2onZcronIfovGDE9iqB4/fc945vH8ExkjaTQQBi3lYmj47/UT41rkz427lGEzDAb41Ij6E+4nqEzDAX47rA342wn58p/H3TfpBuE/

/GzExyrGIyxTmI9Dy2AHa02zkIB+IxQBRwJgB4wLQqEKA7BvIOMAKY4TFxI57HQcgjLlYa1HmYwpH13R9rN3QOGj5XH7Qyn1HA/sbb+YyNHU4xNGRY2ZGM4zNHg5dnHJY8tGnI2tGC43LGto95Gp5aXGlUatolBRuz1Yx+A9LR44BVr9gYJOFHugOF4qgJOBpgNB6DgN+h1xJHakvIlGZTK3H7Ae3H7Y2wY3Lqsn1k5snYOR7H2JCWw0o9wrGLUa

hRCDlLL8e2GvmaUmipapIuLXbKsZV9buo0MHzdbVaeY40nk480mhY60n045nHZoxLHFo1LGVo/nHXI4MnWpttGGGUrG5gwxzmoMeAayMiKZkzHEazOW0woy+6yPr7atg23GksB3GbLV3G/XegBHLebV+bJFbS9VXLBE4DHKbRGGO4mDGfPc3hEk8ZyUk2kmMk/GAskwcAck8abKU29KYkwub+3f6SqxOCA5or7B2gNlw7KFABEKsaFjgMoA7QFPK

z4ill8k1THPY/Q1lbXdq6Y7JHKA8cKDdU/DI40pHo/f49BvbUnpRQnHNI0nHho3pGQU5NHRYx0nxY/NGek3nH+k3CnNowinvI3V767cOiAbESijgG+ATDStoG5E69vwdBT0U24tFk3eo1+Awq77GwKaLBbGSvDtbs5cSmbk3sH6RdFL/SfGmowImnx4rgHjelKpKYnNAM0+4ruGo+Y4ESppvgU9aL3iaymY28nfEhHHRRfvK0Q4MGWAwJbQJiOGS

yRAAmk/anxo46n2k+Cmuk5Cnc4zCmPU4XGhkyCBzHjn6xkwch+Eg201URsG6geYNgwH0pNg1dGUo4cn0o2Sz1MiTaC9funK5ZnyYg4ymhnfEGufKym8Iy64nCFKnpBLKm0igqmPNMqmp5Tz9EPLSm5zc9jRU3EmR5c0A/eKKZLIMfwAgomBywFPBsgUIAfYbFFeLJGSCk1cmqXrTGm5Pwq74Q2nj9camW09nauoxzHL9fJL44xpGBozN6JAN0moU

70mZYwMmvU9hdto/bbxZL2IBZWqjYjEMphEmJJVghun2QyLSpgNunSDVmnsYmm6vXcaBeqoWnoMykzYM1JH1eMrB5qB/IqJucAgMhK6609vLEQ+UnI/aano4z4CLU2pHCyd2mGUvhnTAqOnoU30nZY2RmUPttGqLYa7/UwWY7Vr/w7RSyCMpqiLKymzk5rZzkCU2+6MEemnSU4HaShWOS3jlWdONQBHu+uhGwuUuc/vlRHfMwInlcZ3TBnW6jz05

GHpfb4yYw2FjyI4cpAs3G6RU+Wy+bdDzGwCTJfInmmLk2+dZ5Qhyk8LqA5YUKQ6XltRJsc1HYiCu6uw6cKPrZUn+vZtVOY5anlXQCnIyg/Sm9dpniM7CnJ096mQQIRQqQxnrueH9xFSVin2yFwy7UB2Qm473aW42mmDkySmjk93HG6TVRKtVEnWtdx8s6L/GeE5gnVs//Hls+PHdKpgn3EbpV4Af9S0E598UCpgmTExf8tszVQ949vll4wwx0E4S

dDs5J8bs1wm/4xcYLsxEguGH3HsNf/96TnspHEzYnmEVDEiEwfH7ExF8HPlgnsVQlzSAOQB1rp9nsVcPUjoaDmIkBtni3n8Joc64dkc/Dmd+pQxxPkF89s3rJ5OVDnbaP2dPjfFmsc4jnbs8dmEJQTm0c2iI7s5jmFod9nvBgwm/E/xg/vrQn1ILQnZ/YAn4VYtmewm9mvoMjn1sxEnXs/F87qdtn8c6p08czHJRc3ti7s6dm7swdm8GCtn3ANdn

Zc5Tn7s0rma6ELmXs4rmAmVoQPs6Amvs6eEfs3YnggE4md4/KFAc/YniEyDmZc3p8Fs9Tmic7DmAVfTnnoeTnns2tnHczDmf48LnXczEN3c3onP7ZDmac+LmyqqTmGc3bmgvmdmUc4Tnvc0dmAVVpBw827njc0znfE1fHPE74N2c6JBOc6GHQs7XKcI956r0756NyQ9np4zzmns+DmOVfzmPc2IBtczwndc84zQ834VrefAmG82Ln48ydmK89Hm2

86p0rs/ibac+rnq89nQ685tmZc1nQDc/NmjcxwApfjio/swz1bE1bn946bmNczjmMvg7nfhLHnXDpVq/c9jmEvkF9Bc17n0c77mk8/7nI8xl8pc34VUc07mm83pVvM7vmVPlHm5c4fmB8wnmd84zmWRszn086znM87rAOc7rAyvSPz1td1ROGFV6YAA1pyqfxnpBR4JdpGFoOyJzwdtPwlIsNVdpM5lND9frrH4XSjuLaiHsZZhnB2fVnhvaMHeY

+MH6ZIRmx07pnSM0XHnQDMGS2iuGjmMSw5se2TnqZa6TdNLha47t6HMxNmnM1Nmy09GzIGe0q8FXspMNXArLCgQrQVbnnK9fnnK3bmyxE5JrObS6IBC6IWyRklm1tRV6R5a4QmLDyAJQHAAfLpAXlsE0GGGg6NRYF4rg4mSSCsRhy8mfWmCmWjKWY4pHOo7rbqkypmd3WpmVXX/Dms+3BSCzpmSM56nKC7niaCyimrpLrpS5Ahb2yULKNvR455CK

YXx0T3bWQ45TN085mZsxSn5DTXQ5cwQBY+sP91E1zcX8ydnDulEB7+hkW2llkWEJeqrUi4cI2zkCA8i2vHCi/cJ1VTkXcAGUXBJisV3aFUXWxnSNw6HSMSi3UW0ioOYKQNDmiek0Xki+rmWi0MXai/UWIwD0XbaA3Tmix0WKi+An+ix3mEJbUWZi7MWOAM0Xii3ZRSi10WlizQ9Vi0xqRi5sXGiysWBiwnmhi20XUAB0XRi+MWYAH0XDi/MXqiwv

RWi/cXOjU7sLi7HmMI2TasI7DCpC/f7p8Y/7QsW6Spi+sX0i/kW5i/PHBi4sX8i5jcdi/Nrzi/sWdEzcXQS8cWai88XYS3CWoS4CAHi0wx2i4CWXi70WCi0cWTs6cXTi3sWgQGMXXi8oW+3d+n4rXAApIK0BoIPGBcKFlndpBeSdUy+A5BULlL8ezTjwEsHSs6H6DU0fqThSfqTU3YWBg7VmsM1zGsQw0mms3eJ3C61n3U3pnKC6nTkU2XHsHIrB

2aGiyQ0+jbsMh/I7kLtTzo0hbOC5Yz4izumq6e5mAS2kX4Ac0WRi5MX8SwhLzi6PRLS88W3i2L688xL6vi7hGos9GHm8NsCzS6UX7SzaX7hFaXyS+V6UsyPKPC21mJ0/Cn7gXUH8ky8CBLM4AmvVb8dkKebJKfn7ys98gegx8m+g32GzUzLFfk4+a9LI1nXC9KXOlJOzto0549DWzTT3KhxdGSu5jUF/LNS++d9wEkApkMxn9k2xnps8aWxQseyj

g2ezg4Beyzg5NALgz6wrgx/gbgzgHNMPcGP8I8Hc4M8HC4K8Hv2e8Hdk+Zp/2fXAJAIGHf6tMs/g99LPxLVoWEjbTywMcBA9YbkOAPnZJAEYA/QC2DeJeqnLHpO7hlMH69U1KRb4kfxJLHyX0C2ViEMnCBXtdMAKoopnpYsySpRQ1mRvTamnYJ3ZiPCvzRwG4oGgNMAxAH8IQMHZZftDwh4gJIBRwIFFvAprF4gICIUgHK9ywLUBMAEoltiDOGWM

oAiPYVVDacdtHuusZnP6VEZo+D1NXiJ55C/WEWY4ncgGTOw0WQxdHcRYbH4ozfYH0pOU3NPRVuBSxn7icMosbRlM7o7ZazNDFLaKnxXZyqeSEON/su2JNUd6ZTFhKzTE+rKBlLAURAJkPb0EdOoTgMooYDMmuzXkyhnMC58n+g8pHGHB2marYJaXC5ySj0KBXpbWDrIK9BXsGt4RywPBWmUEhWUK5GBQyZgAMK1c5sK7hX8K+VDiK4UDgEaWXvIy

XElS2MmJqq44zXSRd7Sj9FDUNBJgi9EX2K45nLGSBD+QQNDSWSaXxcmo7RWsMt8q7eVdMjXEDK8k8huYdKgYzpwQY55bL0x6WiQjjIOAHuWnhYeWEePDzTy+eXLyzFm3SUVWWKkGXAC6oWtev3BiAMJo8dWZZtC60BuINBwuLM1AXQHkmby0yX3oq/Fny/fF3jCH7xLG/FVkOvo0C8hmBS6hmsC/bKcC6KW8C6pmJS0uDRqdOR7K+BWnKzBXXK+5

XKcJ5XUKz5W/K1hW4ADhW8K7gACK5pnB0u7DQq+SDwq6CKEhWum4di3bOksfZGK53N7HhUh2NjGmM5Nsn90lUAjAJIBwQO0BuIFQZU0xgjMq9gjOy1RI3LkjWUa2jXIq/DXoppdarkNYkQ9FkQZNG/wHElXinEm6MMOfpXn6DypddSjKSMW9aevVVmeLTVnc3JZX87dfqpFUXbLq8L5rqy9lnK7BW3K2RgPK8hWnq+hXMKwFWPq19W3C0RWqAtKk

/q0XGdC5RXRpVKQoHHLAelJsFUplrG1SKdHG2nrHk9bEXBK71CBOdlWY2YTaVktSnHkiVX9MkzXQ9BVWlTWenN1hriLMjIXTksNXRqwfwjABNWpq/ZYveeMA5q0onEPP1W3ffKz71LF4aLI4ATeilwO7PGAmgCHYIwDyBvUmpary4Gk+ureWEy4voHy5eRRVkhmrC8ZXysSiHDq98ncC5YLsM8OGbKxdXmOMLXHK6LXbq3BXJaw9Xpa95XZa/5W3

q4FXPq8FWVa0Tlq2VOmNFUa7UqAHxEpsgWSLkJYS/a+BzgBqlO7ntS+yQdS5A5umsa4JzBobjW3sVCl9AJMAcFJMAMxU0BYslAgbgUi1l9esKs61BnpBbvr8UXHaJCOtWny5TWTGexaeDdYX5MwwHhS+ZWY/XVnTq7XXCy7ZWXSq0AN8NPBjQIeXnAMvhjgGgHiAOMApINBASxFLWvK2hXfK3LWe6wrX+61dEbwZo0DM95GkytmMVw2DibGDlKsq

BqXqBX2IBKQHxTa83Gq+mLL8o0wLjHtMBogGZGWxQJXko2vXra3wWJK/6S9gAw32gEw23YzaNSa9YxbYKpXv0gjpf0hegRrABlFSFJmUy4zWibMzXXk+zX2o717qsxhmLK7HGDbThmBa4nHzkIA3Hov4FQG+A3IG9A3YG7kn26wg3nq8g33q0FWrwSFWyQUXGaDX6mqK/Y5y2ilYxA9XHFYiIHDa0Za+lLNR5MewW0KQaWT3FbWEiw9GJALjY4CP

DNwm13D5RKVXna+VWAYy6j3a159aqzTai81pgN+LvWd6wfWj6zSAT61JAz64KmNMpHWygzFLxgIyBjQO0BiZNBA2AOgb4gKOBlAHahXCAcB6AJPYjM7QbcrVfXwQ6kLy09hgI0jfhgMslFn6y9bX65VmhSyo37C1XWAKwQX+o0QXamc6zULvfTiy6KS5vaHrKYNtGV9Y42ta5QhUsPXGBZIqSGK6YaY4l3aASLNQ2K/qXza22XwGbwWpmRw3vpay

48mD5ooACCBJAFGBLuqOAuDFd0XmzZH5q51Y0sgEruFTxR4NC48PGFwaZM4VLS66MTy618mqk5M2qpc4W/6xdXLCbyTrCfyTZvsASusQ4TT3fpT5YzngwDV8DVSeDWk5Uk9ziZWZD7G0kzm3t6Lm4QbIiTjWbFTFLkwIkADhNMA4AFGBNADtBjQMGw7QEYAmgE2BoaRQYfm20TOVrsAUeQ/h768tXH69tXGYyXW9q3Si4QBfzYEDvhfyzAcVKWKX

8C/C2gKwNGeEMuAIIyZH4wCdFoIKcA/wMQtkwAMAWBacAZ09H9bCZ1jeMZi2dXU0zq7QJovI/gAEaZrWNLRtga2vzAdw+42F0iETDFV54G+k/Xdw+c2ofChaUSRO0p2lMBk0+DEMa43iaWxxm7wwyL/SSh00OpG2+GyvSL0HtGmQdxI65OVGm5l+k3wHTFBwe2wKkJA5ksKsAsHBWw9K24lna9Xiug+H6bCxUmua6o2ea+o3WA/8mNW7M2tW8wAd

W+OA9W2odDW8a3TW3aBzW6i3hUXYSMW8KTCK8s3YbUMn8ANNpZ01oqRYBkzl01PWIMoc32yJUgG7scM9S5S2haVdHdSS3iN6+dTd2qR1D2sMsT29F0qZo7XCbBTWBhAk2gScaIa9fKjhnRenUm/VXmbIy260iy22W0TBOW9y3eW46B+W+HWJABe2yOle30Y7EnMY2vxkwBwAFoOCA7QDAB9XvtFywMwA1zT9iKmxBTIM9DLfm9fFE9eK3RDEd986

1GlV9HDpJW6zXdq0amTK1mXKMTmX/y3C2zq9jj/64DBu23nJe2/q2B2wvkh2yO3ViWi3x2za3J299XNDYnTwq9pG8lZfC12aEWiW4rFCW8FHQQhUh/5alXg21P4lk7Q3iQFG8GgCwlxwNBAo23ukY25WoD2yE3E299LZAH+ANO9UAtO7IT+RZsMhhEI3b23YlpYZIZaazIZ6a1W3QMjW2Wa2HGFrO8nm0wdXoW9zWY44OG446IbFJdo2AQCx3dW+

x2jW5x2+DsO2LW5xirWxsTU8bwGQLeRnGQPgBtOxWWOhAqp/+MFCsqAbWIa8ZABMu4JgehS2OC1S2a/gZ3aW0rzajBE2r5jV3omwTYNksTZXa2gqkm6Csvaz8XxEzB24Owh2kOxGAUO2h22ABh3WEq+mFkg+4B9SITyw9HXLNMcAOOJ98eQDGBAddPy/klGAA62MRLxgK2eFvxYGBp0J7y2JYpgLxQ+KLGli6+HGOa2M2m2xM3jq9XXxS7/WO28b

btW6x2+2wa2ouya2Yu9x3ACQKT0W/x208YJ3dXQ63CaE62YUlFWtFScBsINTRfW2zikbWyYp9GK6uOfrGvJZxXLNHAA2ACkBJAPs52gHm14a3p3GqJV342/dGjO9Dzke6j30e+SCaGyXJuBt9lFTDsN8s2lQjgAoSxXdjzRxOx53fF/4dq9K3KO2XXMZWZXaOyq2Tq04WGO8WSmOw92Iu/22Xu1x24u+UAScXx3Nien7hOzQInW8uH/C7ZB+BF1M

1UYVR6M3IRbfsmTWy9S26/o8TM0wm2tA8omtzOiUIzZ+jfirx1x/qcbQvp5iLezQSyitb2HGa6aQjRsUZ3hCUAfgAnJdgJBsAFfnZ8pwB20JNCNipb3fahGBXPVoR7NqeZz0WH2aqPZso++l8wcxkVrmNzNTzJ6T1IF72soD73Y8+xwA+zdCg+/b2x1NH2IkBH2q8p5jC+19BY+6X34+xEhE+8ORk+9CrJzcW95IMAmTewiV4vTF8oMXb2BCROZk

jbb28+wITLzMYnHe28bXezm93exL9Pe1KJve773s+w+RA+/OLg+1wDQ+3tDw+4hKS+1Biy+7kAK++v2q+19Aa+56w6+9oUGfhP3sgBn3p+/73Z+7n35+/n2l+1nRi+6x9K+7f38zhZj7PtX3T8kn2vDQ331IE32f0a0HfnFORnSxIXXSy+3Is97WenBABZuzSB5u4t3+4Mt38AKt2p4NtEHG6N2e4y33A0b8Vz0Z33EiVb30B732r+/33Hey6bzT

dnRHe273yxR720+5P3T+1n3z+15A5+232F+xQSN+4vQe+9v3H+7qE4+3p89+4LQD++HlU+6JB0++YAz+/hUL+6/VsLNf2mB3f3BPkkSJB0/3K+5wO3+7X2P+5Kal89/3iFePSVCyGWtelJAGgBwBTgCCxwQMwBoIBGAhAN2BwETMR2gNUAnNJt2yvpysEyXt2puhtWVq5RgyO553NbUo3Oa9gXK61d2pm+q3CC8bafsVvHxgIqBHpcwArcQPZWgB

+APYEI5R2+sSU8aAS7W0Ab/u0OAnWxTLge5Aia2tBwBNjnTKEOk9rM/BgvwAwXFO7u3lO7Gn71N+gYsjbQ3QPzAdO7zqly9Ya42wb38e9mnvpeUPywJUPqh2m3OYB+k/ePm34dAp3Ey5XJxG/+kxJFI3F0jI3q23I3DK6+WG04o2I/e/XxmyKWW24F2NG8F3C7aF3IAAEOYAEEOEACEOwh+OAIh4VZaA5wtLW593pe0l3Ze56zo1E62yRQu3IESN

m5SFRM9srkOGyygRvHFMBKLvimAm+V3Y23r29Sa5nd0x5l7a0oFr2013S0FyyPixW7gByym326AOZENoPdB/oPDB8YPTBx+L6ABYOrB8B2hU8U3h5Vr1EseFk2AN+hGQKqmeXQfjPgrfXBLPsBChZFIjhgMSjKzK3Oe72GaO0pm6O8Czpm5KWe0ybDEQciDLYbuD9wZAACobiDioeeCXYcHKq2beDvU/gBN3q62OmRFhFVKhgi/ijbnJaRwEDM1B

Su18O92xbXeQQJzBwWJXyU6E3xyXCVWONF8Uvsj8G8kRTS8kaOVfdJ9sIaaPmIuIWhE0ymC8yAPOu7IWcFYWy1aIaOk0MaObR7tCzRxB2v01B371GBGoQBBHLOzvzqI8JmZqPzBKuCbpFVFwNGXgzHyO+z2MCwyO2Yx+TvB/R3bu34PRw5yOzYdyPcodbD8obbDsQYKPHYSVCiQZeDu0WKPMG2RW0u8cPpR0r3dWezTi/qDYz8RzjNcGwEaDmbWN

Ryw3E4TqPO41FKRQ/RSm7EhGMqXC7rNgP1kBqxwxauhCHQZpDjdtOOfObOOswXJCFx6F031lzn1RhANxxwmCzQWuPOLgA11ISuPp+lOP1x/ZdaluomlQfuO6kbLUnSwynEm+FmPa9IWXRxNy5C04yPR3RHXlGOON0Nlt09teOZx0ePtiuf1/x0uPAJysUrx3ANFxxuPsR+DSR5cYofIlAA/wFsOwxyFpNUwxbdgCJT7voHDfFboKOw9MPkx++Wie

VC3ue8yPee9d21WwL3r6TBNcx9lDtwWiDeRzbC7YaWOzwc7DKx6KPfq3Y3Z26sNbh1OlRhIWZsIOrHjts5Ld4P91HGp8P8WYE3UvNqPDO0b3MfrGcvM56ON0HHVvlDkjZclqFFJzeOYIsj8UviZccemectMvIX1ufGcNJ8pP+6KpPbwupOLR+uO5etpPsIbpOWlvpO6U8enMI2GHPi1COgse+3i84ZP/Mz0MrJ0pOYmipPhILpENJzLUtJ7tCdJ7

zdZeo5OP06viKS4GPLNC5pGQHXZrtDbiye3xYDu+62mQc2xPWzwoBSFWYEgFoLYKT2JHvmg5mGkH76TG2HaR/hPTu+4Pzu54OYWxmPWR74OZm8baaJ+bCcofRO8oTwgBR0VCyx8KO2J1O23YbY2h6xKO+Mw2Pc/e62HSrk9Wx8Zb124V3SSOgZvWjr3Ma32OZJyKG4s5RC7CowopOX5nPM3985uc6C7xyFnAB9hG3S4XnPJ0kHVxvJP9p5Fz5udF

yYJ1DyR5X+BU0NBBHQAgAowE5Q8ZPQAUMeCkswNaI2mxdq+JfwZ4gkJLL8JllI0ina+m/hg6Rxz3IW1z3sy6ROJGeROf62wG7uzmP4QVyOLYQWO+RxAAepw7CWJxWO0G7UkSK2FX5e2l2YOZl2AbHIQykH55JrQ6Kv5Las/5TAad22V2ex9nK16/2OyU0Ha3Li1plGNBAaQMcAKAFEzEeKvgUnDSAGgBQAJe4DPryzh2ButPWWS3fWC64JZ8Oy+W

hm1K6Rm4KW0M1HG/y2ROfB5RO7hTwgIwOFFFo43orgM4Bj0ovqDgDABXCIyADgDyAxpweDix0eD8Z07DCZzY2B6xo1PYal2UwLtHS2JzimDYgS8IGXiPHG7SocYgag28UO9XIj20p8a4a3pG8vLNegCDStPpJ1V3jk29i453W9ZCasg4Nju5UCR6ceptHNRVAR2+h4joRLOMgX4EKQbgAmwUdq528dO52YZymO4Z4yP2Y2o3lh223rKwi3MrkbPq

gCbOc/PEBzZ4mghAFbObZ3bOHZ/yOnZ/bCTwQTOyoe7P0G0Aj/q2TP8AIrHxp2Mn9wOaZlVIHPrrV43upslgP5eJOeOQU8Mq6tPU57Nni4kW9z22m8k3iCPa4mCOWuxTaGhR5amhTCOXx2APeZ6ksBZ0LP6mOWBRZ3sJxZ5LPCm2fOr5/6Pks2KnvpTbPOAC+i4eOMAUa5U34gALOOALlyeABAWL69h3BWwbZp9NGl59GtWlZ44OJWw3PCJ8iH4Z

0yOdZ0jO9Z1mOWp6OHu573OzZxbOh59bPbZ/bPGJyWPep9PORR4NOfq8NPxR97OS434Wy45nEVgLAi7SsgSfgqIQe5vvPZA9IkVO7Il2Vj6x70PGBJgMmA0U3+AUlYnOko+zPj53j3xK2qYYpfIvFF8GBlF5Z2KeymjbO5gvq8dTXjsE52eeC53ADrI3lDB538mT4lU3Gd2tZ0q3lFpVKmp5o2xDYLWqF7sY+5wPPLZ/QvR50wvnZ1PPXZzPOqxx

xORp97ORk7wvoq8FD3BAtB1YwnF6y9QLJM1BJDwHD3uxyXTV6xovGh1ov9RxAB6u1ZiajHbWHUY12b5y7X72xgzHx8k2n51041TeAuOAJAulbDAvnQHAvZu4gvkF91WubeN2B5SEzSgziPuqEYBsANnIDfo6Bh3foxmiXc0zut4A6A+02gZ+Pp/LobZsF03IoZ9DPqp6VbnF752SJyQvbWWHT+XnXWu58bPfFzQvB58POGF2PPcZxPPmJ2Eu2F99

Xqx17OsG0yAga5Fge2BD2QizWXnXg8muhIHDMl5Q2Q29HOSR8tb24NgBXCDwAc7GwBW0Nj3JAinPNF3qOCeyPLQV+Cu2FrXrdC7nS3nJNKaqXcAV5T9gepk44OTLhOb4Ldb3fFx4pWzVO5hx1GFh5/XzU6pH+e+Qv2R0x2fF6bP+57Qvzl0Euix0xOWF7cuBp/cvIl1wunl76nRk1oq9o5wyXFOrHFCMHP97HeSpZHs3/GxJPvh0E2ugZzP/h7lX

1MvQPW+y2bR+4maCjRKapTZ0aXCs33sLBqvRjVqvj/ndPETeObP+6JBVB0en7ZL/2LbP/37xw+22u9gyWhcMvRl25WJl64A36QcIZl3AA5l16XXjuqu0ByP3W3gibW8rquVBwavgFxoPQF9DyrcSeWaQHCxNfGkt4wABhRAN+gUgMoA8ZNYOUNlBtIx8JKxW6Yu4tPgv5KQpmP6zz3SF5mPUZ9mOe09BBHIPDT0wLWJwQIQBi0tALiAIkBHQH8JL

l3jPQl+WPwl+xPOFzWOCBU62pZ0KvIEZfidmAY19dCsAOcVPoLTF2P/lyUO4a1xW5/PegUMViQe5/gBz0tCuzgrCu8l/Cvmh9DyN164Qt1w2S0V/qYKvOvTi5/1ZS508yZSJDiRh2Itnk1zE3O5MPa2yd2vO02mtl6ZWEZyQv3F3az227WumO/Wvq0DAU0LaNFW18XZ6HZ2vu18EvJ50KPWJ0TPVa5xOJR5Rmuh+wMhCEku4dobpXJaMhGaMtOj5

/uvBBYda7LZplil4COyl+Hgna5MP4m9f7T0zUv2u15bYRw5h2KV0xk11vgDI+mv/Ilmuc15iOimzGu4pxWH71K/93/vFwl6eCHmSz03wDSFc+ee5Qa2PzA/qG14i62z3yVw23y11SvK13sur9QcvO59kxeV0OvHl7WP8ABs3x1y0lpNH2JPwFMnBszagRCCSTN2XKuD51nLk50qu1p3G82+8WyF/b/7dZp/7W/fgALZuGFvN6y1//b36ojcuOyCn

kjIhm8J26K+gyziUcmIYFvG/Rv6e/YAHlKrsolovS6KhpMdUAAxUGKupD3/aHUQt6lvHlrspzIcqECTjlu8txePNzAVuT8kVu7yspUhAOlvytyedKt/lugt4VvN/b37Gt6VuD+JluphpZFNcpFvRhpi7UxaU7e0JML/QpSIOjYnVTkSNuSne2viABf6NPYaum4RLmvN0luP/cv6v/W37OlolvO/cFuut4AGwt2BO/csNvJWcwAYt3AA4tyIcEt7V

vkAPVv+zlbRmt/1vst7lv2t5tvOtyluGtyVvcgGVvXtwkM2t9VvezPdvHtz1vcgBluKt+9vgd9cJQd0duft+I8mt39u+t2WcyVudvinVi66HRNuMIOJwZt34iuKqyzRt9i7e0Mtu8oMFnyKS6XTp+5P8+S/Pos4GupkTlrPN2/6Ot6ydtt35uAt3Dvvt7XzvjX6bTt1bl0d6nkrtzduhDnduWdw9v4d09usVJDvWt9DuyCpzuAAwjuFar1uWt1lv

Ad7LucCvLvut48skd+EEVd1MMgd3Luxd2Dvtd8ruAd0Oc0d3NvqHQtuynZNvcdxbu7kfkird5jvxt0tuEAytvBN8GW41yPLJR3ABQRZmuHZ9PKo7Sb1c67lnZThehGOfcBrXV7iNCRYXZMxVnNZ9su/18q2q1x4v6V+dXLJAZuPZxg2jNyOu0u+dqzN1n0BFztQDgq2OPhyX610gLlT4IRv9OyIkF3Iay4V4OP3NzlqMqoC7kHUC60nUCbEnVE6U

nSv0YXW33IzZE7tcmFSsnS/1dE6fabHUc62+/lVncps79iromlHhPuHnVPvWmjPvXnc6bVHoWKr7ZY6ELFp1V92c6PN76baRC/aLt2ZVtV1tPRIMhUmCqtu48i3uone3v2933ucteC6H+j3vcHaC6ctQPuAXYv1h90rVR99/HxHjcV7nbvbl99I9N92vuVEwc7AD1vuiBwvvTnXY7/93GIZ95PuctbvuwD/vvGd4fv3hMfv/HafuzV3NzL92QV7R

wxuPqU+Pvi/3SvJxuZm94PvMHffvkHY/uELM/vMHa/uO9/3vQTdlVb90PuEXe/uID8s6oD8ged97o699/Ae2+3I9F98AeUDyvu0D8If196Q8kD0vuJD//ahD1s6D9zzuj99vaT99EMz92qCL9yPHL0R7uBq5oPuqJoAGtDMRMUQWnia+PpWvQw1/mwrOLYFxIxlImSazA49a06gWyV5svapy4uK14jPtNzXWa1xQue05DIUsWYAbaKGSh564RJU2

9XF+RGA2m2qB4gFABJgHaBSADbQUBcaBcAD6BEgGYopIjyAdk8HKjAHRYowOCAcFHIAEj8mBwLBGBXCMoBNABPyA1yJ2ydTxOs+osg+2KkE1UfqyIbM8hjieHOih6zPsl5qPhefyDlV0e2leTQp0qZzNHGeajvKYRTyd+gzhuS6vnxaM6n/SXnsFCMfJj2oOh5bBOtejvXEsfEBdWC63V1w5CrDyoKRW/tAX3PVSGTKawVqC4e623JnRm54fNN94

eeo8Oy9N6eJAj3+Bgj/FVVwAcBwj6hAGPg0Boj0yg2AHEeEj0kfJTdmu0j+0AMjzr5anjkf2F3kfpgAUeij8wASj2UeKj1Ue/QDUfF55nWRpfCK3FId7+YCxySW0D0HFDMAF3YuvxswqupJ65uT54kXS82bRds2MeNsQDSK83CS/o/8T3i65PIRxFnRE7TvPS8gO5s8x8mTyczJuxjHhN5ZpJAGiOeAEYAeAIokwx+CHt9dwrUggVPVqQ4xNtFZ2

KA2rP07RrP9q7+viF8nufDzd2/DwyuLqy8e3j6EfPjxEefj38fKcACf4j4kfkj6Cf0j5keoTwYtYT/CfPvoie7QKUfTLCifqjzi2DXVifaC88hoHCiKyDgc3URTTO6yPb1q9xSf+j25vJaYgzXlnyajaUCOAvgbTZSkmeqN/KaABw6PZj1W6aKZdOUz9LS2TSgyH7QAWo69jEGgHDquJVtES9THOtuwcfmvbwAikzVTXaZ7ThWwmSvaSgWrj/Hut

T9R2W51/XVWyjPdN2jOAj7bTXj/aB3j2EfzT1EeYj83hAT7aeQT6keHT5Cfsj86f8j4Ue3T0ievT5UefT7O3qC7g2le3ahmYt9xOj1J21SM7avl4HwmyO/KozzCvKTw3u3M+SzNc5GJMEzaSC9TSf9PhXnXz7avMz06vqlyQfal6qbEYQsfB6bXSh6YKf5zSAvKS91Q1bG6ArujiDl53sf7cbKfQ954xNsJvT8V0pXLj5+u3BxSvlGxd3FhzSvv6

3SuDT+nuYJsafxz6aevj5EffjzOfrT0Ce7T4ufwT46eVzwOkXT+ufijx6fkT9ue0Tzi2S9QXuPunb9jmK2GWOfSG1cC8Q1kD0Ibz3uu7zwevG93GeDmQq1EzyQy3zwpeY6EpeSz0QeHx/+fe6drT82fxv4z4pfkGSfl0z1Fb+l9RL1j4YppSWvk5+S0TGGXWfZTwWvT8HEAOGclhuGbqjOz9hf622/XKV/hfqV8pnaV0OGSL4x2jT6OeTTx8eqLx

afaL3OfgTykewTxCesj9Cfvq2xeET5ufyj9xf0T1cO0u2GT+LzXcJ2IzQNXCJeNexaYexKSeYi2zOXNzGeqTwUv3z/tigmVznAmZ3yjpxTuTp25POTx5OWN/hH3x3tjTs+BfP05Bf4p/ehmADyBRoweULFDKedhUceXXt/tFSF2QsmYz3cmWmW2o7hePBxXWGp/2e+e4Fehz8BuQr0EeKL+FepzzRf/j9FeGL3FfmL4lelaxIBkrxufOL1ufUTxl

fHW2l2I7f6elezBJzVvhuWOeeeprXcB1kJbBJOyzP1Rz0fex8Rvrm+ArqT6pfh8/LilmVznZmVDfNL86vGN66v5j38XFjyXQYb8czSzyU3K2TyA/wAbllAM4Rxr4JLj8XIKXmatQqkMe9saQtfS170HiJ0nvlFnqeKJ2nvgr5ldyLyEf9r98fpz0debTzFf7T0xflz+delm+gArrxxfPT2le7rzi3qQbEutFba9Gj9LJECTMAIbPXI9gO5fHNxIv

BeRVf7YAMecq7GzxyVEbqWe87ShiW6k2Zge9b8kiND4hUmr9MfKq46Ozp86PyD/mfoGbrfxWabecD9Kz9D2WfUA5oAKGu0BNTaT2gV20T6z9Z2OJOSOHWPAWeh09hHGG4299bHvwW/SOm52mPSmY1PANwWXhz0x2WbxOezT+zfDr1afjrwufTr3zfVz3Cf2L+6eRb96eeL7O2IM/UePunsBdWbiu0WU1GCu7IRelG4t+phHPujyvXej3uzKr/eeA

RwW8B8TO8uc33ee8XDe/z6Nz3Sx1eB6V1fB70viHp6PytetIJHCECkwVwEEruh3ZRwLgBCKNMBoWrmu7FH82ULwIt7Vw6Y9Kypu3Dzhf1N/MPfL1puHj9ErDl2RfQr3tfJz5nfLTzwg6L/OfYr0ueErwXfXT8LeuL2LfZ2/Kicr/hcj8TpW8T4gSTyHXGq5w30KG2Sfyr0RuZLyRv9g0euR5UYpwQCEEatFIJSRYaF4y9ZD/CCkAnr9LPs6wfimS

8mXbD6mpDuxDPJKbgvi5y4OHF+4flrx/gvyz+WvD7sur74BXtr5ldNQJgBJ5TwBwgoPAzyK8fHAJxGaQM4AcZy/fub4xf4r06fWL2ueUrzdfRbzueJR10vNmxpbiDvJ2OBllQXzM5L3BDzAz8FA+yr8uvlkxbSZ7nPcUgHFHA99CfmsEnPY29bB1UinKqrwiuteqA9wHiY/ZCaD3H+DFpB7dTOmrkilh/MI2C2+pWHO6G5eYBiwSUf/tyA3opbF7

NYNl6ffvL3hf6p/53cy7zXhg6sP1MxdWOH1w+eH84A+H/QABH0vzhH5zf6L7nf375I/u0ULfi7z/f5H97PfC/ufc/aqS0jNvS9sp+65p1hAg54fBaBVJerssDfdR3JfShYg9c8fDNen9fOyq3e36Ny6iH5yInQY8/PyD+vxz+Kg+MatKTjQJg/lkCECGgLg+AF6xdohjPegC8a4mgMjXhH84BJoujJnQJMBlAAy3OXdkBTGzlaFl4Q/cO1fhi19V

HFYsz3fnEpuGuA/WqH1TfMyzTedT3TeWH2yPSL6eI0n60BuH8wrMn+WB+HxxHcnyI+c72/febx/epH4XeZHyXf0rzi39iZLfIEVa7moPdgZO8yZU1D9ETsFzBdS/ZmAb1HOpF8CuqgLUAQQMjJsDdSIah7ha6h8BDcl/A/OM25dyX5S/iwPO2L1ztIP0kagbEiR3X4PYkLF6dhnOxJSeSyBk65++v7F5YXHFyT0Ynyte/O822AuzUnBzx3OU76k+

KmOk/gX1k+cn0I/IX1zeTr0U+WLyU/pH9dfEX7/eJR+euV58KucIKkEGQUkvJ69vOHh1WX2n49oOZ7GfShUUuqCRHWHnrE3aN8M+T01pfR76+36l5sCIADs/wQHs+Dn5U3jn6c/Ej+c/Cm0UuYpybShN9N370FA9agMoBoIOBZqgA0A3QNgBPwIHBO4IxA/QHUeNhVc/cUW+dy22FoEyykFEx64OvLzcfE918+/Hv5eiL5tfk72w+YJgC+gX7w/Q

X9k/wX9q/8n6/eebxI+DX7kejX9/fbrxU+nl/EyAH/VCHHv94uktkOM6dyWG75tB1kDeTOQeIvl6xGytgy6+7H4g+teh38ARF5HiAI6A2AA5HmAN+hnQIh3wdWtG2mSW+ZZ/7fy30DYJkICQ6ezEY710mXHnxbZnn2x44gEKQ7UDmUiLp95SV0mO1NzK+6p6tf4nyyOk7y8MVX5lcT4hGALRmwAoAM4AGgEYBcucaBnAIQBXCPGBGQOoXBhbEfdX

4U+YX8U/R3/C/jX+U+y7xKPFSxa+7h48gcMN2RF3+rg6ZzzSCLpnFvELDWDH+gADgA0BoIMmByAI+oaX/Rxd1x0+4HyDfSN7c3oeTx++PwJ+GJLWeyvv2I62GkzJYcMpzBtzBf0rJwepuizOgQ78NCbVHeZHNKehwB+jDVE/k3MVKPn0Qu+zwq/HC62+u0zffTxAh+kPyh+0Pxh+sPzh+8PzSACP7OeiP9C/h3/zeKhHz4x32U+J31R/vZ3e/aPy

0ly2/qhiqGiyvIYbpeeUKQ1H5u+6DoDf1F50+Bxw+f1MlE2KN8COf+7PWVP3cg1Pxo+Rn/DftLw1sOu1M/D32DJGQCe+z360AL31e+blES9XCOF/ul44E3b5jfvpcwB2gGY9RwAktWv2Y/9j+CHllwxbRMwyCukhHfdPyqpQ4zQ/on/W/tT1Z/m3wOfiL1tf/DxpmLr4Legv6lfS7/deAe2l3bL2kOp0q6MFCTzK2cfl2mn9YsU1FLwy963eiX9u

+cl+l+uZ5l+04fblTe2PGY+e7RVcucbgRO9+W6T9/iCr4b/v+IVO+UD/w8sP3cgNZ8w1+7RrzDf1bzJT5gRA1y3iS1z2jQj+XuYCjuuZ5zXObUifuaD/4RFav9V1CAK4a9+ESr9+tKnuYvv68tyf9Cr2Crj+HisvQqf8D+XjZ9+Afy72If60VTVzD+TlCuZ4f+7REf9CTkf6Wa+fxj/nuZ9y+uaWapcvj+bVxmfWT1mfiDwG/bb1z97b8SBifyGu

2f84aPv2r+fhG4baf7GI4+Tr+vcoD/mf4z+GGAz+wf26ajf1UUOf/hZYf4RYefw/Y0fxUikfw7/sfyj/ef87/mkcaSBf78jNf6iJJf9GvVjwMuLL8a5EgPJ1gwKEAsUYhfLD102UL1cBib/wEcyu8zVT/lKwWy/WIW0RPLP+mP1r8jOVv22+1v0dYkr1t/ZHzt/5Y/oB8BTO+wOEiMuYHTR9dCx/yJoIFoNoG2uj3d+1b7A+u77JfnvwW9Hb2cX9

b3Szzb0bfVD2SMnb47uXb33+pj+4yrbzmfnx3bfgL11eu/yA6DbxjfBl8a5GQJRQKALocaQAN/5P8CorerMAY5pbBk1Mu/A7/zCuJFMAdtNxIj7OMI0HBhOtoENMoOB2eWqYteyk/N/ez5n/CL8t/bP5BMqJ+RyuNKU/tv0i+hk1ZCVIbsCLtgrHLetpW09GZT6LzIh/7/XvKuMD6Krm3+jL6G9kOOeDAR8ibe+SKhOlzmqAFD/laCwYAW3uP+bt

YI3nMeiQYz/gW8WAHd/j86+gC4AZs+g1bdUP3AQgDggNBwzoDfoED2Fh47vIoQKGDcSFzwKvaZYtYkymjCtmYMZ3z3PvUCDbCaHBJeeoBNUvNeCczvPj52C36v/kt+G15BdkFegvb5/ht+EAC//kX+//7epub4S3o3usjaJWZgAQFGqwZclqMochClXmlWkk63nggB4n4IPutO3gz/cm9yHxJ60CT89XLu/nD8APK1IoQw6ALxunYBrgGU/O4BsJ

JOARL87XJY/s0i/gFo/IEBrSJ4AYqarXaEAbmeSN4FslNyPgGi/q9yn4bQklv8wQFfcrUijXIeAREBHgGL/kH+a/CnAA4qiQBItKN8BN7yEo5eB8BCulTG5bR8wGUg8sDLutIBP64v/gneWf5kLkoBX/485LTy6gEmvpO+tY71ErHKqpYNtAJOUmK1/rMmViCVeAvWMAFObmYq0Z4a3q6+7mbH0PxUCtIbbgduodS+bqv6/m5gqkbuEu4gBqAG4k

xKTCpM5tzXXFbcW4DL9PisnEzD+p0saayHAQJMZKyrAUv6+qp+bqgARtDbAZ9udW67AdcBaZ5eIkzclty4ALdc5wF/lLrQVwF3rN8BD9pkrCQUpO6dLKTuXOY6FCsBzO7vAQccbO6bAa8Bmu6ABnsBRZ5nIr6CAky/ASzcR7SJABcBdkwggZrkNwHYgfsU9wEIgWsBrO5PAZsBLwFvAVSB4u5c7hiBe/qXXBbceIGAgZsU5RwnnGSsLIFG5BCBkD

RQgbUsMIHD3jMesQFT/or+JAFGkssBhZ4PAT5uyIGcAG369IF/+p8BoIGKTGSBxwHM3KcB+IGEgQmexIGprLKUtwHkgd6GceT3bhsBCoFCQKiBOwFMgV8BYAYP2riBWoEcgZcB3IE2gcQy4IGdLJCBbu4bikKBnoGaAsgGS/5Gxq4Q7QAqvLRQ5ZZ2Xgp+4Ib8JO8Yc1Du+ClWe+pSbq4eoH60PmfePl5xPvK+8gHZ/h/+1qZ4ZqoBPQGUfrt+yQ

44KOOWz165+qCEnQgWmEkuiuoPus8Qc6JOvh0Cj34qrtrerxLSgcvQ0XymgfKB3/pKgcluCu6vAS6BDkyGgRqBfwEAgQSB6lRcgaruiFC9gdyM/YFemnLksoFbbjSB5oF0gWiBd5TMgWCBOIFsgQ6Bw4EwMqOBUwz2gryB04Ga5HwUgoGa5MKBfeJUMM2Bhl71+mLuZoEdgUuBPYGqgQcB6oEwgPaB/wFnAZuBokx6gQOsBoGPgeBUxoHh5G2B84

HOcIuBVoHdgSuBtoFrgScBL4HagSOBH4H6gWBBRoHVboeBnoHQgT6BIoET/mKBZB4Sgcjeg9JwgYQys4HrAe2BioG3gaBBD4HcQuBBmoGQQY6BHkwwQZuYpIGkQfBBVoaUgY8BK/oLgZaBiIGPbsRBstI/AeuBFEFvgVRBzoHVbnuBP4EIQY8UR4Fy5CeBag5+gQUB96gLPkYA0ED9wA6A8TJb/ptACdoxaDSGH0R3IBiuSWBzuu5QJVCfAv1m3C

qsCKkAhQqxxKlKNXCAHNHeqf6x3un+zc5yAdB++y65/oaeGe45gYX+vQGhflg2lAEJCqlYo3RsFu2SlYFeNg4ehtg4QLWBltZifl0+Hf68/DMcWADGhAT+QkB+wBcYcrwZHupA+27KgdaB94F9gd+Bz4FDgTqB2DBQ7gxUGIFEQYJBmUGvgRNstSxOgWOBp5wlHPlBwEG9+pLmz24Q7sqE24HZbv8cnSxEQfyIL26NQR4cu07gnICAUUFxFF3kcU

FRRHAAiUGiQMlBXYE1QROBWIF0QQOB7IG8Qa8suUFVQWxBuwGFQdxBWUHN8pyBwu4LQQyB7EFpbvVBeygVbs1BtSytQY5s7UEEnFEBrlqSFtTumuLj3hQeV07EnD1BmADRQf1BrZwJQbgASUFEQRNBjkxTQU+BK0HFQUCB80EtQdVB6IHLQRBBq0EYgWVBA26VQQDBi0FMgTtBuu57Qa1uB0Ga5EdBeDwnQSecvoH7uEm+2MTlgKCuK/jCYOy+rA

G4ovgG8YFv8OwBaHJJ/mxakroanmn+hC42Qa0Bb/4KASsOHQF3CgX+5H7jvnI+bkH9ARH+Sj4dMtIYCsJt2gYBYwExxL5CljRnRoS+sAGpfureABzd3qquXV4vQcqEcSCtgVeBBEFbAQNBL0GoAHVBcMFW0F3k4kHJntAycsFjQIrBiIHXgYqBqsFDQa9BGsEZblrBeJQ6wdL+7dIuTpTurV6kHmPe3J6dXgW8+sEKwVaO4Ij/gcxBHYGmwYlBFs

HKhFbBnEA2waZeg+rCnsm+7cB2gNgAvTB20IkAeUZ+3vZeIM4OjHfAE1TzsgCEU35x3MfeiYFzfgnusgF0wemB7QGrfo5B+m7OQazBwX7swfmB/OBeRvoAY7oRfoziMyDR3KGeZBxRFhd+2GD8BGX0sq63fuLB7d6txkmSI0zD2sJ6m6J2hsA8oECTAGie1QBGAK4QwoZxvDksy7QD1IF0u5T8VMtcr9ShUrZ6e440QoMMR+yMQqfc844bwcgAG+

yxutxCfDrYQk7cOPTczL+Um/znges6uoQIiJfByThAVDfBIoihhH+0uoSE7tbuvaCTmO7BiAD3PCpeEjoj2kPBKgat1KPB48GTwZh088GTOovaS8FQ3CvBO45WXDvBk445hCl8B8FqQvAh4/QwREghhez6QspMx8HaQqfBLSznwVyId8GFON0soYS3wQgCV8HotI/BpDy6hC/BoYRvwc7ui26fwWbB8sHfwei8qEEEAWV+RAFF5kr+YGJRNBrUC8

EhVFAhizqhhKvBqCF/jnvByCF2gmvBJ46IIdhCB8GHATghloLIAHgheKyTmBfB5CH3wbo6VCHDNBohxCGzbNHycYg0IRo8r8F+OkTuWO7EAEwhGR4sIV6B1wjFBtFa5l6PTlr0oQBAigjwyYAa1pH+O7xnAPfAlXifyEXiIPhvOAOI1+ACqF4ItyAZolV8GE4SwpbY7ATkwZTepn7XHrnBLQHiirC2qe5MwQdULMFf3uXBxf4AAUNah36M4tBw7l

BwOIIuxSpDZofAIPj88sl+rQJwAXMBUsHt/j3eAXz+iMXETO7aFOAhJ+QNIa4htPyYAf8IjSG0/NF8/CGBasgAbSG4AAOYZ0EDOhdBbV407tP+WEFdXoMhTSG8Di0hAyFdIe0hShT5AY4h3VAmhiZuGgDtADkhBMH+3uwBv4J/nOG43AGT6MFcchBjdBiwksJ3+KJg6OiQOFUgPZJIcku6Hl6qbkmB4H63Hhfe9x55lvH6cH4lwQLeagEuQXmBJf

6KtpXespKBEoaQtr7FIXwEFcZKChd8XcEzAZUqlgHzAXu+yAEl0LzUW0Q9IZ7BHIgzIbtCwyG7YiihsyF+iIshQyFYoR0h7CExAZwhcQHEAVMhBby4oWihMRoEocj82KEdfv6B96iGDuOATQD4ANaIUs5KQXawlwCP8KX6tyFhpPzBgd57gESS70SFCvaUVSCXIfBw0YF6gDTQYVzSNjyWFkHDNtTBrMYMokkhid72QbB+7b7f/qGouYEhfpXBaz

Y4KOfWxYFzpq8QIPSFKoHO4KHDVMmobzJjZno+PcFpfqFBGX51IdAyn+6IANuOP47wutQAUhB+wa9BchzSHDqA9gaSQq6hxbyEOt6hz0FmwbIcghwBoSMhEI63+k7B507XQTwhQaFsHl5SbqHfjr30nqFhofFBEaF+odUcjlyMoVJBlmgd2E2Ae4JFtBTOYYHb/rJw7AwLuLKhMqHjCMLCNpTBoFXipyGHnhlKqwDHYLMAz3CikLTQkgHnoFnBtb

7xIT2enz6LfnZBOm4OQX8+XQH0yLqhFcEl/rse3MFK9roqokig1pD2lqESEIESTIKzTtMBqt6HzvABCKHSwY2Ba9B5LKJAvT4gap08MkJNLHvBKiG5rNzMush4gBkUbyykIYQhz8FaIaGEa1zX9Fz+BKjomrL0P8G6wYFSfBKStNDcVWoXNOc8X9RPrDOOl6E83OJcZ8GTmLehxAD3oWohT6GbmCQhHoLAdO+h/yifoYgMdzxsIWP+0QEU2pP+GE

FAXpShAXxHoQBhZlSnoSBhQrS7jjcsKXxXoXcsuoSwYfBh2iGoNM+h2DxoDKhhaAw2/jcoX6G5rD+hfS5hwZB2Ip73oIkAXQoNAHAAtQBSjh4huKIOwIcAED6HIFiklMT80vsAu2DdkAHGyTyX/l+cchCHABHC8qg0jlheTyE5wUOhGf75waOhvh5FwROhNPJToX8heqEl/vjBxqFaKhrq3ZCxfquhtkAcDNg4Ld5N/t3B934d3ru++6G4Ungwen

znTGv24/wv9qZ8u2IBYdF8YXxBfDGh7J5xoQBeul6ujrL6z/p75ufmEWGJdCFhgEYFoashxrgpLPQAboC9RG6AxI60NGwBbvhfAusEl2Cqlm84GyCHAGD2prBV4iYa7bCRYHO6kmjtnuFcjyEn3nW+CSHDobZBus7VrmZhTN7fIQF+7cDToVkhWgE6yuX+SLLMVqjoFrpkHMV+XjZjYo4gZgFKdvahksGa3jbWNfoWkmesW4DhYfQmuOawgdvMW2

G45nfGu2EkoXhh6EHOwZMhCQEo3onQ+2HEANthgXzn5ishs97dUFdY0EA69MoA3WadDm7i+wCqQe626kG5Tq8YomazAAHGX3Qius4Ir1Doip603bDJTEDkAzaP/pqeVHZdYcZhPWEpIX1hygFOQT8hw2GaAal2ahzgWrzw7GxOYRBIZrDGoDFMfy7QPhLBrf57obUhMsEFvB1sJ6GFamehFzwC1DRhkGFjbCxCN6H4whkULTpD0pOYD8Gvoexhr6

GcYdU0mGFVNLxhBk5dXrTh6z6IVORh8LyM4aU0zOHzHJFOuay5bD5y7OGvQpzhCGGFVEYh3zRsYTZ0aGFf+lxhwuGjgKLhTk7OWrL+/r41VldBLsET3jThaRJ04bFq8dQUYZc8d5Ty4XwciuEy1Mrh7zQwYRzhp+Rc4WQhSGH6Ib2Yb6EcYR+hZyjcYWi8tiGPYVs+a/BnvgvyYbBsAFzBXKGfyLJwNNCC5DNa1bBOStY8Y3QOCK687dxqxoUOAw

6Plr8E6LIRYH0SG84P/k0BHh4NviOhyOEwfp/+zMGlwRkhf/6mvtjhLAF2YXR+MOjQUqABp57gAc5Kc0oqxiYaW6Fbvi3+u6E1IYgBTQ5IoclhYObRrEFhggLpYYbep4Gr5lPhqWGL/HPh0WEOwRye8aEK/oRhl2GD0np80+H39ivhO/YZYQH+DiFPYca4fhBtdI6A8QDVAIChHL6fyHIKcCJ08ESwTIa5tsOCRAZrpla6KHK+NpKhghBCuvT2an

6lsDd+AoovJnEh3Z4I4UZhaqFtAb1h46H9YdqhfhiY4c3h7kGK9jU+1j6xxNNhK7hfgGyYqHDLUO5hi9YyBkPhO6HVIath7DZg3iXQwvwHYRl8hDB74Xthm2G3YUF8VBH0EWvhLV4b4XFh1brW4UaSN2F74UdhlBGR4TQBxrh3KpqKUspQAF1Wg3724q8QtpjLUMqQE3TnfsKoahIb0o1CyWBg4oR2X5wKiJBwZSDroQoQhK7noDN+kr7PIc/+iO

GQEfTBGYGKAajhnQEWYT/+VmEzoQABqQ51wVCMhZgYvrLgTw6oGPSY14CCoYPhKX7LYRTho+HWAVmmE+HqzIi0SXx24dVq7NSO4b+O4GHjbF7hauE8jBrhhiEsYZ46OuFl6MbhHr5/oZSyJoKS4UBhfLShEWBhWdQe4XZs9GHe4ZqMMREMMDzhxiF84TZ0xuFymjL+v56igWSh4oHb4fpeEaypEceh6RHS4ZuYsEIs9NnUERH5EVERhRFMYRZ8JR

EUPAkRLmjG4Qm+JCqe7lBexrgSCvzA8Rz4ADWeCcEKfiLAx2DBgFFIWdJYOFOQwqhPcEg4Zph4rk14GUquCH8EpSGQ4o6wemHtYYOh4BG0wYYRBcHQEZqhef7o4YNhiNaWESNh2OE3DrkhnhIuEQ8gLH7owM5hCsCvEP6cwUFajo6hT37OobESiXy9IfMhiXxc5qCR6KH3CH0hc+F/qidhYWa1EQRhhfKSgdAyUJG55OCRUea8EYYexrj4AIbQ6A

ZugAgAFd4cvqkEh+Ix/tiu8nb9EvCGoBHyRvoREBEHyuqhY6E3EcXBDn7ZJMaABMjtAHZQvKZtCtgADoAHAFGApwDeBEyg5YCSnuCADQQAntgAWEhRgG0oOqr2Ro9E3Fhwvo3hGgGIEf0BEmHzobn6DIKKwJLCjdxbziu+1cSuvKWwncEeYbChu7I+YVThB6F4MMRSaoyF7D3mD1wLGpDettz10LaRLOEjHlehPeb2TmmhOxyu4VBhjqrmjgxS1p

Fp7M6RjpH2kV6R3uRBkd6Rplxukdx8HpHFvFehTBHZnmdhCaFW4TdBAXxWkSrs25Iy5naRvJoOkad0HRThkTGRizIK4T6RFeTRkYrhhZERkRMq1AE4kTh4jxFY4dii0ZYlyHBmliSDNrYeHpxKzvBaryYZljIBiSEMkUsOir45/syR5mHEFpfIE7Jkzpvwscp2mK7aQUbMmNtoyBJRSKVGdma14p5hw+FEEQsBxoiHBgHAJwb9lhOWg5bv4Deycc

B3sg+ytwZPsunAL7KPBu+yc5YRCG8GP7JLln+yEABfBhIAOQxbltDy+z5rvMrY307TAFtM1Tb9wOCA3QrApDwAm/6POKW+tAxNsoDiGWRkPv025kFu+DGBIH4DoWARqY6qob2RRhGFwTARaOEwTO66tQDskRiQXJGkADyRfJECkUKRlOAikc+g4pE0BlKRMpGt6OhAp76f3kXeTeF9AbnuHEyDAeVOv4JJLgGyqwZ6pELqGS7/EX0elOFj4fku9j

7dUOZCRgBkDP3ApQIrwAcAy+oHANUAH07dlLlG296eMOCGTZG2HhsM2OiUPqrOlMFdek/+nWH0kW2mjJGmYahRZhHDkfHSiQ4qWnt+sgh5KqMoSmQGKmziDQHOSlLIYaRykJx+OFqJMur4Q1zAWEDczDaTZlhS5pGb1jFKANzuUWNcLj552N4h6VAMhDcAZgx1sEXOIpC3rpYCrgiFGAB+RxF8CLXOoI5TDuqemlHw4QhRYopIUQk+rbadpvUmQ5

HHWG6ATYAwAH1AMwobYILMxoCnADoOLoRtCrXB5QDEUWKRpwASkeRRgpGUUfKRyXb9Wv0B5aGvEbKSQNjWIO9UgbJu2kIkx4BxxEtouj7mAeSeysgdXD5Rx7bpEWtcF86AYfNRXr40bre24I4xYVpwT7YW4RV+appCUSJRYlGJ+JJR0lFCALJRYZK8nr0+S1En4bzaXu5a9K9h36BQADFkUkD9wIkAkKRGAHEsCABbDvs+ZxjyUaBRW+oYLry+Lx

ArLg4Orz7qUSn+SqFWQTTB8d6XESZh+p6mEQbOmmBFUSVRBwBlUZMAFVFVUZ4Qp4JuUMKRopGkUZKRLzYUUXKR1FGKkbRRypH0UeFWiMhAAT2we842UVi+1Ar0foSwTpROUdIu51rGuE2AjYCwtHDERpAifs6+DL4+EYb22MSs0Rvkbkb9wJaM8xGO4tXioaQmLv9RQgGOdoK+Vi7CvsARr65ivnYuCjbmft2RBhHZUaEkuVFWVnZ+Tx6zyAjRpV

GtaCjRsECVUdVRGNF1URsO2NFNUWRReNGtUQTRCpGGvmXBdFEcwQxRCF7qkWMmopD8ZKdg+ui2PvTq7ng8JLahE1FVIfCh3hFhQcCRZcpPJGLhk/ClLg121G43ts0Yd86IkfL+0I5BvjPi8hrxAHdRD1FPUS9Rb1EfUXhWapG8nvG+3pJrHllha/CcSvr0FYjepK0AUADQQJoA1uKjgOfwSsDZyN9RAd6hpJUB4WhKztt2oNHqzsqhthZ3Hsw+Hy

GPHl8hp4j60UjRhtGo0abRtVFY0SRRVtG40dKRttFUUfbRZH5Kka5B+qGKoNXBPC7VPnOm/AQ59JmUDNAtjvTqEwFOlPnh7hGVIfo+zlFGuGvwHABGAPoA2LhYAK5YXNF1gYCRDYG+Uf6SV9E30TtAQGAuPneSlpRU9o6Qoqh88k146cFjDjyW0qEkrsd2+mEdYYZhFxEa0TXhGqF14ZHSI9HI0ePR6NGT0URRltHNUTbRspEL0TRRCL7/IQABMS

6b0SD2AbZIbCtoim7CLj8EW2iLYZHOXmFA3k/Rgx7tKm32XOwWJpRqW5hZkT2EI8Y+GkWRJhSPYr+hKA7F5DaRuh6cMbmRTpEcMXaaLpGBojwxtsEn4jGB8HwJ0WMhm+FcnlM+ZdHlgBXROvTV0bXR7QD10eCAjdF+nvTuUYKMMQIxMCZiMcGRrWqiMcvuXDGF5JIxocFCngJhEcFVAMaANIBMIkvg36DI8NcAxMjPoI9KBUDTlM3RXTaTXipRLz

4qzqR2FeF0Pq8hqYGXdlARKOEGUXDR8yBztu0AEQ7KAPakfoArDHXY4ZD7gDbO3E6aYA1RONEtUVgx7VFE0bgx1mEAAUimthGunFr2rT7ret3hANRD+LKhtsCLkftSHhGSLqUOohGqdtx+2ACeXPoAAUqdAA/RIUFWAaHRouoxSgcAbTHwxJ0xX9Hi0cNUUVG0xD+k0sJ/pI+uz/jyoQrRriRvrsrRNJHWyqrRzQHq0bpRfZE2fiYRyr5aobPIEo

Ao1vExiTHJMYd0nTCPSoyAGTGfAFkxM9E5MW1RhNEO0cvReDFaAYKuqL4tJAu4F8LQoWABwaBD+EHO+SFqjsuRhBHB0cQRNzaJFtl+yRFYjstRcdHRaGtR6+GxYUxudVbXQVZojjGEAM4xrjHBTGCu4ICeMUIA3jH8btl+oxHqDpjBblxE3NBAJNxk3OCklNwwANTc00RcWPJRDjCLVqDOa6FKzjoRce60kdpR0DGbMchR1xH5UbARs8gxMuZYPm

gBgLUAzoBYAM5ozoDMAH8wRNw0GkXgdoAcAFJA2g5jqL7u36AoyO5G4wBwAIpuhcgDpDyAzgAHlpw+jmQ1BDngG0R8HDCAxwCTCiX+Y66vMYziZKI8SJJenSRd4cFGdm6pqKThdqGNMSuulmgNAPk2s0TYAJlmwUoppnS+OpKTkeJSMk7lnh6x8LDesRWhIORIIj0SO1DAKMk8ylZhppYCNwByqKsg35xjCIf+ap4aUVQG4NEqoVlRHLFXEZExg5

E8sY/ofLFeQB9CMABCsSKx2ABisRKxPDYrRDKxcrHNAG2cxaTKsRMAarFw+AYsWrE6sVMQIDbCPqkshJGeuh5oprEAAehufMKRwo3crbCpyg3cUAFUMW3eNDGEGgGxfkF8UYeuIoY6TEVBo8jwzCuxP0EMSJURdsFsnrCxG1HAxo0K21HBvsSxpLHk3BSxVLG03Hg+vJ4bsSDBdECZYWfha/BHdA0A8nTxVJMA1QCsQOBY8qbVABQAz1HfoMW+KC

5sKrQMdLHXxErexSYp2nDhPdGNtmExBF55sTB+3LFoUdUEMAD8saWx5bFgPJWx4rHdRDWxRCR1sfKxjbFKsYQAKrGtsbzA7bHasQ5QXbH6sb2xRrEDsUTW/QEiEeNhVNBFoOgY+eEJGHFWhtaGkWSinjbGkduhR2iyLu3AH+jPeLKxQgByylj2frF+2vOxwoT0MZJ+I8r8cY6AgnE6ylyhDjBxsV62BeEycHEAXZAzWi6K3rTnfumxXdFUwVmxvd

FvIf+uPz7cxkPRvLFIcSWxgrHCsWhxVbGYcVKxxog4cQ2xirHNsaqx6rEkcZ2xerE9sYax/bEmsTRxDFEAzvRxHMAnMHdUPkFgAauk8X7KEr9wj3wn0Q5SQdGw9OJxa5FqrpomryhFQc32i5gpcThh50FADuMhluFTPk+xL7HggG+xH7FqAPoA37G/sf+xbX6UHjL4mVQ/QVWR11HdUKeCPqR4QMoA/cAwAIhUk8HQQA0AcArT6o6AnKFAUQ++PC

zAcd4+5I5FWjjyEHH6cVBxkH5pgdDRDN781l4u6w7gDuZxArFlsVZxorEYcZKxtbGysbhxTnEEcS2xrnGasaRxurHdsQaxfbHGsYOxWgGhgT1Rz4JKaGMgst42UTBaJfpF4gAIdr5ccQQRPHGkvj1AuwBbDswArShCfvGg3TEkkE44gbF7vtjErQCfcZPYP3GdDopxuHbjWgxaSmFMWg8mLFovrv9GaVGGCrMOyYGxPpNx4THWfgFeOzE60aZxRb

GLcShxK3HocdWxdnG8kZtxjnFNsTtxLnFtsftx7nFHcZRx3nFncdjhFFZt4VOkpYzNlotQe2QZ4V42tQFUxkFBFSExceTh+nbxcYihcbxUprV2NKYIkfIxtS5HsanRDXH9wE1xLXFtcRlwnXEZ+O4SUs68nhLx1jEQXrGuExFr8KOAudgJgPa0hIqgig0AjIBrRDAA6oQ+RMYCfXEEPkFog3HNkSheI3EPaisxWlFQMZDRMDEp7nBxWYGdtppgxb

FLcahxq3Gk8Rtx9bEKsVTxhHF7cd2iHbFkcR5xx3FUcT5xJf6+cQFxL4BTTrmonxHI7DZuSGAwOI4wz7oq3q9xOVgtMa0KkgA/YA1+NH7yyqJxqeqi8b5hdLYj6iXxXtyXvuXxzTHbSFDxSKSCZDKoxK5VpmbK4rrl4W7xjaZOLpXhecFQ0WhkA9FWprhmfvGfAAHxRPEVsTZx63HYcRTx4fH4cZHxtPHR8Qdx5HGecSdx1HEl/oMKbtGLtl942l

a0ZieeYZ4ciudsTrGB0cLxOPbV8TNR1Xbc2jLih6ZSMTCxzBFwseV+zG7JkRAAhvGTAMbxjICm8ZoA5vGW8dbxTAT8bvfxOvF9XnrxA17twOjqVuJ+BPGANDrGgBEK/cBQAFe+DQCEAMYe+mbzLv1xFMCO8Tt2xD49Ni7xC15jcbDO1kGe8bmx03FKvnjxezGP6EPOQIoHAHQBKYBNgPWu/cDJgO0AmAaOgDAACTGh8VtxEfG7cSvxwcox8YdxFH

FecadxvnFk0e4hu/F3DspoNsAVYYHOgqHfyrLgXZDa9oLxuQrWGlfxi7HczrUS5YCJAJUOhADdUTshA3HX1ltkjbJ6ZEu2Q0w+Eu8yJxHZwZAx5xHECUdWETG14b7xTWTfVnwJ6/Hx8UzxwgljkSPWJmYsCP2wzozd2qeeJKKE4fIQSQCWGooJSmL+sVvAysALsbzR4+FxvBtOwPoggOpAPADeAT5OxIxxCQkJ8ZFy/hbhgF4okURhck7i9Jpyqk

JU+mkJtXH68feo28R8wMZyPXFZzvoJV0gMsRxIx8CVtBXi3kIcNOYJcFGssR7xiFEkCbAxTJHwMYxijglr8XHxjPFCCSX+ODYmrJF+AOQchPU+1bRMFi8OP16xxCtg3FHH4I4wQPE18UryG05GzKPAqQljjF1BeQleZusJCACbCTUc0vFZcQox7V7JkUmhawl81BsJhQlbCfexUeH3qFGADTAlMFGAbCxVCcN+wd4u4tMmMriG2OQK9d5R3gQJjc

5ECR0JNgmcsfmxPQk1Mn0J9PECCZvxifEAAQ42KfHWLPzS+fq7UiyCLcGoik2yPkKboWLBJpHKYioJUQn8UbJOgVLkwOpAuyh+8MqEXObV5PKgxIm5AKSJ8+EsnjuxZuGlfknRpwkXYQ0RKVJEiaJAJImbarSJE3a68YSxb2JSQM6AGSAMtj3UrwlpZPtINVKzui5eXDJJ2rwyFMG6celRkHEaboZxup7GcfrOaSGqAU4JAwmCCVvxAAGmbhaxUI

yu2hN0eipTCYLBrdwIGt9027ZYidxxswEEWriJfTEWkajenADZruyJWKg3AGSJ8tJOiRSJWABUiVAAbonciQ/xcjHHCawReZ6okbESdoCeiS6Juyh+idiRdXHGuDsOyXDFQPV+oonosCN+8p68iiKoIcTmDH4qLQmzfpYJmVGtpsCJsHFwMfYJUpb3ESiS/QkM8dqJMIlaAfnu+omlMQ3IibDybnCM4aYIIq6MyyBxBONRS2GzsRV2dolOodThRp

IT0LKEhoSkQhc8Q4nEALCBg4mThCOJqdRjiekJ5uEqmvFhr45ujklhCDDp0GOJ04kVMLOJxQngCbeQe4IHAKCKUtjJiQN0oSG/pLyKDJjPIPMxfwnBMejxsr47LiqJo/GsPrcRA2G+GDIgmomVidCJzPHuQeVxYglTpDW0/UJkooWMQ1FuINJoZwDocNOxzf6AsXFx4QnLCdfx7So0KJcJxbxjiUbQCEIjKvsJVPqR0BGJ8qCBofBJqEk9cnKEyE

l7CXEJGEkrwF6JmACRUlURx04JkUiR52GYQTvhXV57CXhJhoQESQhJREnW0JhJWAB2IWZeV1ElCZZofMCYAG+AMgBzEUVhDvHVCbwAEYwNYV/wS7ZsZnT2VJFtYRYJZxH5iehmWPFFid0JJYlFlmWJ6ABviVCJCfGfif0BmJ4/iYzifSRASIf+KIlAScZAPwRLUAhoCwm9iUCR/Ym5CVRqtBRFQVFiP0FJCXtOvgxOSaJARUFziYyJmQmLiTL6FX

G3QQ5JhygeSdVxt7ExiTxJ96C52NBA9s5FQADOCnGiSSGgsBYn4mGkdgw/YL4q3hE6cQmBrQlLXjeJEH5yvspJpAkDkWCJ+7oQibHx74k6SW4JmV76ALox8IlG1mjsmTLqxijSM9YiJB7RnYnUMSuRtonQSZEJ9okvEgxJXIAkAK9mwyx9SQhJabL0ppRJGQkLiWwRKZFVAMNJqEm9XrFO4xE7ieWJkIkb8RVJNbKTQM84pYxMlu600sI4CXvq07

p98V2R6zE6UYWJmtFtzp2mxUk4hppmo5FVSXueowlZ9AoSbizvXoHOAc6+0SAo87p58TCh1olwoVBJgPGEtj1JYuTdlpuR57LRwAOWl5FyUPuRUMCHkfey8XiTlv/A05bCcSuWVhbXkdQYd5EPka5AHRTPkSPKQnYzUrUGG0nFcELkTJY2Hj026LKW/KpR/cGHSTdAFn7ssadJI/GJPn8mUTHqiQLeN0kPXp98YBoKwsMIs04sgrgRqIo/8I/4Gc

ohCfuGRKbxsEtQa5FAyccGIMmXsucGe5GXBgeR1wZHkUWBK4hnkU8GFWAvBleRC5Y3kVbG1cCfBgBysTjlCtjElbHxgOCA5nb4ULByndFH/qC2KnEcSA/hYPa7gJJmwDELMVSSffGKiefe0HF+XoVJmYHj8cba54gbcBbwhAj2SFi2ttoAAcSRJTF5/IdkQhBNAn4JkQnfyrMJPbB2itFxSgm0XKLSeIlLsTEJRk5xcqtyu4wy4htOy3KHCRlxoy

FBiTpeU0nnCenJlxrbcsF8NwmXUVN2/NGy8m+xlIadDiVwtg7dNlbJ/MIb0ktolNHy0VeJzsnjcUqJbsmX3g+Jvz6FsYHYPsnFCFeIRAgXDnq6ZlH/3nWJ+FwCbNo+JVBpPKJeD+C3If2ImIlLkdiJg5LTUaoJ4UH2SUtyavJZyQXqOcn7yZXJ354USc1eVElMiRMhtEn8bkfJ5cm2HOFJS0kFxLh+19FAgJaQXKGNyYmik17cwPGSqMSLUClMOY

m6EQZhVglAiV4OtgnFiV7Jo4YjyZeIdki7cIHJKzYl/oo+tUnS8HbJJwCNSY98aImQWnUBAdFdiR1JOOzeUdvJYdFNgZD6IVKwIdE6wLpv7rCBdPohoaQptB4gut5JI96+ScXJoYnjkup61CkeocK0tCkUKduJgmHtwL4AsIqTAESRItHCSdBm5snLQFliX5wQOM6MrkoXAOnBwLaxISjxhqYAiRDRIClrXiCJdgkQKT2mUCm2SP7JsCkJDlXapl

EFgYhUYBrLUKwIknYsgp8uU1ohoClgOzDcUVGyAMlDHpaR+FLpoQLUzB54OpuOTik+UqU0rinkSfSJ1RFoQdRJSZEsiW+OBbxpkaFS3ikPyTwpVQDjtNkAPYBEAGbJtg7apj02QJCYTmJSTyZVTgop/JaECcopObG0yd7x4ClaNsBWq3B4CNoppQi6KewuOMmTyYYpKL6EMZAiZN58wm0+h3ymiTagE1ReeLgRCcmhCZGyW8kpyd0+iwGIzCmhwV

KekYQ6v+7YSX0pAhYDKWwpGaEcKSPuPimrMn4pHCGXyTlx18nBKcRh/SngmoMpNCnTKREpdjESAEgUPYDBZHXa78lVsGSRjbLSkOlQpYzfAukpGbGKKQQu2bEFiaApain5KXNxhSnWSL7JJQjXiB1RGfpk0ea+bPGGSd60COj6AX4JR/EvDiNRfE5tSTOxuCnOUuXS3Sk7yZWc104RclRCRQnZyaXJOYIIqRfu9Ck1EQspWQm/FnRJgUkycvgeWy

nYxEYAQgD9wOOAcR5QPPEp88pt0YcgJ8AEsE9wmcQPIb3xGSlvlmWursmY8TBxHsm48ZdJmraaYFopfsmlKTeIv3b2tgYpVcHsUmX+M8mzvshSvjZcycyYdgypyhHeb1TvLi9xDTEQqaxmVzb2KXBJjLJKFGgBxlRmIS7uXObFsvQhY26Lbuip/imYqX5JdO68nhSygJTaqfNuDCFlOgSpblyZAty2tNyTAL7ewilQFphyDZ6MNFV8wOgmKRrezF

bCthFc14kvIVXh3WF5KapJGilMdrypbynjyXApM7beppXYCQq3AFoKBEwKjlnxQwggKK8u1km/MRHCA8Gu8k4GZ4ZjlFlGOUYL3LeG0QmS0jksTbrQmupA65ZHNKq0yMYaMKjGF4AWbAYhxRHvLJNsN+ZkrC3mYFQjKS/6PPrW+nAANalfeoU4A6qSOraGgCFrNMWpkgCxRgxUDanfRqcoLakt8tQhtSy9LO0snakdqSVUn9ozKY/xF8mMKSGJOQ

lz2lWp6GpDqaF6UCHrCHOpTamcAIupKjzLqZrkq6ma5N2pulRdqTfmnEn8YQGOkSl02vEAyYB2zp4gbqkyLkWme0kCkAH6exENsBHC74B2oG4scim8DIqh3dE9yayp+UnsqV0J+lEFsQhxs8jg0NZQ2AC2UPZQjlCw0G5QHlCI0KZgxBAo0FsQHyly9pleTS5LenXc/rbqls5hOpZjYpaJ68nfScoJpJA1mNBo66JM+nZiLPoxqmOUIBYggGAWLA

DTwRWpK5Q99OuJZqp9apv8HDD9nK2Mf0Hhaq5qEmlOamMc0mkwMgIcqKr+amgCkmmbYpgwchQqab+qCgLUMK4cfDDaabJpOqoKApqGCPpFuldMU6psVOuWCgKCRN2EKESHhP1qZq6DakqqI2oCarNqghQuadKq02qjah5pzmlMatxqS2r8agaqMuIiaVOJYmn9KvJpdDCXUlppAoHGafWq6mkKaSQwsWnL0DppJzhqadwwaDCSfClph9DGaUlqCA

Iaaao8Rml0qqZpPob9qcyqwEZj2htMvGn8acmqVml6hsWGZq5CRA5pcqpiqv5p82qTag3CwWnsau1p1Gqdaaxq7mm2aZpyC2qNmoOYy2q+aSap8yl7qfEB/G6rlGFppqoKauJpBWlJaQDSOWnD0Glp1mpSAoVpz55raW4cxmkZafppmmlkMMVpumlRad3Qj9zx5BtppWoiHlb6FWkFACmq1mmNaZAmjoT2abZEPWksMRKq/Wluaa1pvWl7KMxqU2

rdaWNqv2lcanSafGoratwp2ykgoJrY7QC1iNy2sHLyzg2eTVKwFkK60EiH2DhApeHBxqcM/wk3KQZxfcnvIfTJ+ZYoaYZRx1joaZDQ2Gkw0C5QeGkI0KsQ3lDrEEmQqNApkHopZ7qrNmvR7FIIVpdxs75hppfiAqhZUM5hPrRcwM1CgsmXRr0eRzApYLb8CXEEjFAqfwhccCDwSKlS6aQAMuneoPnJsaFxBicJV8n1EcspkCoiFtLpk/Ym4LcJfB

FxpkYAOXAb8I9EiPJK2ojpkYHTMcJSXnju4tXODsldyUypFHZKKbcpSkmIaeGpyGlcqRPx5QCk6ZhpUNA4aZTp8NCKyVTghGm+UMmQitY/IRUpSQ4iqbkCE5GfpE2WGfG0pCkuzrzXgMQctrxn8TgpkEk4EoYQEtB5qWd6BancaaOUrQBjyt/Mk8qCaaUKq5SnmGkG3gbeaq26lmrPqg3SlelJoHoG1elCevmpp4YF6UXpNWAl6Yp6D2kJahCqk2

mkoWapTCkHqTLcjekboM3p/gZ6zPVpVmoOqW9i2ADxgIqEpwA8gMoARYHN8cb0SPLdWIJmtyY00DGO6kGjDkjxl5AwaXpxWSku6drO94kE6Z8hFAmB2D7pWGnQ0E5QAen4aTTpSNBEaZsQ/lATyVHpBqEnlgkKkybVmMGefCTPDiQ2QRKHnv0O7SlCyfu2HJiVzpJ27GnaegAhvIZoBhgGWAYqsmXp7mazwakGTel+Bj4G0EZx5D8Gk+kV8hzsG6

kK1JQ60B6lQYIeCoKAOugeT9pXGui6VzoPqVfchTrEGQeBkh5EGeAePMwXOkk6NBmErBvuZBlAHr/aOjqKHpqC5BkP2iwxrakemvAmT1zuOrUsYW7sGWbe2+63hLEaBYIK5DgobiidLKwp9tw0Hm3udB7cHreEz+7UGVg6ULq97loZm5jBof2KNB4j7tQAhhkP3LdpMJqb7hepb0YXgCoZ8RrWBpfaMzgJOPM4/FSViuk4NiHuzM6Cuh42OjwA+a

yFBoJAvhl4FJpAnSxoutwZ25i6qYtufSwEunia7JQ4/gfanSwT1L4ZE+6hGbUs+rQpGYc6aRnPLHl6mRkrOtkZcuStmtKUfuQK5P2a85jlDFXpQQBc5pVponphLPAZmAbYBh4G6BnpBlgZ4eQ4GS7seBkWzFva9Bn8HlY66zoCGfo6c+61LDs61BkmOpwZeDxEGb0Z/uF33BEZLBnOzGwZELocGfs6/RHughIZjBn8GbMZXiLCGUupH9piGWIevB

m3hFIZixkyGZ0s8hmrGTmEShkpAA4ZxCkaepvuZCmxOm/unSw6GewZehlW0NC6FhnXCMYZM+4/7uFSHxnBBuuKiPo2Gb9MX0aXqdPmtSwVik4ZB9ouGXM4ZMjuGTuKARlJmgoZOYT+GcMZgRkCIj0ZBRmoumUaSJmRGe/B5TrErLEZbJTFmgkZL9pJGY064CYhGUEgZJm8OhSZLuSYmQo8uRk4mblAnSxFGfXQixllGfuYFRnNGd4GA+mnYQEpW+

HZCTipD4YpBpUZmBnRfO0ZP3rMcKYGBBlF1JMZ8h58GfvamxnSHpQZ5cnj7iUZOYRjGcsZ6LQRGVMZvZi32kqZyh7uzAsZ6plLGQ5ssh4DGQwZcuSoHgaZ+xTbGTepuxlgVOIZPBmyGZuYRxkmmScZtSxnGRaZlxnXGSb6X+6X2vcZD+5/Gc8ZixmvGTg6LB5y5F8ZdxkIuuYZk2xWGWoZOYS2GT9GYJma5BCZ4QbOGXE4szhXKLCZyTgeGdWKCJ

kFGjiZKJkCzGiZMUAYmVSZkhnYmecZuJl2qQw6BJkYmqw62ZoqIp0iiRnpGeSZTJn0mUuEmLR5GfkZFZk5GaNpDnodmX2ZhRmFesUZA4o5hByZvZhcmePpGBmz6TFKZBivgDAAFvHs6boJY1DGykXiEqhPcJ9JXqkxaMdgniCa4Bky0GgNYemJRwC61gSwDjwU3kSu2OksqSmBbKnuyUhpMNGMydIqWBDMIDgQ+mDk4Mb8YYC06cjQr+lo0HGpfA

ZDJroOEervypHCJklkHJ8xrcG9iK4opgHZqZOQEumucF0a+RTkmohZZRTIWXiUdnpNmrWa4BSzGtMaLHohkSOaTRTDLChZlZrgFChZ5RRilF3kGFn0muSaOFlgFHhZ2ZGYgfyahFnK6etRwibMpsyJSynLiRuSxFloWcUUNZr+FE1W6Fl5ejRZg5q4WW2a3Iz9mhDp2MRmdGwAiQB/COOACeGi0Ufg5ulH/pbp3CqetqDitTFuLPbpvwLXmdTeJ0

n3KSpJHulqSUx2L5m6YKTgeBAU4PGQ35kv6X5Qf5lM6di2gFl1UQZJH3SrBNOkEFkJGDAavMlRSAYaFTGgGcLpu1rZ6dIEJTwcaS2qtRnAPESpJKlkqXOUZan4iSKGFen25LD66WwAJqeYeBnecMHyyZkWzFEaDfJglE3ykuZd8nr+TvYymaIZE8blDB3kRP5x5DUZunqt1JFZpKlRAIxU3/wpWfbkaVmcAGJwuvJhGZgeOVkBiAVZZtA9We3ybf

LyFMVZgeat5mVZZRpHCVTu2XFYqXpemumYLKeYSVmEiCcITVlx5C1ZVfLtWZWZA/5dWeCU+Vlt8r7khA5rqZupI1m2fISIA/J8YTYx76mQ6RAAboDGcp0EJLGiCYcpCOlH/l8Ev6QHdiNMYyBHZMmSvaHvyMGpdJE0yYZZHKmMwbDRkdJmWSTguBAGYPgQT+kh6RsQdlmM6eUpf3bCqZ/p2yE/KZ4SnOJLaCA+JFyrAEvJ0OhEsDJJsFlGEIe2Wt

4vEqT+vVn6/psUpv7HGnr+Fv5a/ob+Pv66/hZ8ZNl0/gXQ9Nm02cNZE8Y9XiTZ2Vk2qTIZJNkR8gAmkDo1wXMAFVnq/icaJNna/pTZIYh/fmLZZxr0/iTZe1kU/ozZMtk35gAmstlrblSIqeS82T3+UrKQvKeYPNk2qZcZJ1kx0T+e40nziY/OU1kJYQFJ6aBE2Q7QTNkG/pT+Ctm2mnLZMdDW2UVZNNk22fLZktmfFA6Z+2Zs2e7ZJbKq2fXynN

kj/l/G2tkBmurZFAHg2FJZblyr4Bp2doDOgLB2Zunlvk2e6lla4B2hIiT/7AfpO9jfWWyx1gl/WQ+ZM3GA2c+ZROCvmXpgZOCGYARpPlBQ2WHppGmXDg9e2vgI2tzAWuB/6fOknQjxft4gF8Itwf5ZhKb7tkFZFwTqYlVZw8Fu1H+Az8mIVF5AyBni5AlZlVlI+DcZrFRLWeHkK1ltWZlZHVkbWYbyIdlt8qQU5vLO2bQZB1mlWQy6fpnu7gXqc1

mhWUuq/dmt1IPZjIAvyV5AYCFT2YtZ0Xxz2TryC9nrWYCUatnL2blZkfLbWWBeA1k55CIZLNn7Zj06VCn62SbhhtnnyRNJJtnmqTye2wLj2eHkc8FX2fmAM9k/CLfZGVmnKFlZnVnP2d1Zq9l9WbtZQRr7WZ7ZtvI72cGa35QR2W9iB5YQViCAx5LOWe/JG+l7vAdJ6lkJ2kY0gBE6WXwqelnUydnZqilGWY+ZROnRMeUAwNlvmSXZ4NnWWc/poe

kM6eHpGkk9okHJCalGoS5ZrpwSqCNRAsno2d4R38petMmSqLK42TnpXIZjqbAZzgZjlHwpN+GCKaPZuNQH2Vzs19nQkagA8DnV8hwASDlL2RvmL9l9WWvZNP4b2eMZ2DmGyOUMB9yC2T8IfdkTqZo5fgDaOW6A8QAMVAY5MDk32UXqWvLz2Yg5i9mP2fXyKDlbWWbytjl7WZvZDjm7mE45KDzjWY7BwYkzaTNZo+mJWQUAfjmwOeCIJjlrWZrk2V

kROXlZUTlXFHY5WpkX5inym8FvlAQ5MUoh2FJAmhaEjqvp5DkqWfiiKF4qaAkAiqhPwOTeCY6MOWrRBlksOf9Z7c7sOUDZhdnmWaDZH5ll2XTpxGlv6f+ZKXZYNsjwnkGMcjzwIXFRyQSeiWAFIedgPOlC6Z3ZIund2RJx9/w2huo5hamjlNEp4ZA8QLo5C7QH2cGhhjlbmLk599n5Ocg5ljmoOW/Z69kxOfY5cCbb2YXCqympGs+ULjngiG45vI

YnObEphADe1F85DeTXOalZgTmB8sE5F4DmOWE5BvKPOZE5rfLv2aU5ZplxORU5nzljKWspZO50ibMpRtk+SZNJ+6lCmX88lzmgucxE4LnNWZC5lfLQuTXyoTmEiE/ZCLlFOUi5LzmYObE57zk/2dXCpLk/Ofrp1ZH3qJssxwCEADyArFzx2YfitQmuOEzEUepCrOnZNb65iQpJcd4qKVB+udlkCZ7pxtpcOcXZllmfmcHp5dn06SRp7+nw2azpD9

gTkaSSQQkVMSiJ6anl8LDsKmjQAVaJBfE/SVnp4tDBWcOS37pt6SBGSzxOqU0ALqlCAOc5NLQH2UepnbrkuctZlLlscK1Zd9khOQ/ZdLnhOQy5r9nFOX9+KLlf2eU5vfKuevGZvzkciP85GjmjlG65HrkMVL65aXr+ubPZgbnpWaY5sLnhufC5PNloOTtZfVmDWVg5bLk4ORj65Wkwmkk5LBFFyYS5s2krlD65Vhm5uXA5+bnBuQg5MLm0uX7ZJb

kr2c850TksuW85JVnsubW55mmduv/Z+LHF0Q+xPkq1AE2APIA1YO0AAe5NOeW+R+JVfD9g1WG2oIyCEgGXmfqmjukETjeZGPEIafeZ7ulsOcq5o4aquRZZYNlWWcZgNlkCOTq5MzmdUbnuHABjYeKpHQgeUDto5fSIErOuzko1kP94jfTKOQ65X7qpuUc5XgQ+BH4EAQQ8tsEEoQSsQPuisVmpyUJpB9nQmdmZaZmeGYY5v4E/CGF6EXSoefk4Vg

YuqkbQr/T72Rk5MBnZqmm5EHm+BP4EgQSweU3o8HnfoE0ZSk5Zmfh5cJouqph5OYbh5Dh5JSx4eW4ZBHmeGUR5DbnP8VwhF07MKeFiKHmZma4ZOZmseRh5/jlBThx52HmCejx5UnnxmoR5xHk8iaAJfIkxSqcAfmCpoEQgFz4cvpvA5b6dEhIY4hHHgLKQufS/cIqYxVrdySfpuOl3mf3JF+mD0VfpuIYjOSDZ75ml2RDZWrlTOfZZsNlCqfN60e

kZdkChJlKkcHaUXlkyqSZ+9OoCLNvSAi7AeT3Zx4aH2TyGabm7KXAA+yleuarMzvKbTuEA/vJduatZmVkFOZG5JvIXjtG5hVkumpLmsyIJuafkPhnDLKXyBznkeUc5yXn7KVm6zoLZebFUxeoFuSHyDzmluRiBQ7klOaV5j6k1uZl57wiCearpKTkUoUS5mCwZec155fI5edS5Zjn5eV15pvJMucO5IP5leT3ydvJVeS76GMGLSR+p6AC5NgE6mA

ANAOJhwrl4kihePUzo8smo+ajPEJ9Zw4KZ2e0JOSk52ee5edlPmYLW17ljOR55fDmQ2dq50zkOWaI5qXZPBAs54ckSwo9UhgGReVYknOLWUV9JNrnMaXBZqjn/wXV5BemGycbJUWTa1CGsdkkTea8oQboKci15eepQuSG5MLnzeYO5i3kx8si5fXmK2X3yqpn/2eCxmZh7KGB5CPmDQEj5+FBZulj503mteUE5ePk18gT5VjlrQT15Mbmk+VvZ47

k7OsN5bFlOjooxnFmJYbW6GXnM+epAtzmIOZz5TzlE+cLZJPkJ8qt5n9oVeYL5m3nBeJp5/pKGRhkeNDp5pvoA7QBRgM82qSzQCkksjoCBeVGW+MkDVMTJgd50eLGWth7s0O2RFMmHuZ9gR0mD8T2RnQlvUAPJzU4skZOhI5GTBvL2mhbobsikZYxP4S44nHGtwbuASt68qCD5kPnKqZnpERJ0vMikYskbkRLJfZagyTuR4MlRwJDJooDQyQ+ysM

mnkQ8Gr7IzlqrJWfnFwBrJqMk6yauWYsQh1FjJWvQewI6AzABcthEsPAAQ6nDEKH78pmUUIy7fUU1JDAxExM7xMkau+Ld5sraaADwANICt+Tvxp7kOeVrRfNZAbk+J/z5EyAZGDkZmPOOAjIBCAKOAxwBwCa90cXA3aAOkUArsAFImcAqUAcb4foBhEB/q6yzOaJ5GmhZ0cR+5VM7tQtpaTH494SX62lpbmduZeBHw9qLKiPb3oK2sQay/cQlGWs

lqLhV2lphPwHs5a2EOxjFKP/n9NC4+btIsNAHwNwDWsVFgVybYEWoK0zFGCVLIbig8wB9Z+7ktRjZ5zul2eVP5iM4Abhqh8HHE6QKki/nxgMv5UYCr+ev5m/kYWkGAW957+eUwkK5sAEf5qrHfoKf5OMEX+aIJpZaaFv5xt/ndKI6wWzCN/t3hQHnOSvdUQhjl+ls56Vb6dsAFMEkEKWj56AAXrN2siazUzFSswyxKBVespsw0zFUK+oAMiW9SYz

7sWSk2KdHa4s3gV1hN+Yb4EYCt+U2A7fkm9ECAzmg24ryeGgVPTCoFfTTVOf6SPLSupFJAxg4x2W0OG7yVHvRYfoBugIgKWHaAcTwsbZGexn2IhVqD+dw0w/mKSWfp3z7e+Z4uIXbPKcaI5AWUBdQFG/lb+fQFu/ndovv5zAWsBSf5Z/nWWPQAl/lbRpoWF3GhybO+bGbnSMmoe9GRyWGe1d666BBZHdmLWiS+xrhugId5PIBxHqSpf/nJQP9xoE

mFmCAFQbFuXB0FzYLdBUIp/6kgUQ6gEQWqjsaYiyBNQDzwjiA2UmjZIDE9OcdJv1n9OXTJM/lJPrNxyQXcqYB4aQW9QFQFa/mZBXQFO/kGLHkFh/kmlGwFHAXn+SUF3AWB+WnYscoYsEgi3kJSYorAB2Ru4uqk7krWufH5zm6N4pSR/1STMqDeBS7BrhXkjBRG0DE6mhxaGbnsrszLiusEghQtusT4vDES5AhYWbzXCJCFwLowha8on6xW5AiFZq

5IhZT4QvnW3pdBptmnJB4FBLzeBc6AvgX9wP4FHCBBBXXavJ5ghbuKLwFQhcsA2IUK7GDM8IWjIIiFAvoizMiFp1m8idt5F1nSyo9Kf4D1NqtEBjBugHGqCjBavDAA9iI9+ZysdHjQbGBxh/JrBe75GzG5KfTeZAkkBRw5ReCHBSv5JwW0Bdv5DAW5BUwFVwXH+ewFRQVcBVf5MADJ8fwFotByuDNiaLIMgobo7YkoxH42cfmn0cS+TTH3oP5si0

QdgA0AldwicQAFeyaEGrIFR3zqqVJxGx7GgAGFZ+RCSZMFYQXTBUgFSBKw8ebYzFq5SunZR+npUWjxIalD8V7xRAVMkbqFkdJjGMLaFAVHBRkFxoXZBRcF5oUsBdcFhQWcBfcFtoU78UgpYiSPMOase9H0xG1CW2jfXn5ZvwXehd2JjeIRhcCFEn6gscKmDlrjhTi5O6nAOeM+RgXHJKnRooWuEOKFygCShWwA0oXQQLKFVs4Khbixk4XqeQtJBh

6xiUbG5YC7xI+AJ75+gKlwDhD/MGnUXFi8ZoqFEkYVzqqFPJbMsTHetnkTcQQF/dGOeWPxBSn7BZZkBoXHBTQFWQXnBYwFB/l1hZaFtwXFBaUFCKaaFqIJSClCBcsg+kGMFglYz/n3IGzkb/ktBRfYobboANKRYUD0AH+A/cA0cSGFqi5hhUAFgwWC6SsJac4xSthFKLF4Rb5xXKEenCWmWQpGytBRyZK9sA9a2nHJ/llJMrnwgLmFP1nMOQq5Xv

mfhb9qewVe6fqFZYXpBUaFgEWmhcHKlwWgRTcF1oVNhWUFgVpLeprqKeEJ6ezEZkkwJF44ayCnmQsJw4XwWcsk5crE2rfxU4WBiRNZaul1LvOFJgUG/CeFZ75drheFfzCuENeFQ3YB7gXRxkV7hYm+woXYxMQAdoBr8va0pcC03CkABwjOAKOAqR47CA2Ad4URBZ6pVsl4CVeZ6oUhMaGpSOGPeTqFJlkXVjwAdZzAesmAtQDQxFJAks5JwJGiwI

Z+gPEAOM6lhUv5FYUSRWcFUkXsLjJFBQVWhY2FkEXkZpoWIwmaKpAirtqnYPzCe9EObtvO0vBg6FFx/YVC8Z4RMgWkRZGFfYkkWpWyzoA8gLOU8kE6CZJhUwXhjsqF5I5HgE1Ayan/dLJJjKlXKZkpeAVvhXeJCQWCRYPJqGmP6CVF5YWGhQBFFUU5BdJFtYU1ReBFNoWKRXCJDoVXIFjanPEJ6QDRdQIhxGkuvUWMaVD5ymJ6RWLx8l5kEZOJ1o

RGXumsN0w2zKzMW4kL4ePQa4lTiYDFF6xgxSZFVS4YqdNpY3msiRDFBtDridDFD6wUgLDF7kVjEQeFEUntwNAKo4D4ACaGBwC0RUpZvAD6Fn35JKawFnGS9qCe0kIYgsLF+gqhcUW5SaEx9nn46dsFDMlDOdIqh0XiRSdFJoVnRVVFF0X1hbVFdwX1RQZmmhZ6iTUpU6RvgGfgxUYWsGcAFByzIOkyYKkQSf8Fg0Xo6GRFsEmzZsMeEx6kUuDFfq

KekSsep8m+KXi5DCkEuak5XFmD0luSTFJuBd9KrWgIgvQAo4APBC4+FMUNnnR4J4k1UkVQ+yCmsAhsumFySdlJ7vHAKfd5mwVJRUVJKUXsPn+FlYWSRQLF31bVRcLFV0UKRVBFA+gvLpxI8pAC8SRcJ2AQSM3MPrTNBX1Fick6kt9F5EWzZhtOqowHirNq2wlBSVOsg/5lxbyZidGIxdwhonklxWDMetBgzHOZ7gU7otMA5YCnADq8LsVzRfLA3w

RlILJuBaipqGYMP77yKetFzKn6WRsF/EXahWHFkampPpHF5UX8xTWFIEWXRfJFYsVkVpoW34m1SRgFf8r/ghnFcjkNlvqymlpqYbpFQ0UjhTYBacnJCfGchIWqdAAKDvrLgK5JcKmDjHyFP6yYJvfFqPpKgLXFMvFNuRbF4vneTm5JL8X2+p/FO4wV5h/Fgvptxd9K1QBRRiMud+SruWTF/3T3heKJALbYru4IDdw/CVmFzMV5hR75WoWqiYze+0

WY5IvFfMXVhcBF+QXxxevFDwVXDpoW+km1SewEj0lmKcyY0hgUHIcMg9qR3uhFk1Gw9IXFWsWkEYnQ/FSC+qZc96zAxU9GAtSzrIbA86xH7vmhv8G8Jck4/CUjHoIlpMyPrMK0oiUIQOIlWB6SJcbFuLlAOcbZs4WLKRrplsVdXjoUsiXeUvIlIMWKJSNsyiV5AKolEVKQJbRKtQA8fsB6FAAbNnRFrsW2+SgQSUmUjgqQR4BoxAOCACkssTlJ2C

WahQ95s8Weyd+FIkWpBWJFZUXEJUBFZoWrxeQldUWUJY62mhY1SXdFy6QTdBjoi75qxvRmxn4I9GfFGsXDRbZJjYEKFgBAtOy0plT5RSUNnPgAtKbbsZollt5TaebFSMVpOduwIhbFJc7s76ZF0YH+JdE+SoLO2rytdJcxM0VhBVvpffkPWceZ+wAAft4I4BoErtSRLvl6EVnZ8rlTcYq5c8WhJcbaPMWRJacFy8WkJRaFckXxJbaFd0nNRVOkKM

Q9CAvWK2ji6c5KSt59iIdkCwnn/ocgx3oE2ethzajMRG9O2yjoxUIlmaw8XGIhPoJ5OFxwwgBkjDmEFiXgJSLMdHqVNHsJ+JmbmJgMD/RshXg6u2IN5I8lfJpKBVAM7yV6gp8lQgDfJTBEfyVWJdIhLPTApbqEYKWYOhClankBifDFpqn1xSJ5I+mbkg8lYRCwpRjFbMxvJfi6BPTscN5AXyV+wKilNnoIpShJA0kgpVOZeRa4pViFtsXQ8uQM/c

B6/K0A+AByfgglLiUVRuraDnaDxRRg2NlopgtA92CUolglvEVzJQVJCyUhJU8pP4WiRaVFx0VrJSQlMSVkJWBFFCW2hXxeKSW2QD1YZ2CR3qQxiAWg+UDhXgj/MRvJftrcSDA4noXyBQ6JFawHItXFeEHUgT7Bu266IYWs5Rwa8hUlrzSPJR8lDKXIpX7Ak5gmJcIl8KV3qWSsWoTsjI7QAaXrnGe0iADBpYiloaXfJepCchxahC0ljZzQpWEQGR

TApeaCOZzsjDmEOaUu7Hml2AAhpT5wGaUw7puYpkRfhMWlzcWlpYGlyaV1gPmlaaXVpX7AHoTqQiqAHoROzJGlryUjbGWlQaXtpbG6SKUopb8ln6xMQsOlraWppWylo8BLbjCB3xkl1ASBu2KmTu9AhsEMgcbB/m5EIX6lCaXGOS2lFaVVpYyl0RoDpV2s0aVy5PepcuRxpc3F+6UzpUelHaUnpZmld4T3peSllaWVeQhJ8oRJhI2lfMwwRK+lGP

hzpfSlnaVkjGBO9aUwRD+lrsx/pYelb6XHpWGlR4Q9pfJgfaVFhGelZiV3lP+lKaWjpUBlJ6WopVOlMO7oZW2l76XzpVuAjtBLpdGZVtCrpSxZe7HC+Tbeovl6Jf/F4uEBTmZORjnewTtuO6W+pWaA/qUHpUmlD6Vjpeml4aW6hChlw6w0pZelsaXxpYml3eyzpZhlekDAZc+l2aXQZQBlo6WFpd+lVcVQZdxlMGWPpXBl3Xly5GBlOYQQZX6lza

VqZQplhGVYZXBl3aW1pb2l/aVWzC8l56XCtPhlgGXSZdhlk6VchdOl8mUYZYRlwKUkZT6By6XkZfilIAmX7Ft5uMWPyW5wrflEah+KNhH9JRTAiCWexsZ5Q1jdEg9gm2hlICFCMSHQaYqlsyXBxTPFeCWpIdzFRCU6pdEl50WxJQal2yWKRdleJqUB+nyQqTyg2GIu9OrHIEHOXdqXJY/gfAigBSQR1V54MOrAUQCbpUxBrGWv1GJlXGUSZTxlJm

XfJRGlVmUKJUJlclxWRFelzswlpb1luaXqZbxlMmWhhNzMgmXUpWNlIMzsjPyMLKWy5JqZ5Rxf3F5lD/Ql1H7wvmU5fgF8bWWvQcxlSsEAQT6lYgK3peJlM2VGZbBlg2UCZcNlpiWjZXAhm2WWelNldmVSZeOlfsDYpUNl3ExwpcJlk2XNxetlr8VKgJ2EW2Xv7qRlphnURodl1SXThdolhgW6JYKZyMVq0CdlHWVygRdlbGVXZb+ld6WuZQRl92

X8ZaGES2X79G9lx9ofZXjl9mXfZUL6C2V/ZeJMAOUrZTzMa2UbjKDlPojjZdilO2VB1Jf6d+5W0AdlvKUjyktIy+muEKke3LruqZFlYqVjAPb5PTYKEkGpuAU46VtFtN5NvgM5F0nhxR2+OWVVhXllgsUFZVslosUJJQD2mhZ4Pkgp7w47MExmlWWKmN/KQ0xVmNpa9WX8gk9g+kXQMoWGplyepUiBmOVqetdl02XlpbNlA2WE5Z+B/2VUpWERGm

UPZbWlWuTrpYHURGXEADSMemUcZQZlfWWe5Q5lpmUTrMTl2RFzZSelHuEeZaRlmIWxOodlVPnwSf6GjuWMQRjl3qVY5RD6kGW45YZlbmUE5dEa+wEOTPTl5EKVNF7lleUKgiHlBaWfpRHlKmXR5bdl5eUB5V2lCeVPZVGlE44s9PXlqeWfpZ5lnOWIAPf0GeXQhcSF+GE0SXRl5tn25a6GeeUmgedlheWu5TjlN2Ue5XdlXeUN5Ynl/eVvNIPlfO

5GGYxl70DN5ahJ4eWR5T8l7uUjpcZlceXfJUhlckQ75VRhA+U35eGlr6xp5btlE+XshUgGAWXu3m9iYbB8QCSKMAAONlyhrZ4yYY4w74yzUHLgSAXHgPmg8OiHgD60O1LGmOIRR4BYOFXigUj17kzFsQVyuell8yWhxWqlwkXG2jTQ+FRNgOm+4WTNEk/sMACvoBxGbACdxUygKQDjgD1UywrEAJk+BUCrSPgAE0ZVgEb5K8X6pdrlEEW65ckOmh

YS3lLFjOLJqG7SjhGtjmu238p9iLa8yt5ehf1Fg4XqxbrW+SXP0Xcl0iXfLIOp3xxUrPSe0DJ9NC3FmhXfxYXJiN4NJfolBbw6FRoVahXzSR5FgWU7eVZooPHOgOOAkwD65LISntJx/kFCo2apGBEF27jHYKfAf8pPcLa8GUo8oZBwqMSJZdHuviUvhZtFvclsxR+FHMWE6Ze5PaaEFfQAxBXQQKQVJihITpQV7QDUFa3hkAB0FQwVkwBMFVEAII

aExewV2x48rqoBccWFZTrltoUhyUjZspKuOBmiFqXMmDLFzhEASJzQuSWKFRfFvhFxvMfQ/0XDiUY5fTSZat0V44lfEgMVYJFUrP0VkMVyhPoVZkWjeQ3FpKVdFeMVPRUYkaMVNKoDFTYl0nGnAFUOPEbxAE3xwBV+snyscWjPwDzwffgeFVpBUflH8NwkK2A/4TQK+aDHgJi+cOyYXv7FXEVtCUHFdykhxcElnKkq5aeI8RWJFckV5BVpFRkVtB

X0FREEjBXMFQUVbBW1ABwVJRU/IWUVPBXXRUnF08lCFZ1MPZJHMGBZK7iNFasG55nlIOnp7UkJ+fcSXCUupX5h9yXeUin2NMyvNFzmi+VzIUsVDeSTFck5v8VGFfRl4x6ekcSVlJWVkdy5h4X3qOP54GakLNtEThWKkBMgEGmLCLMFVyYiJCw0qLKLWHsER5lYYGGkXhUXwhJmulYPFYApeYlYFS8VGWWJBfglpAWc6EEOCRUkFTtAKRUUFSjI6R

U0FZTg2RVAlbkVIJWsFUUVnBUbJbJFDYUVFYpFiCkmpQ3cpJCjCIu+aJX06ssAiCUyFUqpA4UqqbiV58V25bCpOwnw/M3FZhXHrEcIwPytxRXFMnKlxX0VKPwRlZRlT/EjebSVMxXjeaJyUZXBlQeKmtThlXzMqxVa9JKOHhAP2BhWPJXBgLaUgcYO/Jr2kcnipdKQ7zithkE+Bc7SwscwqlZTVJglmBWAidgVKqW4Fe8V88WZXF8V2pVkFakV+p

X/FUaVgJXfoMCV+RXmleCVxRVcFZslNpW8FbaFVT73SR900Dh09hBpRfwSFUfFbdxpUDu4rRVyuP6VqZX5CbfFMZVopQ/F5cVIqdfF6QwHlSSVCg7AJRAlU+WJkQKZ2Kk3ycipR5UgJdXFh5UtujmV3VB/gFDqApFk7NsVZMVn4uXOMqGpBD9ekgVxludghwBfeHuAwcT/BJu5snBd2lLwhEwhFVN00rkKlbK5rZXKlTgVbxUA2c9583E9lUkVOp

W/FQOVhpU8IMaVI5WmlWOVhRUTlZaVeqXTlSLFs5WKRdUpC5Wykg3IxjK29KuVZrlu4qDkeECx+V6VchU+lZbgeJXQqYQph6FH5YP+H0E15UbQMZVN5R+lp+VkrFJVolUV5b2pbqWPImJVaUHcjBJV8lXupSfl7KVyVZeV0lXJ5XBl26mmRTSVhhXJlSjl7fYqVeZOPuV05X7lpTSaVSpV2lULpbpVSxX6VfXlr6lnWf1e1hWaAEBg77G4AI/sRZ

VxAFUgB/5ykDaxYFWaViNRF2D0qZnF9ZXRjh5QMwAkcBeZlynyiZmxr4URFe+F5+nRFZfp8/nlaJqV3xUEVf2VVBXEVZpgpFWjlSwVlFUQlVOV1pV0VbCVDUUwAN8pEjkmUssgGqQQ+d62rpWG1lBIoi5WSVIFFgGcJX6VP0WlCksBMiUizOSVPwgxlYYlw1UslVIlbdB8JRNVRJX25GNVM1U/rFSVt5X8mbRlyOWNJYNVhThGJYyV81WXleNVS1

WTVdjFBLGeRW5crQAggPGA3Og8AG4hRZWAVaWVmIqgVX35O1B8rKwISjkMWkSS69iQcAyEiVUIZqlld3kYVe2VWFWDObEVTHZ4VT8VBVUGlZkVEAAlVeRVZVVglRVVVpVrxUVlScXTvg6VXMAjCBim4hVmuYnqtAqZJd1VHCU4EoJVUYU8JajlQQDtZXJ5JIG95YOld5QxlSdljlVbgM5VahVCVMxw/SEGVd8lSlUk1czVVlW3hA/l1NWXlbTVMl

U6Vb6ZoZVFcvzVrNV+wEZVhKV1JSA5w+kplZJCaOXk1bBBNlXWZahlklV81aTVLNXApQzVwtVM1VEAilUflca40JUzlTVV9ZFW+dtI/vARBUpRJMmgcYAcUUVdnumWVMm9OdPFOBWFhcZZXZXPiUdUdvAsyXrl8MgTkaUhTyDKcQkYhyAK3nFo78rYKdiVasU49hGFTWUgscF44sm9lqcGmfkRwDLJw5ZyyaOWCskF+QyAyskl+T6waskgCCjJHw

a1wLrJ6ABPkV5Fs8o8gFKeFgCJABQMcAlkDFGAPkRmjC8RAHGHmj64UUUVRrroUQVMsb9VsrYMPg426VU7RZlVdSYfFbPI6dZtdG6ADCrxALOUpADjgM9R4LCjfJ5cWyYQAFGAS4V0MlbicQKSnvYVNAZtlBBGL6QDpPiIa4V+gFTIFAARgJMAS5lR2L5KrhAYWoZAV/lGAF2qS3qn/q2GRyUzkQAZhHyzUHTwnpx41QCubQVr8IE0wTS9BeY+1s

ZfRfxOqVjDBW9iP9UY1KeS43T7INa6y3peIHaKbdUhoCgF8p5f8GjESmRa9n5GKqgSvn4lgcVxBa4uiuVbBedJ2tHFhdIqI9VsRuPVk9XT1a9h9RJMWMNWTKBL1X+AK9UGQBPBPAAb1dgAW9WP7Duu3aJ71Yc4h9XH1afV5YDn1ZfVB37ixTfVfAUIlTXcX8iBEugR86Szkb3h7NKXwn2FH0V/BTaJvVXaRcA1/VXuZpy099Q8tIVq6gWnNDHU9u

GXQALc5eqmxcNyBgUi+RM+xgXiJkwVxwDl1R7ANkbV1XfYUAB11VGijdVz5RIAmjXctCBqfOVa9PGAs9JNgAVxQgBV2K4QxijHAOJckAq8ke9O8lHKhZO60TUd1bDhLZXZKf9VbumA1XlRQ9WP6CQ1Y9X0ABPVgUoUNbPV1DUL1XQ1DDVr1cw1yyCsNaVE7DUGLFw1B9W1MLw11uL8NXAAF9V0gEI1m8U31RUF1RX4XK+ArxDuUJrFBgEReYbWc0

rutp1CH9Vn0bxx/QBaALUAE+rMAKzxhEUWPoAFQ4VANfdx3CXkGv6S/oRUyJM1rPFr6ZGSwUIMRbYI6E7diBjoXnjhUWQxgBwoVVg1GVFKla7pZ7ku1Ww5RDWC1hk1ZDU5NTPVVDXz1bQ1y9XMAYw169WlNWw1O9WcNcil3DU1NSfVdTUCNU0119V/gBs1SClpeMGmFikOivd8imjzsmN0A+F5xR0pqeqWmEpkNyVgBQwxVB5EeiIe4fZEDjuE9D

ptlKrgRElBml5s6XpmrvwlWQwZlWGVV5VzrMeVvRroNM2aoxpYNOJZSxqalAXqzIVhJs729tD4tcQAhLViAMS184p9NPi1w1WUtW+VLOXRGihZjLVMmtMaLLVMWVg0K1VD6S0KvjVcGAE1QTUhNWE1NAYcuprx2wIctbomXLWFQDy1fLVoSWRAcZqktcK1S1WitZeVz5WC+vS1pRRStbRZKBSytZJZrJV4xVUAKQDieiIA3eiSzmmuVdW8wH6A44

DEAAvpqU528ZfWdvit1WMAe0lI6NEFvJbTJUApODVMPhlVBDWz+bsx2VXpNSDqpDVZNeQ1TzVz1TQ1lOCFNe81xTUsNd81HDXBylU1PDWAtWfVDTWCNaC19oViNc+CZ+LoOANRiBKxxD9EcRg00G0pSLUI9l/VpQlCAE2Am+BhQCFYMzUANQXFCzXotc1lAlHGuNPgfbWqJBxGp5LbNZ7GN7oZSnDx9yZfyJmFSVWcRahV3EVrMRqFfTn8Rdc1T3

nkCam1gdj3NZm1jzWUNTm1BTVvNavVTDVFteU1PzWltX811TVH1RW19TWNNVfVZQU31S2FDpUCUjtokmbc8Z9eCCLtiaahdsCtFWi1u5UaZKUlkTa7hQSlJX5mxY/OcvEmBe61ZOyGhMaA3rU8OO0AfrUBtUG1hTba8Wa09iHcSUFlG2DxACCA5YAUAG2UadgcAO0ALKj+hBGAXgV1FlE1gfD3hSqF8GbgcQk1p+m4NU+8DylFhWk1x7XptZk12T

VT1dm1+TWvNfQ1BbU3tV81d7UltewuZbUAtXw1wLXvtVBFN9UeCU423fh9EvYCaqLGoM5hfGxEfIqp7/lZLj6FrrH3oOb5vUCEyMeRTNG1DqGFKjS7Wqi1ajVFxdoulbI9MCwkf4BmdauZeti7NXGWahJMRZWm91o1pvKVZzU8RWllSTVXNZllc/m++Wm1o9UPNYJ157XCdXm1V7UfNSU1m9WSdZU1j7XltXJ1VbUgtR+1FkaUzsUgvYjttYFIjk

pmue9Ek7FuEZ21AVnhhaO14HXACUdlb6YKtVtRr/FTPkR1JHVkdbbQ8wpUdch+TlB0dUGiejFhYlV1M7kdJXO5lmhNgCWwj4B4RaOAeQAfgFGA/7zqDIfA8YDycSG1qC7SCox1C7XDcdG1O0mxtYqV6FWXNdP5SbU7BaF1BVE8IK0AKGLQLmfkSTHOADqqLhBHdLvipQLBhYvVcXWFtRJ129VSdd9WMnXPtWl1b7XNNSOuPIA31bdFdbWzvu+Ajr

C0WqDYUsipyitQmDhh1eCpOJUCVRV1wPFuXOMAVMinAPPS/cBVFZs1i3URjOKltQlnAILw0IbXcatFGBWy5ce5t4kK5Zx1rDkHtcDVF1b5tde1nzWJdY91yXX71al1QLXpdQp1DUU31ZLFTFXPgicwIhDQtS5AgyV6kQu60FLaPirFALER1QRaNnWLNfiV51I5yby11gD8tdcJvwhS9US1VPpPxYGVmXLy9TL1ZEAJcqr1xrUggNSVjbmmVSSlst

WS9Ua1BwkEtdL1WvX61Wvw0wB1MFJA0wDg6m/JZMXKhdlm1tXcKkE+8ZJ0xUmSzh5+dWEVcuVpVdtFeDUdldhVXMWC1hT18XW3tTT1u9UpdbJ1DPXvdaC1tYm/dWBwp5lWIJgYj1R5pN/KqpZV4qc2oHW2dUs193oMlQ4mesVTVROSAaIlkbV19SVmVRtVSx66xTbFLrVBZUI4wmjFiJoAd1n29VTFnsYtkT02+rJGQbiuqqL2oJjpyPETxU7p3v

Xwab71xPVK5drRZPWZXMH193XU9RU14fV09ZH1lbXR9Zl128UmpQ3IXdqjdC0eb/m8yT4Sb0TgSUL1yjUE1dD1dnUtZXrmNVCYJloVdjJaEKf1OvVCeeShZfXGFQyeJ/UV5hYVOMU/5TFKOUU0WDrQX6lztWj1YwBqWa2RD/CKELKhlQJIVWtFyVXXKQT1eUlD9X1SO3WcxWP1MEwT9eJ1U/X3tdJ1EfWvdVH11bWZdTQl37VypUFVEfksgin1Lw

6qkp44osGKNd6VkPWlIKL1Y7Ux1Tn1BZ5smc8lI2W6TPrF/DCUpcrVFIBX9YmVevWJoaJ5Bl4MWUtl5vX3qHfsrqRCAMQApnZf9Y715I6qjnysHkINeFYgQhChFZZBqVWD9UT1UA39kXgVaw4pBfANVPVlNWH1vzWz9agN8/XoDYp1Bi7ZdavYM0xpWIu+QdViBXaUYBWC9falKLUH9dn1BIksKTTMuhXmFZQpVKwuDcLVo0nOTruxCZXUZaSFoD

muwVKB7g0hlaS1T/XHVVYVF1n2tE2AtwJGAB+Aog0SRkH6v6Spoh9E5KJ1fOu1dtX+JUqlbZXJNSF1OFXqDXd1CA1aDdP1Og3/NXoNr7UGDcz16BqxyrlKPZI8yTKpG/UNlkiM12TTrsM1A0WR1fYN4vUOKWyJlIlGOd7kHLnyoOSJLokO2dyMpElsDX4Nk1kBDewRLqFDDSz+4kyjDd413VApAFJAlYh/gN3U5h4RZW512WaP4L+kcZIuKJWUDd

n3FSANG7VnNS7Jt5l91X71KTWj9Tx1x1gaDQl1RQ1IDc91KA21NfoNGXWGDSVlcfUA2CdgzyBoxMn1HFXHEg/5DGn1MaQNwvUqNdBIYvVCVQoFsuLsSdCivQ2ylOGJJEnkwNDe0I3RfH0NyiHQjWMNJIUTDTLV5lXwjc6JPQ2sMXCN6I0LDca4h6pCAJWq0HrxDdFlKF6YBTSpSt4lUGgVWhF1cGx1+AWQDWIqLb6dlUslo4a3DaH1xQ0PtboNzw

3lDa8NlQ0G5Q6VZyH2/AnpsrjHRmzk/NI79bYN+7YUDeB1cxUb0FvQPDBF9vww9+ZEMPWACAAUAFvQWo0UAK4cf5Qaje3QaMUEjdxMxo2ThBOJTmoqjVlp5fbqjdx87dB6jbqNTRL6jY/2ptT2jcMVsI1mjSsVJfXS1c255fV/RVaNtDDnaSv2bo0y5g6Nzo1OjdqNBo12jWGNHo2mjeJM5o0TFcSNa/D6ADZY8tjICfAlouWbDRJGG7n1lfKQ4r

mVXF4Il4nWeet1aFWJNVt17MXQDTEV1w08INyND3W8jcgN/I0vtfJ1H3U8BTfVghVs9fVCF8LltLUF1bTMcS8Oke5XwjxVenVLrm0NIvUdDRCNrqVq0LPguyhMAJgmfIzcfFLA7HCAgDONFtzo5ay0gQDBFIwAEYDh0K4cpO4Ljbtiq406cPONNIyLjZwAy401MPsaa41nZYiBm41sANuNu41Z0PuNp40+jTolZIX+Sd11K4nZvLON/eYX9aeNMu

ZLjQvQR41zjTeNDIF3jQ+NC9B7jZ6BB43V9dYVCIJnljKx1liUjVcm4inMeF4hd5IZLnAVp7glJvj1U8V8RZhVuQ2B9fNxdY2IDU91qgEvdQKNLY2gtcj1sEVWJGgYNNH05LGBGCmSBj1gmfXgjUTVR/Ve0IL6Tiapcv5AW4C+9tGNFLVGQFbQWoRCTfcINIxa7DicfZxPja1uOYTTAH2lmAEizDxNW3LOQPxNm+aP9mJNIk1iTcqCh5wlnDVQFW

5yTQpNr42I5e+NFqnbAmvQ3E0ewb8IRMLEAAJNGk0itcJNd4TaTXSMuk3tHFoQBk0LpEZNGvl0cFr5dsUmscdERz6uNSj1YbVKhZbJb/DteEagAqhBxt05zI3y5Y2+w/WqpRyN6qVhJSRN9w1kTT8hFE3NjYz1rY2B+TfV8JWdjb7w7Ylk3oJOuA0NlgyYdci7ZK0N8hXtDao17E292Ql546m8hqIg3cVYAG/SaXl/PP50toSC+js0JNTv3Ho1OT

TBAN1NfLS5VCGNHRSotJr1ArUQllbkQrX4lEqATiZW0IbUxtTMtRM0QEYNTYc5BenNTUlAb9KYdF1NIsw9TcFUfU1ZNGc0u00/rBc0I00x9vww401G9ehJB+XbFDNNlk23GYtNmDQytStN8ZW7qaX1+vUtuZ1Ng017TecsB01aPP1N2jU/TadNw01KVKNNAjpCFBNNN01B5eAm901KTXEgC03oNEtNL024tMmN96jgCqQsbqS78PENXhUpDVfCFu

WFJkdgTQWnRl0kAITdgnIKuiqj+Bjpcg1g0QoNZw2sjRfqDMFA1TWNmmCpTcW1tPWlDZRN2U2gtfOVeyWF7hfiXggB1fUNLYmsfo1GLoxYlRD1II379bVNlA0ghdQNu8n5CYL64QAgFI8lkcjT0CdNSoA7NPehIszKzUwAqs1STJGVis06zR/F+s34mo/UQ01pfGbkxs0qzYxQBs1vTTOFJk2TDdNJAZWVxUrNJs22zWbNGs3LgFrNp+RuzTbNS8

B2zVXJ4cELDFJA5vmjgFZQvqbvyepFG9KSyBBpI0ySdhVGoCjeIcjaReJOPNvyVXzI6HLANZCpDWG4OE2ljfBRFzXxBRcNhE2wDaeIbM1JdTP1nM1ZTQv1hg2MVXzNrlnmia0+Eo34DdQKa76rpDtobE2yzaOFnE0P5hl87Fwn1HZNK/b2VcmZoeVajLdNU01UtV26Lbrs1fdhYOb9zUk0g80XTcPN1ygAwnyM481TTWK115WlrBiN0+WBKWL5bj

Xjknp8882B1IvNao3Lzd8oq80STTDNE83F5drVNLViJceV7lVChREN2MTyQaZ2YWRtZMhNcZaW1VbJ5qU2JDcVbl7J2kcNGQ3YNYXNHHXKDdsxAfWlzbPI5c3aDXyNVc1vdRUNwjWvHj1mKBJ52Ov1Is36Wta6nQjCBewlsXHSzWCNXc2Xxb9FXtAG5Pq8vSFezf3kiLTJ1Pq8XOZkLfEJvRWULT7N9C07zXeVa1UPlf6NpC2hAAwtueRMLdQtH9

TcLXwN4spjquOASapfzX35aE7O9Vu5W3okplMA9yFjxSAR+c1PFfG1fdGJtSoNSU34FVyNBQ2aDezNlc1PtVzNNc2VDSjVHw3FIKgp3MBF0qXuqzlJ4EyCTrB2pUxpgDUyzeB1kwI7jPiqRCoohc4tZIyuLcZN5jXq6etVd/VNoIMCXi2wTRdZSw0QclA80wDnalHNhtjX4M2hEOJVxrb57zgFoGG4IrqqPuph4OFrIAkArCW6KlyWRpEO6X31R7

l4TcqlOQ2qlVllQfXaLXcNui0lDfot1c1ILS01d+xUhlJoS07VtMiJLw5PYEQcUPZVTfxV5A0TjRxN8s2xEqO8jC3AzZrN/C2uLcgARbxc5gMtvC1DLd7NIy2sqifk4y3eLTRlHFmz5Z+NV2Fq0JMtKfZ8LWl8oy0LLcEtIgp43sxwjoAO1QZ5/cWexs75rZHSYWuGntLaWkE+NM2waXTNJ7kMzV9q6i1QLSzNnwCwLQ2Njw1NjYgtQo3ILQN+EL

VZ0otQQs0ruCyEtqyg+OxsUwGldds51nU9LSNFLxLFgE0SUiIeNcdNBjUMDQXqCK05APKEyK36NcERhIisLatVyy1+LfSV6aAYrUitgM0gakItezguMTHZGFaNOU31qYl9+S4lx5nOKPwE87p+xcAtnl5ljex1CbX91VWNWVVhdYHYHy0PDeRNTw01Lb8tdS0fdbVJrhHfnI8OfY2YLbMmvkJjKMpxeC0X8eONji3qNeLkmy3TLVQt2y3xajPpyb

kwkVsteKp6rfXpiy3+DdiNjSVardg0v026rY+q+q17LW5cjIC9RJr48Tx4PlHN3/WzQNVS8p6qEdu4BqCjxdgFTslKLZkNgXUVjVEVfK1OeUe1Nw3lLTyNwq0ZTaKtPy1M9cI1FvmVBR0IXSRR3Pawvw2oGKZBmamdzYqNeDDv8h0KV43HjaTUK+QwTQX1Ba2kLEWtIE0YMKWtL432zQjlPi1I5Rwt/i39LTAAha0/jf7Uta2Ure3A9hhZwGrYII

B9JcFNKGz1ssWwuY3cKnzCBWZfAr7FDgJ49UGtoC0xQnK20CAKtkXNCU3+9czNbtWniLJ0nDD9wEGAPID0AF5oV9EepJQMq4CjgNNFkABNAKv5pVJ/gIyATQANAMDF5L7CDdh+LYoR/pAADlBNgKNEygBugIQAxwBcWMtImgkICcNe2T4p2CsWwKRsKM6Ayi6OgOCAVwDeRhFklzgCgHot9PUvDYmtLTWvYUt6aRh/gkx+mNm7IEpoBbYrBbxV+c

V+2qiyV8KNPr0tjg0eZs/FK5z9TR/FFa3qQIkASvWVxditus1lFG2txbx0bWatWI1+jS2te5VeZoxt1G0sbbRt3a1VAPhUQgAggLgAPAAAiIYuI61zQF/JV+AgQjIpCVWMxY7JUlKgDRtFA/WziPK2V/I8rcXNJS352YLWW614Rbut+600gIetathNACetZ60hvpet0wDXrbet9632RqFk6haYokygb60frV+tP61sAH+tdoAAbVJAQG3iOCBtIK

QEUBBtUG1V2M82f4BwbRzN1S0JrTlNVCWyQfr8ggZKaM+WLjgHZNzwlxIldSQNfFVkDaBJEsiVsHRWGq2E7IAllG1HTQMAH8U8gADFghz8jLxtIBQVrfRtMnKVbUwApW0LFdIcFW1UbVVtLG34rYq1f8UHzeRtyvXEjHVtpAANbXQm5W0fKC1tL+RtbWjNlmgUAJgA5ljKAO0ApumfYectbsUB3ovoVXilRnsA9snNlbhNz2qLrZfyt+HKibytLy

3rrZyNPab6bTut4wB7rQetRgBHrWZt6/kWbRetcVDWbTetd63MiA+tDm3Prc5tmADvreCAn63frb+tKrJebcIRPm0L1XAA/m1gbUFt0G2hbeFtCG1z9YKNyG2fdbFtuyWj1vY4eagauOgpzJhNKe8AgUjBgC2WnS2ZbZzQ5SBljOB1G06MbVRG58390JfNQhTWzXrNHs01bfkJxO2jjpPNE8JaVcW68EJ+zVTtAc1orRol8OX4ub6NnW2rLQAlFG

01nP1NJO16VQpVzO38jKztUQrU7eNt96AtdH+AxoBz8isWkm05jeSOrtpM8LrWF+KdybpZsU3n8kutmm2qLfttkC2HbclNxtonbYZtF21XbeZtTKB3bVetj212bY+tjm0vreAOH22ubT9tHm1/bd5tvm1reCDtgW2lXMFtMG1hbWkeEW2IbTDt0W2JJbFtvM2I7d34tlJ80psEhujJPKkEBzYqrWONnCVthWlQhO1PlYxtFa3fhrqEOZx07eAMSY

RHKCNtzG0ISqZEqKV9bdVtMIhE7f1NA21URrnt/U1Z7UhGSYTHAF+EbG2nlQVtgu1FbQgADe3gDArVxYSF7Z3tvIwF7eXt/G2l7b8lw+0DABhEVe2d7TXt9O117Z3t3e2vKE3tLe3tbcSlnA2kpVPtXLRFIAvtnRQ57f3tm+0DAIPtw22d7XxtJe0aREft++1MbRXtSIgb7ffUM+097XeEe+331Nvt5oLN7TBEre1BzbYx2MSTAH+A5vkjEP3AH3

WJ4QtttvlhTezw9ggKqKgS6UkbbXOt5zULrRptu2146WGtB23K5Ruts8im7WdtRm0mbcetN21W7VZtNm1PbQyo9m1PrU5tlOAubV9tbm2/bf+tAO2e7dgE3u3gbb7t4O2wbYHtUO1lDVRNH7WvYU3xAK2cUNHwUyaoGJ7S14CyjfYtBcUrIB5Q6e0sjLiArACqTfKEvgyMbfQtSUHxAEIALwGvAY/t2TQn7RVBUERCAPRt4h18TVIdvW39TbIdI0

HyHYod5+331KodDYQmQBod7G3mRaZNYDmvHGOEWh2SHX98Mh3cLXIdCh1G0Eod4+1qHeYdgm09QBvEuhw7QJEtZMWaHNTG5I4zIEtFv/Dtzbj1Sm3ZhSlV4RUgCHAdK60QLTjxry0oHZQJ40IGbegd5u2mbZbtlODW7Q9ttm3PbYQdDu3vbZ9t323ubZ5tHu1A7bQdYO0hbYwd8G1VLcHtrB2Kda9hkq0OlRdglkktVaeeWG2iECa6yZLg9arFe/

XIxL9gAfAQFXmtyKHOHYfmOu40icPUjG137W7mOKHjHRvmaOaTHVyJ0x3V7XKECOaWHdMVn02cLWrQ+h2LHUTmyx2BwcYd2TSzHf7m0u09raYALTaMgNBAQU2AHVJtlXxW6Z5C7gjwRVjaUB35LWB+Nx7bbcut4C1sje/+Gi1qDRqlsphpHadt523GbZdtWR3YHTkduB227QUd9u1vbSQdzu1kHa7t5R1UHZUd0ECgbT7tkG0MHQHtdR3wLZFtaA

3irXDtzR3B+YJku2Be0aDYXR329MIQtsB2LZ9FQh3b8tMmio2ciQ1BxoTH7RGABQDKHUUgpx35gAgyzJ17KKydF+1p5BydMx3rHTv0PJ2bHUmV2x1cbbKYfJ1gqlRt7J2cnQMA3J3eHRhImsRysRPBxTEbDU8YUm1UObYePKjSlf1CFyl3LcfpsR3qbbrt8B2RFWothu3IHUdtTHZoHaCdmB3XbaetOB33bXgddu2vbcQdPCCkHaUdFB3/bYBtaJ

0YnXQdWJ01HTidQe3Q7Y0dzPXNHQjtnglJ4DltFFzqxgB1rH4TsBYayCm6RS/A9jztFUgBcbyJjcOJgwBfQGf16AA5nfKEeZ25AF4NpuFzKYPpq+1nCaJ5RZ16hqxA+Z0qnRAAl2itaJN1cHjzbTqdVKmNcJnE2uj0xR9Zxp0KiXBpZp07bQkdvx1MzTadxu2jhvadGB3gnVgdzp1Qna6dMJ0EHXCdnp3+8YidPp1u7ZQd/p3AbeidAW1BnX7tEO

1MHfUd4Z3czWwd8QBGABHtMZ2bQGJIDbSvSSXi6O3anRKupJI2DYIdhG0vwE9gbDZUDWRttZ3TLJyqUCBhAHzmwyzfnfxUv500gP+dZZ2AObUllZ0fTWvtstVAXck4IF1gXY2dcADQQFdYhABfrYpZWY3anRJGP81oOAtAbj5rBitFZeGrBdrtcR3mnSOdjM3GEckdtp0XVlOdmR2znbdt0J35HUudHp2O7d6d5B0bnX6dgO3bnYGd1R3+7ZDtR5

0sHSedTR1nnRwdrR3FUKIk+ugc4rPWqOhWueltBG0otVkKy5XgdYuYOcIXgBGAjADs7fACKl1H5TuNz+Vp5KPQ2l0HIpf8p+UFndT5Oyg6XRpd0uYmhq8oql2cALpdVOVaXTZdOl39SQul4F1nyZBdfJkdbXSVXW2GXY8i6l0ezY5dlhw6XcXgwGWBXeZdRl0uXfaqjZ2ZTVFt60kAcmA4T1lIBZItth4hQh3RlskgLXCAbvnxRfmFnvn7tUq51w

3XSQH5MW3onbZKvWDt3OSdkPaKbZH5yan2Arv+rRX7mdHVcs0K0HHVW5EZ+RUIQ5aaYCOW/8BjlhnVGcBF+eeRs5ZJ1erJX7KV+YXV1fnQAFuKdfndUPoAboAQymU2doC9cZhd0drZZtLlqAXjIA9gkyBrzq8d6Q2crQXNm3XkXc8t1p1XDSkdgdjVAIG1WUUysdKFF75iYVlFjIBxLNEARqGQAOMA1aAbyMc4cMgcAIsQLXSNME8wqSxhnYJdhi

1JravptUl2rJawNimVZcQ2Xy4Cyo/48clQrdIFNU2ELaId7e1lydpyX47WTeXJNO1eZmi0vE0o3f6JBtnuXfgBUF087d5dfO1sHALtyN2PGqjdaLSNnfU2NaQwsGwA+knvyTz1tvljrSldyQ3g4kH6aQ39nTEdam2PLUoNo52UXUbtmi09pmddWSAXMRwAV11KsZ0Ky0j3XWwoTKDPXYFaMABvXWAWn129JT9d2nbMHQYttS1EnVzBELUSyE6UDC

UruDMgHOL4thLCtJ1KNba5gx2wrQUlBJUnTDMNceSojfMNjA2kSSiNspSO3XDFsHUIxdBd1Z2zFX0p+I3DDf0NWADU3elwUAAVNmMYzirK7d8EjjAJACjsLWHFjRxFGV2nDbzd8U2JHeyNVF0TncLd511i3RLdN13S3ZMAD11y3S9dit1ZKB9drqmq3a8Qv10a3WKtsO1tjWkspV01sOMJT0WoiUfFSBaePrmteW30SQhJzt2ejeJMewkB3bf8UK

Wd3XbdZv7cTL3dnzkDDRKdHA3e3bLVOEkDSV3d8Y0OTKPdbt1HVbO5dwneRD72XejydLZhQ636ytlmjHKJBBlMJY3vHTMlf1WhrVadSR2C3QCdYSUi3Rdd4t15yJLdt10y3Y9dPVCF3UrdJd1fXemA5d3q3QJdmt2EnTXd4jlIKR/IZV0tLYbdDE1TWqFGvxECHXSdhG1W3coVXQ2T4REgwebX5kvNIu1M7d5myoLG+nfN1rXbzWFhQXyIPXHmZ8

0oPQ5VYu0gBpvNtLUvlRLVHt1EpV7dQSnSnWvQR83P5kPNhD0jzeTtGD2ktVwOD83kPejBmvknVenOOFDL6eWIhWGJhRvATN0VRjhdjRDsAVWwHIJn4qq4MmiZSQndg51J3dXha63jnULdTHbX3Vndd9053Xdded2y3ZTg8t2vXcXdKt3fXZ/df10/3dXduU3QQFvdELVknT/sGSXTCSQ2Ml0IGCeeSe3VTWqtCN3t3drI7UE0jMj2P42mXTruGW

58jD49FtxuXSbFWiXc7W+NTs1Jof49DUHePVWtg5iNnfeNkwCuUEYCyPWM3dlmNvntsL64TUBVziNMeECdArKJ48UqbZPFTDlFLWe5lw3JtURNKQXqPZddmj1S3do9+d16PS/dhj2l3cY9HXRf3XidDR1CXZGdyBFjJvwIefQWsPY9zoqosppaZt3AjQMdvpXqrYf1fS3oAHsJzt2RXaZdsz1EifM9K+3UPfvNJN2ucIs9lInLPecdVQBa0AmApw

BNADUG9vUiPUeaKF4Y2R2hFnmwNRyWXN1gDYUt2Q1lPSXNby3lANU9t93XXXU9j90F3Qrdr91GPR/dbT2mPVXdoe165bJBQU1SrSNMU5F2PYmdq7LVmAHCf16w3T1VBC1gdR496aDO3f2AW5o1UMZd7KWmXSi90sCbjfmdkV1qhCs9RN239cSt9jFEiai9uL2lnfi9YQ0r3QbpMdbxgMwAxwB9fk2AdvVLXbPK6T3iDTT21d7/eOzQWdI3Papt4A

2sxecNq63lPbt1eQ2AnS892d3vPTo9T936PUXd710/PWrd/z2xXWwd9Y7tNVUFhX5fyDIJjCWKjjPWnOIGNBH5Lj1dLQMFkz0ODSKG2L1ovREgYxaa9Vi9ZL04vei9OwhGtSE9NSUE3Z5dVZ00PSS9EgDmvRS9PYQOvab11L39davd96A+9g0AYUzVAOWAUKSk3DAA4dihZIqEjIB+gJHN83WhBcI9MTXN9Sx1aoUkXUOd3x1abcK9hE23NfNxtw

JKJOMAOdhfbU0A4rzVAGws+gC8fs62C1JPXU098r0tPb89Fd3f3QC919UksRORn3i2DNhuiDpiBVnN+qSQPebd1DZ+3mvw8QDbxBM1xAD1gp5RJEUmvZ0NFEX+kiO9ygBjvRO9DckZLZxQwzL2jJoRHhV5pJk9HPChIbK4Mila6mE+conHDV71Ar0JRcPxyj3HXdRdmVwFvaVExb1ZcGW9Fb1VvU0ANb3P3V89zT3v3Yq9ld3KvU0d6w0NVbO+9S

kACE3Bht2DPVNagRWNQmvJQI0ZbVLNlt3TvZONLxLIel41MuIIfTo19a0PtmY1Sy1zhS0Kwb2hveG9KrykyNG9LwBigPG9hTbIfaitOz0SAEN1FNzJgFWI4G3TAGiCboB5MDs+kwArFiKl/mjAUfQaKb3cVY+FSm3MddAdid2E9cnd/N0oUYe1Aq3HWDe9Rb1b8Pe9bADlvQYAT70vvbK93z0NvZ+9zb3fvZGdG9EFTXAY3149WGpFvFGtwehw7p

UEvnJdBsbdtd5El4aOgMeA2cB/1ZO98zWwfaRt2MTJgGZ9Fn3iOYzdAjYvOAoJALaJse4IIwhFjfQ5YXDRHbc9JT33PZWueV0DkXm9KQXifXe9pb3SfY+9sEDPvZ89Bj31vR+9Jj1fvQSd5j3FXZqdf72prRwExyC+CZ5Zicqoit1M3gh/tTjt0H0TPe49Uz1kbbq1CB7l9ni1ghQm9Qr1JrUktZrU+LVpcupArk1QTeVByoKkPRw9gvowRIjZ1X

Ummkau2LUyHkX2tX3ktVDNjX2CtTTMLX3lyW19Uk1HnPpNrW50jN19KiXHlX19E93CeYixlH2ZAjR9f4B0fQDKjH2SAMx9cxCFNlV9OLUx9mN9z2ly9ddNk31t9kK1dX2tfaJA7X0yTZ19y31WtUiFck3kfegATQCwXlqYzoDEeJQM0EA4VlEsIUynRJjqUTXJXW7Fv/W4CdG1z4XyDaadij1hqSK9nMVhfYCdEX2SfVF9Mn2VvbF98n11vcrdSn

3JfSp9qX2AvfwVskEknRyYSzkVgQ0NtNGnYHTQt534bcZ9voXtwC0AQgBu6GU2pj4UipXx8o0wPZJx9nXfSiz9bP3sUuyKrn3OAGSSS7XphQjxa7V8vWzW27XZXTglD3khfbZ+qP1hJej9Jb0PvbJ9OP3xfXK9+P1JfX89KX1IbST9VcFfddBAojUafQ9wlSCHgONioD5VXQV9+aja6ICNS9bm3dYaCo1IvZjYkHVXzLh1cOXGVbr1HzwIdeImP3

3dlEKxAP1NAED9tQAg/RQAYP3kglrx0HV+ZZYVL/XipqsAzgDfYk8En/G+RHDqzADLAHDqHKAQ/Zx9bdExRQe5R91xtWAt2b0p3X8dAfXK/cbaqv1SfVj9cn1a/Yp9uv1NvR09x50A3Sht4LWijTlK9H66dSxxvwmyCUxaCyYlfYXxozX84CaAEHJ1MMGFex79BY4gtn1wrbXx30pyvMaAo/1SiML9DEV1yB3xzEVd8WxFPfWBrYX9Zn7edusF+E

3tlYr9uPEV/aOGVf2Y/TF91b11/e+9Zd16/UT9Bv2tvV+1Ji2cJK5KGyDCBSxxPPG89WnFNxW+Nm3dFX0ihlV1VPlVdV79ktWE3Tolfv0mBCa4if3J/eq8aJDdfl7cmf3muHG+bkWChRp5PD0xStFsmyyxbNssKmx7LGpsXXVqpvbxeVoxNYkNab08lg8dfH0KPQJ9Sj3I/YTpJ/0Z3aLdNT1vPQ/d0r1X/Yl9N/2N/Y2NCC3E/a29ynVbNjQKs1

CSCUxNejJloM5KEqHwBQqS+fFO/Q4t5X0ODeWexAAGDGwAZOxOJcc9Ww2TXpj1ewzKkJSRRF1RHV3VKi17bdptu0U++ft1mmAKfdf9rT0cA18tXAP3/WwdP3Vm/RzAzFbt3KFVJeIpWGyY7tJIIhLN/R0W3WV9iL1//VfFSN2QzTd9PC31fWr12vWGzZjdE31BAxEDhL0RPRat0p2G9ab1xvVRA19995HcQFBtHACzGA3JJz2RzG3RLvW0xY4eDM

XXecptx73w/TzdVANI/Y89J13HWKYDbAPmA+09nAP4ndYDTR2x9XYD2/4Y2R9E6sZfeEP4tfxbtr/9pr2dFRX1hsX59SiFOsWDA1X17t1+vuE9js2xAx6905LLHkMDKAP7hfH930qqAOQMbXTOgPVVaT0SRrvquF1/vlsEChKcMry9nvUlA6e9OV24JTptYr1hJdUDOv3sA3UDlgMNAyHtrb3RnSp1wKgIiooRLjhSNVNaniBiAT01I41k4cntCL

1Z9TO92sVPngg9j/X1Xhf1YIMbfTf1Up0zA3yeX0CX9ckDYb1ccNZQLMLh3dFl4g3/9fyhQA0fMjoDGb2I/YlFNAP8rcYDnwBXA2/dNwNKvdwDbB3vDS0DMnDVmAIDjdkh8MrAkL3mNE9wIfmjPVB94z1Q9dP91t0qFaJcw5qU1azM0N78g77lLA0c7TB1EwNwdTEDnG2wg9wNvZoCg/PkjZ3KAKHN3egcALUAK5lanctdWwO1CRINyBXSDQyN0v

399ScD8v2vFRUDV70wTKSDCr2E/U39/11a3TXdHY31zb1REGljdLl9zJidA/+5a7K58Qo1kH3yXdz93IOwPRqpZBHODSENmtR+PWeBwQ2TzU69XO2Sg1MD0oNdbZtVwtUeDaENjZ3mjJFkPLRQ5miDKE0x/rsN6LI1kFmJrFqFPcUDtM0I/WUDBIOmg+ndTHYWgwT9t/3Wg2Y9hv1rNsb9F53PA8uk4pVbQB0DCEW89QYaN5K4LXC9+NUwfTIDQI

PE1WxJCI1+3bMNDky4jUvdkvFdXuODQ91U2dxM04Pj3ah90YONrdYdgQ0zMsiN3d1jg0SNDq3pzq8K9WhKsYBRrL1ZA7fwX8n5ZoHCK6QdyYyNN3mbbY7VB/3FLYYDaonSKpWDDf23AyKt3y2Ug00d9VVIKX+c95KdRd62rtpZxQhorbDPnVA9dg1+g7z9Pc2ZaTSA1o00gNGNoY1j0KuJqMVQxfPd3IxFnZaNyo1BjcQw4M0mtO6N8xWDbf7dyx

W4Q9EDMYO87Zap7DCBjZBDsEMTNLGNuEMu3V6NhEPJA7qAPZSxcEd9GYNxlrqdUuXoTQhwRBz2Arkth91FPYaDdz1Bddt1SB2XveWDF1ZPg+SD+v0PA2wdol1P/a9UXJb4QO2DnR1avSQ2DdxuYX0du/VeA1yD/YNwfbyDs03LgMpNaN2SHafNto0rfZYldLXu0FaaM80PTSjmNk0mQ5v2DO33zat9L5VatOXJFD0Sg57dRL0wg3GDKAHwzZ6R60

L2Q4vQZkP/JTl6DxoRIE/NmLzf5Z1+0PLykNWgj0TXaDQ6yYCgypxuiE4nas59ib3N1dRG2WYPhaQDSm1w/UWDpQMdsmP5E/kHXVu6IkMVPXQDTHbQ1XkVsNUWlZCVwjmG1dVVicXM9a3hmX2fDWvOPrRN3RgRHZA/RK2G1sBrpozR73HoAF/MP8xAsFZ9k/2E1TP94AX+kiNDc8ykxUtd7uLjIDHMkSHnbHhtriUshIg1KV2LERBp0q44ONmJcd

yYNSe9gkOn3W4uub1PPVkVw5WlVaCVdUOVVYjVtpWKdT09lr6pqGawsCIEbuxRzZa/nBpDco0i6ZNDPINK8jD8yPaALKKDFCx9PlfMAMMZnBesIMM6Bd79TaCbUYex9XVqmrFDdoXGxscAiUPJQ2ksqUPyCIU24MNAw/QNUMPbgzFKRALYAOWIYDYYViXxkEASmOiQj4CCifJRmcT3hb35MP1Kzv2hjxXBrSfdpUMOFufdqTWVAyRVl0Mw1ddDVF

X1Qy+JthBCxeUV9FWKdeFlbUOmLZgFHJhKQwkYlyYPcR4gGaLOpb8DzrE7pEP96AC+pPQA2BrggG5G40Nc/T9DfVUVfa/Ni0baw7rDnQ6ZxAxFaVgaVlxI7pVRptSOM624g9eD+/2lPcF9Z0Pcw8VVvMM1Q/zD8NU0VVVVCcUbxXDtIL0mpRjZasZgFWiykUhZxfZK9QEeA5pDzv2Gw30DktLY5eYVzfZ9NF11wAOUPVLVUoONykTDJMNv/JsIhI

qKA/ZYWb4QyL+9TIVSAjTMXXV9dafhgb3twMwAE8FfdfgA5owR2JW9jIDiseP5KQAT8hsDGUMXxHTDkUUD+Z3VeIMlg+e9hIOD1e7DnwDVQ2aV5VWTlQjVcSX3Q8z1apG0JZr2AAhm5WQcDOSKaEziWO0dHYa9jArqw02dxoAAUS0AZm16w5Z1ccN5JZmdTQ4TCvvD8NI78PHBC0MY2ZFFMPHb6RL9q7VpKQaDVhYBdWzDPx33DEf95f3nQ1DVns

OTw3DV08O+w3dDYsPM9aq9ksNjSp3apqFMfnzCP0RUHE44EH2O/WM9WkPdLWfD4HW4dVT5nv1/EqE9Hl11xfDDCLFv8XXDrShlOE3Dsn2tw/3A7cOdwzh1Mf14dVxJ1cmoBjAA2AAxeI0w06m+gN/yDEoMvWwAFBW3Hd3Dzzi9w0gFTK3SRgPDTsM7tU7VANVuw2aDp4gTwxRVQCPUVfll3BVG1c1DwjWDrTvFWQpwFdOR3UNWZo0NEWBvEPEt28

Nvcca4yPD5chIKmwjHw0RFVnXldegjMPW/5W0wLXRKgwEdd8P0/etDYhDSwp3xPnXmykcDwzYfw88VJ0N+9T/DgzmVQxdWMiO1QwLDt0Ozw2AjwjUB7oblHghp8I/5v4OtwaMgXwL0fv29KCOnw20VlXXIA5HRBkU+4FUlOCPOvbhh+CPwdQjDwb4uaMwjdyAzwDSA7CPinOOAXCM8I0gDhkUEwzmmSSxMIn6AiQDW+ph+NcENBDAAzLYUAE0Awb

VsfRgJUpBerZTF4knCI8rCugPF/frtBgMD1V+FYkPdlblVvZW6lX8VRVXjwwAjsiM3QzPDosPG1S0100WQI+jAdxXo6LAjb/0vDr644gEDQwP9qCPGvRkjNiMxSokAFADQsOWAa4DjAECkpKlyCMb4PIDEAE1ubTX4PqG1R5oJlm3VUUXFtgd2QLZ5zTv9XK0sjXzdFF3CfUEj7D5qhJKRNIC1AOoWveh+gOWASIJSiASO44BeflGAPqSGhHYQxm

zsBbI64GbjgHAAkp61AEGir60vZJgAzgCj1H8wo0ZQxu0AvGYAhjKIBizfoMwA7SMndEb5KQB+gAUeRVHDXmIKC/J12m2Npvjf6aF50qWCLpb8khXuKGq4a0MGI4P9Q0OAwCvpqKOjgE025iOzNcRFhpbcFi5m4EMTtfiKSqML0qqjmQNdNW4IR8B5PU2WcAUPw3EAQgGquKkAyIy58YHGWNIYNYPDEA1Qo3xa94PJPvZ+s8hFiDuNiQCIo8ij/c

Coo+ijwRQjRNijuKOdRA0ABKPAWDwAxKOkozEKFKPgDlSjNKP6AHSja0RcRoyjma7lpMHAA6Rsoxyj705G/DyjPgSLuZsIFACCo9fVUFZ5KuashaDDjQkYY3TxfpFgguQw3UZ9YBkd3kaWvgOS0jr6BnJmVIVWRep/ekYpRjW6BRWd985ww8UjhCNTPvcjjyPPI68j6dGtAB8jXyPYfoU2HaOVQF2jTSPfSqYoEYDOAM9O9yPjAOAiT3qQpEuZpA

BFoxD9AKN62AytPTY4OAfefzjxNaIjcv2BJSaD5wMifcSDlmTwo76jSKMZugGjaKOjgBijIaO0NWGj+KN1nFGjMaNko/GjFGCAekmjKaMMo0yjmaOso+yjTjV5o9yjvKNFowKjjWhlo49DkCLKEumi0fAM0IpD4XGNHujVg0PGuHPSO0CiLSXkaqPDtZ0pWqMgNTFKhGNHdNrE80NCPf8jIv1fgEIBNjBywnWW0U2NAdejLMVnvQWFkiPzIx2+z6

N+o2+jgaOfo8GjWKM/oxzC4aORo0SjvgSxo+Sjzm2Jo7Sj0Pypo3iCkGMso9mjMGOco/mjCGP8oyWjyGMfta9knMrI2u+AXPXAqLqRkfl+srag8o6XIzbGFGOu/YN9Kv7ghYkah6S9Po720Jr3GhtlvZiUWcJZWFldmsx6TrWjmpyaiSL+9iLUCElr+XAAyPZeQPNyqElhYxFj3fQhQPQAIWPRY6hAsWMZFPfYWoA7hAcJQN2HGli1jmOd7s5j6R

GuY+hq7mPitcma3mMCWW32rJn4WZMaAWPimqGE8WOJYwNJMWMwgL9moWPJY81j/tTBY3/UrWPhY81jqWMmhrAAGWOy9avp6cMeQ1Q9XkOIsWujG6M/sUNcO6PdMEn9jIAHo8Nep305Y43kTmPG+AVj2JRFYzuEHmPXCF5jA5n2taJZdFn+Y00UgiKdY3sJTWORY+djbWORY6TUZ2PdYyljp+RpYwNjghSZY8kD/7wuAMcAbACDwHx+YgCEAMaAOv

gUAOCAjjG6MQQDfyP6ysejs0BO9SQ+oDFPPmCj/EMFLYF9QkOVjeVDor0Po0PJx1jeowijr6Moox+jX6NiY3m1v6MRo/+j0mMko0Bj8mOgY4pj9KNpo6pjWaPdojmjsGNcowWjfKPFo6Wj+mO/vV+D8qmu2kku3zEvRU20f0SpIxyDl9i7w//tsTKnstZCpGOWPu6s7ZY8FnZ9blwi4ydEf4Di45kDgPVgVWZjJ8LPw48mmu3x3btd3yA+I3oDCB

3J7gEjXMNSI16jAmPY4++jQaOYo6GjEmN/o4Sj0aMyY2TjJB0KY8mjSmMQYxmjamN04xpjcGNM44hjumNCo7lNDQDB+ZHqs6KP+S3NF57CtudI7dk9g/gtrGZ2xnZjQqbu/VLxi4OeQ2ADJSOp0e9js8pfY46kbiHPAP9jpACA48DjNCNtJSUG1cO0vZZoZABsAPfsJ3Sfoxb4IIDgCpR144DNNp/xR6MpvWMjr1DnozBRcOOFg/ctxYMuo4J90K

NcsX/DmOMvo/6jwmN449bjeKNE43bjgGNxo+Tj1KOU48pj6aPMo7Tjwcr045pj8GOFozpjrOOKdQ0Abf1yQxmoTjCnNkAR3eF9iIpoevYm5ZIDKCOYRc3gzXFG6RUwY84T/frDbZZx40bDbly5cv/t4FiYAJmN9GPg44xjiX5r/d51rEW+dRytEDGziHrjUyP6A6utRuOENUPjZuOj47jjomMT45JjxOP246Tjs+NO4xTjLuNU4ypj7uMr4+wua+

Pe49pjLON6YzvjvAMaWo0eBSFV7hahzmEOPA20Uj22Ka2jCcNuvlkjA33h0XkjbdIFI5lxUxXwsZM+apoV41XjCAA149VJ9eNUdU3jlpCuRY0jH+3nWdjEAHpAeiB6Jm4yepB60Hpy7YI9cKSEA/QaEOPFsND9Vskd4ySuwGR73ZxjASW7tQRN96Owo3ANhONSY6gTsmPAY87j4GPU47gT0GO5o4zjRBNIY/7jMW0NAE8DfAN6pNg4dZCP+crDMc

kPINxIHbVNo2V1XBbS49qjtyXTQy0OFzhwAINQW3z++mIN1MX2Hu2evZ0e9SATpxF7XeWN7MPJIeopJuNFsbYTruP2E8vjjhMM41pjm+PEE24TYe13rYIGAqjo6Mfj5ikaRS16VMYNZQwTtmNtowNVAwN59WMDwwOdE5OSUIN1EUStPkOElaMDxfUro9DyyPDpA5oA4wCswgkTCQ3agzvpYL19+JzdXiM944VDgr1PLWVDR10VPdAt+ROYE3YTOB

PFE+pjThNlE8zjrhPX1ezjJqWJsKD45qHo2ZojXy7HIDp+unVyo1cjW6Ydlu0TvSlb9KXkvT6H7C5j7in5Y4BhPxMFY0RDy4ORPVwNjin/E2ZUgJOAYf69peM8uZZoxoD4AACk7KFx2LMT0WWTXnAVSDi8qJHw+SGyDSsTJp1rE9xjnvkjw9feutG7E/PjWBOL4zTjJRPr4z7jW+MkE8z1DLbVDakEtqB2sTKp6BV6kfuZUhWwvSET0K1eUS/jTB

MfE0Wyx6kpskVj+qnCkw8IopP9E8iRza0yg+bI4pNuY42d0dkICVEAjiM/4416Ww1t0a/VQ8U+EgpuvEPa46ATmRPcrdMjOb33ozsTgdggY+ST+xNL41BjRxOlExvjpxN+4+cT/X1IKQ9gu/4C0nh8Vi0bUBBwlZQC4z6DLaNtEwKT4uQbTrfF75VhA398IZPitcCTGH1NrdNZcQNPlRGTW80/rI2dSKMcoCXAQOOok1cm2wPs8NKQwhB1kJjSRB

xvwx8dIa3ZE3pRF7l/wxaTYGOFEwcTNpOe48cT9pO+49vjDJOoY1Ok9+H2rLlttxNdHSKo1M5GmNZjScnhE4jd3UGzHLQUUvSWoqFOxuy9Pq80MtT0bS3sCZwh4Vj0BZzIDBOTraVTk1KTM+WDE+s9EUGDk2T4s5PoYbb+C5Pjk+kRk5P51I2ded2nACksygBa1BmTcZYxZSwMJ+JTIKtDDKkivocKGRPKLRATBuMG7ZzDokOqPRdWFZML427jhx

O1k3aTtJMVE+cTECNfg0tozFYmY52IHFUCUoJkkZ69k5vJAZMDgxBDzuYMWYyqnbpMDpVq3Hz64vwxEJPRDCfkchRD5uU50XxwlEuTaIhbHAljXWNJYz1jkWMRctRTsWPbTvRTzWO7cvGcBwlxCeCDk+YQ5kY50JqYU/Cq2FMLMkY5pFOHk8ohpXREU7MiJFNfE8JT5FMz5ndjTFO0U+Vy92PtY5JyclMluoty+QlsU4r1q5N7zSstpEPH9aomXF

NbmDxTy/acUwCq/FOHMoJTklOAYQRTolNj5jfmElN4U4hUTA75aTkclFNXYzRTuwmKUzdjUWONY9djqlNPcjJyGlNkQI2djADNBLd0doBdw4eDEqVxlqhN3FCNcGGmyhKkkkRcUGl9oZMj+11fw4ddn5PbE+WTBRPYE9aTHuOr417jzhPlE2cTH7XJgBcT++MLpBkKbwVtzJKubiBV4gIsnQKtE/2T8eOSQpVqj9SmFe96dWOfeqep33rIAFeUIM

Ov1HF6W5hCU1ZTraW/FCNTZlQ31PzhzsxS9EURSCbwqpOYWD1o+rQ8mWqIBowNbVO2hB1TePpdUwT6w6nFLJKZgKo0zHvc0AzjU5ZTk1NjU2tjS5NB4aGEO5P64ULhj6EmUwa1uoRLU476K1M0qmtT4wP2wb4NmI1WHaCTPt0isjFqm8ZBg51Ts267Uz1T0YoHUwNTJ1OXU8JTQ1O4U+tjo1PXUzNTguFzU5XmT1M3Uy26HOVOJlw9Pk1oA/6SmA

A8gAgAXFjjgHecV5N9+XKeth5EXPfAuDgBwpG4GUn6ky+TrMO+IyWTYCkRqXkT5pM5U5STDhO2kzSTLhOOk6VTwfmR3GcAeWJc8qgYffiY9XNMzxM2Y81T7xNBk94MZXIHKDGAEIBRgNnAkobFvLeBqFOL0MQAwgCmhKdBFcUK0+8oStML+KrTVIjq04DBiu5l5nDmqADa01oAwQB608njY2NZw8TdvJ5jhAbTeyhG0yrTPZSm0+pAGtOG5lbTNt

O602jByQOnACuFo1zeRT8jjN1RUxItk16RxJzw0BVops9wu1JtePlDqxNGg7ejKpXuo6Ut83G/kxST/5M1kwVTdZPAUyVTinU+RNUNL8C9iKyTIK2F9Lz1pyFCGBYtl+OC49LT/JPIU9M9kkL1AJ7AG1OorcfNJ+S1qcFsEdSMbUDNuK0O4aGEndNA01SsqNPb5rgmA5mz9KOZAjq6hB7TJtMsVEkRKRJ4MO3TzSCj0zs03dPIAL3TXHknNJ3tg9

PAYSPTgNP3fQ9T+lMu5lPTDHr5eq+hs9PctfPTYQDG017TS9PYYZ9TPg3vTeNjU93mVWvTW4Ab04i0W9M704J6A9OtEb2Yo9Mn01/Zk9PrxtPTaAw30+jTm5gL04/TgQAjEd6SkkGdJZZo8YAwAN/tjoDosWTTbsWJXTdaB916KKc1R0OI434jJpOZ07pt2dOc03nT+VP4E4VTJxMNk/STwjW0WIMBzx2nxdW0HwWWDYIDraEIU+RjMtOBk2302C

ifpT/TWqmSXOr6e1MzLIAzhWrZ9q/UIDPTfX0RaNNfxuSUuRlQM2JZY01308rTi9OBAItTcQnL0+aSzaiCM4DTm9MiMyfU/9O9UxIzqK1SM/Rhx9OyM6fTltPn0xAzl9Mz0yozc9OhhHAzatNaM1T6FRH5I1GDKePEQ87T5k0CM6flQjPWqUYzSTQmMxDTmTQX7QfTARH+9tIzVjPj03Iz4DOKM5Az19NOM7fTLjP3057TbjPPU9ozz9NHVcgzA3

V7OAEEH+jY3n+pzNEaE471RsqwbMc2NbTRIQGtGdmGE1kNSOOIHVsTqONmk8dYOdNWk1STPNOEE8VT/NMl054T6dLYk+84KkPzpPKouL7JkqN0X0MvncLJvDMt02RtOsgGM7/ToE2dZezublMMU8JAITP+HLJTPlPuU31j6WPrzTfNVuRf0+O9izNKFOFuhzNATvoz8GqorSHBrBOtU6czwTOXgUbBysGjbJ5TnRSbM9SBrlNvM3szz2PXzRczTB

THM0Ezx1kHM/8zzmxXMwVqNzMoQQ7TmcO+M8S9QxNCk9czQ9N+FE7l26WvMypTIkAfM87lXzMqUz8zMADnM6CzOBSAsw8zwLN/MwSzVuTApUCzVIi3M1XDBHXWFdfh8QDlgHyjEdPHPeBVbuL09gJkVV02QGyWYpA7uKdgTIJU1iZ5cFUX4oeePEPp2ebJ8j0PLUPDXvHEk4+Jon1enZQzRRP50zQzhdN8042TjDONg3wDIcMp4QF4sjlJ6ZYpmQ

7CJL6TyLVxFkhTukNwPWrQ5Tl6fIYzZlyhM9eqkNxWs4i0x1N5YwjT51Nw0/bkE1P4UzSq4CG05ZxBboHOs1dTwHQ6M4PSlrNBfNaz/Hp5avazobOOs0I8MNOjU26zceQes45TXrOazAjwPrPFnncBsbPnU2tcnjMcE94zjtOws95DG5PQMiGzGXxhs91TRPrIAJGzpbPRs0g8mbOes/Gz4eSJsyhD3rOPZXBBfIF1s0mz2bO5MwsDcf3RQyPK+g

Cg8RfVHWQYXWqTs8qw7AeQHrZA4ekl18SzULyhDcjPcEfY9qzhIRXwhyATStoDcYEp0/iTadPGExIjppMFXaoBBBNFUw6TarMtNa7RtUlllXFMJyMruGF5JDbtJNYgSkNS032TzdNmswGDoAyl5CBqf9N2szvcIGpOswYxDlPcjA2zPwhNs7g5S8ips3Iz5Tlps8ZeXiKnU/+zU1PlEd2z2SPQMnCUH7OYs73TkNw/szGz/rOw06b6T+5nU56zfS

Go0xBzrbOugRmzWHOI04GzCHMAOfjdhSM/xZPd7r3wsx+Oh6QoczazgdRoc9+zhWq/s7hzsHMXU2Rz51MEc+BznxrEc76zpHN/sy6znrNdsxHhyQPMKtMAlLHOgNuj7Ip9sNHdOeEiVvnhHFDOKAHw0uCW/fXd3YLjIHdU7Gw+fYe9OYjPk/JJhpOQo/3jGVOp3RfdKT53EULDrui0M/WTdJOVE0C96n0Og/hcxJ5ZDnUNfCSek+TFroxGiU1Tz7

OkbX4R0wLhiTsIZbNg0xWzu9P5hluAIXPd6JkRkRpxM4zVjkyffBiAzNTWM3G5syK6PO7QUKWBFDFzYXOiM+DT+1NRc95FiiSxc0oUog7J5Alzd82Q/ClzvaxqFYRzGXPovFlz0LOgAwWzMF3mVXsCuXNLMyxzPdNiMxHUHXMlc4fTHm6Vc2w9SXMagKlz8TM2M2O5hIiZc2GSNLMMI29i0tgBBDAAfMAHg6OzMq3WAgZaEq56pPWh8HAKiJJmBE

BypYUKYOEzULwBSmQfRHmDbx3w40WTn8Ml/UJ91xFtM99Wh7N0M45z19UEMTSDofCtPq8QJrlo7Y0T5fCrIGrGAKmPs4hTszMvs8CDJdC1nPZT1JaFak1qg6gD3ruokPMZaiyqsPNaU/eVsZOyk+Dz8PMWU0xz0PNI86wojZ3ggPNE7QACuGmACnNxkkyC7ngCws8QoGgc8EDYdQHu0ggStyaNcEe8RHUwSJEdUd7+ffy9x0Ms01x1rtXs09X0tP

JPcw5zIFMftRl9SCkUTGJkVArzpDezhHyKQ0DhD7PR46qteCkBc1NDr7Nz+rBqn7MFczMsTbqDUzhz1whNs4tTs1OWM4izOzSTmCWzYOYm87qEyHOFauiak+Sdc2czRvNpetf0g6hBs11eb3rhsxFzgnra89DToYT6889ThvNH08bziLSm87MiDrP28z7z77PW8/30tvMDc0HzDvO8+gLhzvOUcyNjX1Nv007TcLNFs3Yydfru83mGkXNe8wf0uv

NwDL0+BvMo03HzGRFh85uYZvMRIBbz4fPY86itNvPRczHz5fPAM7GaHGGJ85JzYxNwTvZzRdN9M5b58V2ExA0GgiPvCYqc2aKbs+lRWV1cY6cDCv1lg9+TNnMe1bUoXtWk/cH5ViAYsCMohYydk+alM2LdgzyTcN1TUaazgXPvkKn58dXbke1dydWdXanV3V3p1XcGhflTlsX5iMn3kWX53yD51beRVfnjQGEzxSxTXdlhxijRkOOAygCVgBDI5g

CVvUsNyYCYAHGqcV31wAVGrfWuJexDVsmYBe2Ridngo1dAxy1iI7eDwXV7s5UDhV3JUG2NN/kVUwlR4gHv1Wzi7ZP9NQltPpNnxWxm39EarS1dkslgyUNdEMmyyVDJ8skwyVfzmdX9XSrJOdUP8x/gT/OWdWjJRdUYAL8qH/Nr8EvItUC5cjyApTPJSi3xIyOI6W3jpqg0xQ4eCpBOHlVdcj0640zT+uOWnR+TlnMqPZfdxtpCzlF4sLTRDV7ymJ

BhTP3A5TZWAK0AK+qQAMaABwBNAMZtuQIUFQy2/YAUAIa2dQBv0k9eqhjjgKQA8YCHxKOAxAA+gKFEEpGQrgPAi9FVRXWkCfgUAOnRcACTAGowevT0ALhRaa4foJ5Gc9xZ+A0t0ikolaMz8t694ctQP2BxftwzVfGW2PYCERMYtWDzNqIjE4NJjA3WxaMTL9N6BT4zIJPTAwxzhfWjHo2dhkbuQO0ADAG28UtdXFUVeOINh4B1UuiKsyCcDAIKeS

1Xc8fdzNPpU5sTmVOtM3/D2gtugLoL6OptCpIAhgvGC2PYZgtWaJYL1guJALYLirJhsI4L0/KYAC4LXwBuCx4Lr05eCz4L1lA0Bv4LolEXBcELygChC0qxEQtVUessMQuQQOqDI64JC0DdDpUY6NlQXVXo2cA9zrwNiSpiziNA82JxuQsBxqMdjeboMAKeHFP8nvlZUZPmrbGDGfM10oyeUIvJA60ASMgtAM3Yg60KcRILQqFAo46QaNJg9tEYOS

3JZVjpzqPrE66jowvqC1+TmgujhpML0wv6C3MLeEULC6YLTKAWC1YLHAA2C2VTGwsOCxd02wu7C4bk7gueC94L7QC+C6cLYRDnC3v5lwvXC+ELkQv3C/akjwvxCykA5YDOk60dnTX7mdWj9Q21U2s5/YidkNmp5PN5pPvzJC18gwmegMUmXnczsoNEKIJB0IscbSRD/jOo3oWe5ovJA3Wc9ADjEIqEBylkxe0LDHhUqS2e9PMtYVv9RQMSs73jJI

vmc2SLZf1Wc56jj+jUi741MwsGC/SLtoCLC0yLKwusi2sL7Iv2C1sLzgtMoLyLBwtg6gKLQot6ACKLgQuxxeKLYQu3C1ELDwtxC1tGCQv/3Q6V8pBrsjltma2GKpcSnQjEDd6DxrM7OUCLuosq84UL7eYh5C+eoYN6U/x8n56RgzDD7A2bfR/TOx1D0t2LjZ2SAA5oDQB8ae+xWc6YiwKQvH1/9WheeK52lIcNs60IC6+TaVO3cwPjoIkTC5K8Uw

sRi7SL8wsxi4yLlODMi6sL6wvJi1yLqYuU4OmL/IvHC34LuYsXC/1EVwuFi1KL0Qsyi6WLCKYJC1Y9lYu6siKo0vPZlNT9Xy6MNDah7IN+k4FZrYv5C+O18zPg3rAyq4GqTPMyLYFGi8penO2Di+MNv1M1C3CLUtLIS4xZxotzc8HNblx7fVJAVuI3HWqybQvzi8Mj2oPsMr/wrl4yiYWTQwsqC0K9pf1jnRSL1nMwTOGLeguzC8eLJgtLC+eLCY

uXi5sL14s7C2mL+wv3i4KLJws5iwELz4shC2+Ldwsfi7ELTwullgkLzZOM4g6wo6KV06kL6osOIK/V3jggGQrz/wPIxJx4eQsgi52LOnJd5j2LoIuPZg4yFouYS7CLulNWS+ZLNkv2ixTcpvg1fhs1GItvnHzCjbLKfjNemTLsGoUDHPPFPTeDLsPI4y0zMA17izoLh4vcS9GLvEtxiyyLbIt2C0JLTgsiS7eLYkuHC1mLkktnC3mLpRUFizcL74

sli0pL8vYJC0HDOAunIOiyBai1iyX6P17SEczOBkuuPb9JdPBti39DqvPx5EcyjNlCg+ZTBuIo8+wtaPO1C2jeHUvJAyax0n2MgNMKqhNiCw7xlEuCELgzrZGMxIHCOUo1ZY+TjsMUA5KzfePUA9PzlIs9ppxLkYt0i0YLJ4t8S/GLiUsciymLqUvRjOlLmYsPi8KL0ktiiy+LEotFi9KLiktyiwvSQNZmmMThmktAS8yDgCiJ0xXivgkAizkLOo

vQS5+dQXMGqaYheJn6qTShhqnE7oMVzXOuvas9OlPWi0WyEMugy7WZ0MtSE55VF1kl6LFwc0A/kR3UjEB+RP61dH2CuXStgyPqE5gJ8YEcUJNe+96d41N05AMbi8oLb5OqCzMj4a0kk/jxgdjbS0eLsUuxi2eLh0uJi0lLnIspSzyL50tHCxJLj4vXS7kFeUuSi/JLhUtPS+VT73MoEEEWaHADKKA9CCJNBQ30TbUN0xBLc7FQS5Rjc71rQJ3F0E

CSANdKcYBSQN5FBwCZRZxGq3NqE2DjuyDky260tQmkPqCjQNGBMc4OxIuflmVE35a91RsTHMPki1lTY8PlAO0AtQBEINgAQGBdMKh2UkCVvdUActgugMwAiNnmCzzLgkv8y9yLokt8ixlLl0tSS6KL4su3S3JLxYufi0VLVw4JC6LzJqXdsKw0El0vSfKtMcTqEc2wEq74Y2vwwCGcUpPBEuNzNfp23iBypcfjsuPpzmYoICHMs20L6hEllaajNJ

0nnnaM1iQ3rlMx463iNoagyKT/vo1GhnNSMBE+vMSHQ8cDXPMjCz8mZDMptXKzmmABy0HLIcugXXc4EctRy2KxscvLCwlLvMvHS8JLgsspyxdLIstXSxnL0kUSy/dLCkuyi2WL8ovB+bXIXjgQ3SHwjNDaS8jsfJBsVdkLXdnayy1TlcQy4kArOLnevqtR6EuPtgexI6O8E8G+8QB6y+WABstGy4Cqpsvmyw0AA35nUbPEHfPxWqekmtjjAAHWpM

itAEjWLLb3YODITYBqkaDjC3Vky+GOggTA0bYCDMRfvs+YCi0NQGpRQTENM8WTy8s5E48pM/MwTJvL0sDby2HLe8urCgfL8UsXi0mLyUtJy2lLF8vCy9mL2UsyS6+L+UtSy7nLT0um/a5zs742IF+AYN2Q9lz1QPhWvjg4hLbPE9fjpIpsAO1k4J7m0Y/jJ8M4iQArr+OOxsaAxitV1WsgWc6fyCd5whi8vtLRNNay0XTTYrNzy+BkC8veI7L9E/

PGg3u1vGPcK6eIvCvBy9GiO8vhyw0AkctCKzHLIisCS2Irics3i2dLUiuZS6LLN8tBC1nLCis5y49LT8vlgHvjcssQaW6T33PdQ929JfouisGmFiSyFZrLPYlWK3wzDtZ38dHRVHPVxCtR8dEgA7DL0CuWNRADhzi9MCCAeCtiYW5oRCtRgCQrDFj50dsChdEl47SzF1nv0vTiodgavE2AhABNABPV9AA5cLUAmsSlHrSxtstXgNsNu0nRtS4lfo

sEk5Pzd6Ory5U9gJ1hK/wru8vRK/vLcSvcy8fLCcsnS+fLGYvSK1lLT4s3S7JL2SsPS4/L34vyi2QTHTLN2hJo/hPNwe9LCCLsCFjt8HxTMyBD/8sAyzrL30phTNBIvu4UKG0uvx5VHkPOhAB1iBadFCtJvbtz4Y5ocN9hDfTLemMgicrFtgwrT5hMKzlkqQCJ6oB+dNDcJMSLhJNnAycrbTM8IMc+pACuECbsrhDOAFJAyYA1VK0A5YC03MA4nE

qbWkfLoit8yw8ryctPK2kr18s5S1CVd8sFS0oreSu2A6orHQjR4KKuyBjec0ZaSVY3E9UrjP2GdSBswtF+gKdEz06NyxqjIvF1K3MzXkV6qwarikGui04rk+iJLvJWGnMKqAG2Gn4oYMVQOkHbXSqo+n6n/oIEsrgCrOAxjNOvWv4rRhPiI27p0BMVQ3/DzKusq/PyHKtcqwOzvKvvrUIAAqvxK0dLV4sCy2Kr4ksyK68rmcvvK5LLOStfK+RmCQ

vNA4qrc7JDMvxIM67OYU2QxVBW/RrLzYuQS9CrLVNgsVB1/RTf8DtkDqtFfo6uJjX5s42t4AOnJHCr2EAIq9EedhBOHBj2sQLoqzQjyQNamIyAznU0gM6AQBWui1NLvAAkA+OtIOI4OAfYRp2AHMzDm7Wbi1kTHCulk6T1f8N3i6nLV8vpy1KrDUMyq4oruSvfK4b8nMpQSP5IjUklK8npdZDltGlg2otNS4DLTV0ihkN99Pou2bVBPtkkIZbZoh

TDDQBr1P7LqcMNkFRjwviakMvmITLZfv6E/jLin6vYWcBrY9Tfq3+riGtPGr+rs2yW2WBro4NPOj7ZIMvO3lEZZTrQa8oOjfb+/mhL7StFI2nzhbO8nvBrdtlIa8MND6H/qyLZGGuoa/0R6GuDEd+reGvD/gRrH8E+2ROaxGtf9qRrPbPP9X2zWvTPsWfwlR7YgI4rV+A5fXnYKHKKmAKQHII9Et8J6HD2sEW2X5wu4ra8MUwVtudIDEtF/VuLxp

MsSwLdGgvsS/ncQssSq8ercit3S7KrF6v5q/KLxqU4C+Jm+EAoEp54n0vpClH5Fcayo/VLRr3H4K+rpkuzzZCLgUOW5If0t444PSlhDD3iPMFr7kMp8w7N1Qv2SwjLvc1g5kQogWse4d+gIWtYK91QUYBNgMzMfoA9fn+VFEt4QN/whrOSqJium+mzuvT2kFqzUAJSOw37AIpk1j5YpPtD6RMmc1urRpOQE4Zrwn2Mq+8MZmtpy7IrbyvyKzmrny

tfi7ZrP2JobTe60mgua/RmFmNg4sBDUgNhCXWrstP8M4vhXYvhaxfa4rQytJCR9BF4PWMcHuGnrVFrr9Mxa9GTK4NTDSCRG2vLawc6q2sRQ4sDImvdUMCk9lirdpoAECOeS/4IZ+Kya8sRCmGX4kpr/Ygqa1Vr0zGeKn8E35z3IEtLAwvd41uzS8vbixZzwYvGa6GL/5Jda0erPWtZq31r98vSy3kr9pUOa5Tq/UKec1Lzrmvh4MSeDvyS015ruO

3GS8CLLVN0PVHmm2sXaYf0a2uha2DmSXyBQ3wwFOu7a5ULnasHa39T093+YaTrp2srGfTrjZ0kxc6Ac9wAYHFJc6upojJrUFWva284LiwfaxVrqmttofUJ6Bhu4l94okhXoytL/ot0q0ElG0sma0hcMOsZq2LLt8tZK/1rD8uDawZmCQvGLe9zB/59+ACpK2gPkg9xsDhHZJHJf0tQq75rsPmDwfD5bap2gN+g1YBxBKOA7U1z2rWUdKEmesRGCP

CJoMOg5gAWzDfNQ1Ogs+ohZLPXCMhhket+4ZHrvOGR63ti+LPugrQhCeuWVMtcr6DVGWtNzusjqq7r7uurAKOAvvK+6+is/uu4AIHr3vYh6xPNYev/MxHrcesvoQnrsetks/HrMetPwQnrKeuR62nrUNwZ6z1LhK0yk11tqBmYocXrRjnFxGXrGfYV61NNVesXMzXrjet1683rmuGks5qCTetks4/cILM4FG3rZLMd690hjZ3l1fDIrVbkK2TFOq

Ji6+iT3RKhIeAabMTsrSK+o/Pc3duzwasPPWgLfPM8IKWkvbVkgPtEyPbmfT2EfEZ7Gmke8aOuEOzsaFpkQPGAWgAUAAh21Uk3ACRLbuhMoMuAiaDYAAsr0ECo1sxwP633nMwALgBGAPfR8OtWa+ereauG6/KL/y0mpQ1JsSMJ6V39DZZ0vEuhL6smS47rzrlVaWEszYJUFunRAJ5e67whKQbdIfE0IwzCITgUFlwbKVIQiLpMQhNlWuQQNKxhMO

7ytK5sTEIXaWvrWuQNVKtNZHklqkc51Bv4ALQbPjlMG0F0LBvqOkxC7BvsKSNsnBteoe+st4Q8G6y5yzrR61bkghuUIQIbRWmlETgU4hswyxRrrXMji9KdqBkKGzaprBt+5KobkynqG16hmhvoQjobo7nn3PobvBuObIvrPhuyHqIb9zlKFBdrvbNMoZZo36CNw9qNb1bf42UzFMD0fnirW10QGdhAASFlsF2wAca0VhH5JMFKYZzSscRiLDPLRI

tsKzdzBmt3c7uLfsuQAI/rvkRTwG2cpQJxgP12qXCKJJIA3+u/68Dt51WAG8AbIICgG+WA4BuU4JAbYRAwG3AbHdRNgIgbyBuoGzrr2auI63Krl6stHQ5rGL6eCPl9M5FfyxnS9dldtNWrzaO1qw7r82uk3dsobtOHKLqphdjj+UlBWoSq7AAApPIcGzOIZZVBRxuaQqcbVRznGykAHoSOTmUl8tOXcsSMexvJgAcbI0FXG5OONxsYsxcb/xxfG2

ghPxt3Gw8btktbHW1zjSWu0y8b8ZxvGx8bd4QAm+nsQJuiQPcblxvn9IibiGXRTu0lsJNsleEbtivCaPoApHWyEp391ciSEPXcUyA7c7Kcg8VZCmMo1ZjnSMdziHIu4k6w5SAZwZJShDOLy8Qz3PMk9fldZRutCs4AT+tVG6/rtRsf6w0bTRv1Ei0bABue3u0bnRvdG122UBv9G2l2gxvDG84AKBuWa9nLA2t5y462CQvYyFe6JrB1llez0jWLG8

K2Y1G9jasboRON4oTrzUv+g8XFzxucQJlyDIAgZr+GokC+0+I8Fjr7QSLuVkROmxfaLpsIwW6bsuQem8URW+7204fJ1ptCQLabOIL2ULcbnYGHblzuylRem519t27um+bTW2sBm96bckR+mysZsZtTDKCbkp3gm3GTLIw7G0HkYZsOm5GbX27dgTGbbuSum6mbiZv8iBmbTUE+m7eEaZtPOsmb5UGNnUhWuusTGzZrJtV98y3xp6NH/jFT+kgMw3

GB8YEZXePzQasoC67Dd+t8Y3ARdO7KS4tInkHDxYCQGnWAqyQ2zxBOMLktdustixuhcXmWm/u4lAvp+VLJu5GXUDn5YIB5+S518QhZ1XfzudWziJwLFiPcC+Ndm5bYxPRKBkBAsNBA34lcofR+UGy1CbkDsgupEwoLDNNNa/TL+mutayUbuROTm7PIFRvP69Ubb+t1G5/rjRtMoD/ropv/620brXEdG4kAYBs3db0b0BuEALAb8psIG7UjIxsqmx

8r+uvqmwD2c9wSwzvFrbBfBEIDqJVAqSQ2SpBktg79+BEza4CLc2v1K7n1fROlC3MD3RPig9FrDa3M61hLDksGxV0T5QvL3QG9ZeP3oHqAi+rSUfZQhJvi5YIQ5snZYicePQuNUhceeJMDnatLAYvrSxObIStgW7yblRsv6zUb7+v1G1/rcFvNG4hbEpvIW1Kb6Ft1w30bWFsDG7hbSBtKm6MbmSvjG9ZrmBtkVnPcpUvvcwFInDKpUSXikWCoGG

u+SNrH0fjrpX1i0CxbczNBc/dSiIuMDVFb0uRZm3Rzaz0CW5LmdJ7BU9UA8YAbgoAKbq3767Jbt8DYi4SSoyV4i8qeWNI7XQaTzWtmc5pbDKt/w+Bb/JsGW9BbwpsmWwhbrRvmWyAbqFtdG1Zbspu2WzhbQxt4W45bBFt660jr3ysLw8HDWpF8yObrDRXRyQ2WDfTD+KahpBtE65sb4uK2iwhLRuSdS9gw6l4uTN3rvi2969hLpotrW4hL6WvGuM

TD44BugMRQnDAyWzirxMHA4hTNrZ4e0oqQPotBSwJD7Js7q6zTvPOgW4/o1Vv6W1BbQpvGW5Tg8Ft/601bQBsWW61b0pudXdZbmFvYW/Ab3VsOW8qbvWvoG7mrBuvuWykAYFOo1bTQ93xdQ/Tk2iseOAWojHLLULNbFps6o7BLvYtRfP2LEIt9i2Be8VvDi/Rz21sgg6TbI9KNnRQYzIDBkrcCZ1tQbEfry4v9JKuLDWvri4MLemvbq2DrQYusS7

7L9+uaYO9bkFuCm0ZbsFs/W6Zb/1uSm0Db7Vs2W+DbCps9W9DbaBuqm0RbcouqI6Vl52AYioQbqJUTW7Rbi1gx7X/LG5sbG6xbNA0XgXaLjA2aTHhLqEvcW3trvFswi1aLJRI2i7hLFttoy2AJ1hUo6jTQmAAbJgm9S13vm58E1EvOXrRL0ok8MrprG3W828UbO4sgW9pbb1u6WxBbApuGWzBbIpt/W+KbANstW2hbEBug23KbENuKm8rbYxsI66

5b8NvPCx66TFGl+u4oRfx624R8LEVVq1qraxtay+FboPODgw1elvIk2xZ8PV7k29CDOZvo845LbdsWS42dKBva03jeHqTM22SOPku2MGDiggTfdC14YdsQo3FNFVuzI7Kzj6PlG3HbNVufW+Lbydtim0hb6dttW5nbHVsK2/Zb+Fsw26rbA1u2a3sjtCVWvnbJOtvo21htX3A8SDLguNtvq93NrdOyg0l8czKW2wJT3UsWG7RzFNuJW/FrbUuQ3u

jeyQMUBd+g9AnikSPbk+gzS231c0u0qe/K7GOqW1froOuR2+DrAtvjC9ybItsJ23Vb31s8IL9bm9vNWyhbGds9G1nbnVs520rbTlv5i22bhdvEW8kOc9yyy0WrWugtq3EEgk5Lm868v2Ce0nimJpu8k7UrDdt6ix0TIrJIy/hrYMv9/o8zkGt6qRtbMZNm2VTbfDvCM+QBXGuCO/tba/B2gBlwpoB2AAv9kA4HAFMQ6l2BgYb4ogtr6ux9cRsDmx

TLlb4kq4feNMsw49++tKtHKxnTC9t7ReqVnwDoO7VbX1sS29g7Utup2zLbBDsym/LbdluQ24fbKtuEWyfbWBtns5cTqBLg4ncTH8sPYLasphYGykbb6xtkG9Yrr/WuEPrcKIJhEACkW/B0WIQAI73HPs4AEsOYq5lDFeLzyvbLcGjpRE7Lxa7CBQcrAr1fHXrtQFtR21wrm0tMdlGACjDA6qOA9WiEAACk4K703QS8cgBKgw1bKdtb2/g7O9uEO3

vbXju522Q7uUsUOxgbRdszmy5zke1YQBblGTJPDhxVXMAlUFUrDP1dtUz9j0Zu6/RK+etGq5YjFXbFmF/hMKvQ8rnrmzuI24Sbd5IFoFnN5cYg+ELCdsu9DjFRbiPEohEJOGA3LY5RNi4TDssxSuuHK4ErztXBK7U75PUNO4h+zTutO7YrvbbtaNxKdXqQADg7Zltp2307wNvmIEQ7+9veO71bR9t+O5Mbtmtvc3Q7r8oegz0IakXdHfRmzxBu4o

AyIVucgwDxJtsRW3G8w+s5AN72C1Ej6+YAgz5xNr6+PFtofcOjqeOjo2qaCSyJOxpdKTtikfdKGTvKAFk7az6l65S7GfbJAxd0OUUpAM6AqgBYWjijvTDVMGHYfoBLDfJReTt4kiwr27j0K8xFsOPFO/9RJa6FG8MLfNveyxDrbEtQ6zcN/ztNOyaGQLvtO6C7XTuS241brjuA2+47INuDO11bwzt9W+2bblvF24XLOAvwYHPWuXabznqzKsvw0E

CQONuXI9fjsF6+wM6AlzhtNUO1kuOX8aarjdvLNd9KobtCAOG7gkYyW0Yu2yvzBXZ266YOdu4r0hhy0fmDs8tvO5E+0B3gE4Bb75P+Iz876uuP6PU7xwCNO4C7p4LAux07YLvdO7g70LuWW7vbnjtOu6Q7LruUO3KLLzEVU2MoXYNo2x/LPKittXDsT0kP25kjvS6Ic2N2EdHNK3pkULHNduRr39ue1mnjJgWiuxQA4ruSu86A0rtwCuoAPADyu7

6mEhOzuwRLn+1uXPx+9rTZgP3Ag+gmlO4L4UxSQFAA2djOAALrJMvWy0q7yPJGO+2RrJsFQ9frY5thS2MLEUvcm9W7tbtmu/W7FrudO+C7EACQu9Lbdrv9Ox47YNtDO127yLv9W6i7WBvmsRVTbQNMcss5gdVrlSQ2z8CiSOimE7u3I/6SbAqcquyrUOqgrrgAd9hl5JqALQCFUoq7BjspoipB+KtvvjI5JD7GO5ejKdp/vpSr5ArUq/EtZTuIO1

U7yDtGa4a7pJPX6U2AqdjPoEvOuuQw6eCkBPNOEKL4L71Qe7a729uwu2F2jrskO1DbIzvSq2M7cNtUO1XBc9xL82qSqsbYbskbSo7RGL64DFsf+RxWJn33oIGBxVFJwBwAkbvmKxYjzGncO+2LfP3Q8nZ7MAAOe93LapM/Sxiu8wXaRa2rUFWzYUkpmn74bgJskrl6ftb0XqvyENx7fqv/mwGre/3IC6FLRnGmE3/DvjUSeyRQZNyR2OSAxoBye2

2AlvXNu1C7bjuwew67Hbsaez47+duw22qbcovYC3LLDBqe0Ux+0Dgc4s0NUgaEe/Nbr0q7SuRucOXNqwV+jqvqfku7BhW+/au74iYke2wAZHu3dI3oVHuXaIsrPAB0ezuFyQMyiKtE1QD5yGQ52VvUK4urep2JsTFM4sI2vmtDbXhfu6nTAntlu6Qz1jtGA+jjcHvZ24rbmnvdu+M7entrNnPcKivTO+kQ27jqEY3cQk6+0Z2hEl7H4+ubMTtzW6

bb/QAOY6tj36tmmshr3368azT+bGt3GrRrJv5Ma6z+4GtddVT58GtO2WD79Guo+1D74PvS2ZD7IP7Q+zfQTtmpsmI7h2vOzSCgwPsY+xr+6Puw+zq0Ttn2mlT7xA7w+27ZHGu0/JXDmJtTK9jEg9gTyrrE4wAxGxNLtAw+Qtcgz2si6yVr/LqKa+VrUqiVazq9KV0aa3QTu2CujOyTQOv8e49berucK2zTr1uB2Bhb13sH20i7vjvIex2bCNs/Iy

6T7DRxTEO7Doom+6x+so4bYPXTtdummyarbnstSx2L/mtycvsd+D1F1JFr62tha877ku5u+0T7LOvmVXdhyWuvrKlrIRvCa2Eb96CpcGRQZ/DGyWdbT2tFa3Jrb2tlaza64vtS6z9rNWtjCHVradklW/6rMB0R24J7/NvCe4LbavvHWBr7xDs3e1V7zlsF2/d7cosFKxi7KGwNU5dgj/n3nVTE5DZ3ltNraSOWK3b725sQQ1wRHOvamedr7vtL4T

37GlR9+z77/Ft/2937nvt/Qi0slOvyOzHWprh9I76AO/Fvm09UhWv29MVr8mtHbDyhYvtfa5L7SSm/aw20/2u1M5n7iXvZ+y1rp3tta/dzf8PF+wi7zrtIe667EzvFSyv4u0YENmgYr0ON+50IiNqWWh17gPvHaxl8NOvqTao8XOtU69ZLtOspa9P7FQuDo5YbsWuO2/4y8D1OSyAHgftgByJbWJuutWug5iiV2IbQdZTfoM5ACSw0gDGKNID0AF

GACYVWy5QrQOhKhQvK0UWw/alTE4IVOxadzEvAW8QFf8POAEL4DoCaADWIrQCZPqCK4R7JgD3UwOq5JlnGboDOAGZtoiBL1Zzq/cDJ1on4/SPmfWgJxdswRQ6VP/AnYFQTkPYWZkQbq/t1lq37guPX4zIbchvbO/UOQ0zi+wc7I8paB566EwWxG1Kh165r9To+z3G2+WE4m0M9Nj84keq/EWuLUR2+K8d7SvtIO99a53tJBb87mVxMB95FrflsBx

wHmgBcBzwHB0SWRgIHQgcNEgCw8XDiB5YLfmApANIHM5u/KweeIChNljWLgc7sM81J6uBOQq0TibC59OB1ICsohQUHtsHGNWE9+gXMu4jl3atgDjfhE5pVpPIIq+DYB+oWeAcEByXqGCu+cSe70hNuXIy9CFB1OZ/x+MatAAG1UWSaALWk5TZ5a8lkpMukBzmN/cMKpRY7Xzu7sycrZhOniL4HLAcBB082QQcvoCEHfAezRuEH4t2RB6IHMQeSB/

EHcotNRS97tkAhsgG7nYWFdaIubxD/C0S7QuMKo9MAdmj/7aEL7iFRu03LYZy5B+vWkROzvd9KDwc0gE8HNAZZzm2OKE3KEeyw7lDU09d+kXsiVorrdMvzrTn7Z/sgTKGrqOMLB7PISwf+B3AA7AerB8EHM22hB/wHggc7ByIH0QdNABIHcQcJBw/7CqsnB6qWIcSwIocMFBzpLoZ+OQeZMp8HBQuJFsyFDpqkjAQOiGEKHqSs8+76tUYb532xES

Ae8RGRGrT81+7h5Kb2V31VxRyHoYhEDg+h/Idoa2Ix2uHVfaxrCodChyoegJQj+y0KXQctdDyAvQcpAP0HX6A7msMHxoBN8WXDaIXGrtuT7IdhJrKHXIfXwUqHfIc2h/aHPB44ayqHN9ComqmIbtu+TdDyx3QRgHWAv8z6AI3DQj4gZgTqHEbD2LfDYwfWy27SEkap2gZBlAczB+nTJhPzB4wHzAdohxiHnAfrB9iHmwddJtsHwgdRB2IHRIexB1

IHcous9TX7isQEsGnwe9GSo60t2VDizTXL96iWQpPA9dFPyDoHSckfBx+dTV3YxPWHY4DfoE2HkPFAh2xDj8O2Hncm/74vw1rjR70ZXSW7cIeMy1ATFbtGu9pKyYesB+iHgQdYh7wHYQd4hzmHewf5hwcHpIf5y6j2E5FFUFtA6sshFiIDb0mLOfFVDIeeIEyHMEsihpgjUHXsEysyebMws12ro3sQA96HvocR/QGH2rFdVKoA60RHwzuFxeP4df

NzMUrpFaCK7rWWABd0sjryu86kNTCDUC6LL7skByDkn3t9+atdMYciIx87P7upe2fdPstIh0mHfgcLh6mHawfcBxmHq4cRBwSHeYfEh4WHT8tL9WVLqHCvwPerw7sMgwgiShBC00azqzs6q3JAaUVFML/kqr2vB8ar7weMh22HT9v7vkYeHEdyWXaAD2uui32HSEdq41hg7iNAE54jjWsBxf3x0r6NMyQz/bIzh6J7uIbzhysHaYeERyuHuIckR7

mH+wckh3KLmA0VU5DiHZAdzekHfrtJndKl2VBc8dE7k2ath5O7uSNGRZITZGsZwy1zT4esu8G+wEeaAKBHssrVABBHeoDMcP+gNWANIy5HM/sPpDqH4IDBNd4LnEooCfNE1UnpWhZQ2Tt8I0WwkYeFJrUJ+f2/yFQHp/tTh+f7+bHIh4/oTYBGAFgkUHItccb4n6OoCl4LIGBJQ/GjqId4R0uH6Yd6R1sHa4e7B4SH5EeHB0/Ldc0nB4tQNr6o7R

gRurKG6MZjmlrnh3kHRHuwq+UO8YB2aErAgIdzRWI9TzJHSJ/IQEjKjr59Q7BHeyDrbge5+/q7KDsAe3zz31Y8ANmH7UdkRwWHXUffixPVS/NfA+4I/Q4raArFM0qB8IBIzj23BzZjTkfE670TokBOG5k6wyncfMWyqbJD5rVejV7Zc95Sn0eeod9HMua/R8z7/0ft2xqH0AfCskULxbwgx1MpYMfwQ77ZfhR/R7ZTvDDQx5FH96CHR21HpEdGRx

RH14wNkZNLMTVrdWx7Ss6IZqVbHbJICzejO7Mhq2rrRrsYC1RwykvxAL+LOAv/VKYBA0dN2evY0PaJLgnKEKtMW8LJr0ey07ubCdUn84ebdAu5+QwL+flMC31dN/MDXaX5NAtXmxX5BdWfAOjJXwD/CPwLQY5Io3AA7xutALOrbQtSbYkpVslR3O05urKp2bg4+RuPlmfCDyaJLl90ke6uyz3VHJsj9QX7MduB2LZNgHoyQPdKk4CsuP41uAAsWE

8KTRJpi1V6xABVDk2AS85L1XsA0sCEAPUATYCjgIO1EABMiFWG89Ju67N2IzBwAEvpMAAUABVoya1OqIZGC3buuVsOkwAK2JixyYBjQuCuLzYUg40D+avxAO+5OAv29CMokwkkXGNbt7Nz1uqkv0vPR4A17bVgPiFZkhtcaW2qYxZONat7jTDlpKcAR9X4gNELytj1gvQboob41PfYo8CNaOUwK8C8bfE0ZjPWrSDNShS8bZc0501qje06130JA+

hJk82G8n5D3OyZ633H+ekDxxhArzbMACPHvvrjx22ApABTx0YADFTzx8zcS8cLvVRtq8fkrUathIhbxxilieaujWNN+8cNfSCAlgZ1c/pDl25xIDMp+aDWunEEEGkTsELqECu7zajzEju8nqgZr8eLx6rQK8dBdGvHFs2AlH/HYiFt8n+UhvIRA2AnwtXHxz+sTibB++ENSwPQ8smAWYCUdfGAiAmEm7vdKu34Mx4wG0dqW/6Lbstvas7HiU1p3W

7Hx1hSQPnHUYCFx1MAJcfFMOXH+3QrMFJDEZ2G61vgVIbQjPAFFYE0E+9EwtONUw5HU706Qzw77maQmzabC9C9Qc9NYBTqQOGKt4FNFHvHA/rVY00UrpuoAKYniZvmJ6ozu/qu3cIOLACkLFg0RtAEWVg0I+WZm/rTUJv3QdFBTRQmJ2Yny00QzZYn6xo1Y9zs3pu2J8EnKM1pM4rVDkzV5BLAbifTGh4nVideJ4Gb7kejY4+HfFtxa7YdwZutjI

YngSeiQHYn0MHdgQ4noSdOJ+En1idRJyUnW0ES7uUncSfWVQknLicHzO4nnifTGt4nkxw402z7blx9wPEAv/NLtiwnEkZAHWg4p8KC6qMoER2FA6dspUbIpDMnVYuOx+7LjD7uBztH+fuoO0LbnwAex9D8myxoq6BAjAQtlAHHaKOF4KoYIcdhxxHHrhBRx5qAscfxx0ygScdiAF0bu4BkQJPAmcfZx9XRTKDCJ9FJoifVAEXHEidlx1jq0idVx9

JD50etQ7VJYOhfdGJOB8WpyjW0uK6J7Z3HI7VgQ18Hs2YYJ9WgWCdUbQPgO0BtnFRtDdJIp+/HvG1op5mcmKeO1lfCaWBEp8lYpnseRx0rlGtd211t2Kcop8fteKcYp8ftyZPisdYAdoDQEp0OtMs4MzkDmlbNsMTEkmil4VW2EmQ1kIKnsycLJ7wnT1s882WT3JubJ17HOye+x/sn0oWHJ8HH9kanJ6QM5yexeJcnyiDXJ5Tgtycpxw8n6cfPJz

nHbyciJ2InxcfI1pInfyeVx7InXT3yJ6pLi5VclhJkPw2VZdZHKriCXhXu6gc1KzZ92ifuewUuxWqmrTLivqdpqk9SRUZfdKSnC7NXwkgnbC09631L2EsBpxsqjZ3foIkAoK7CsccA5Et+e9lmkDtWyUdkHaESue6rrzvCp1WLkyaR3or7zaY8Jx7LfCcXva7H3gcwTNKn2yc+x3sn/scKp0HHt4snJ8rYZycXJzHHmqcJxzqn9ydpx08nmhYvJ7

nHkADvJwXHXyfiJ2anvycVxzInd/2ApzXHZFuijct641jASyHw8vvWZrH+8jXupzWrViNep/b7iRYVrcBNcRRBPdqqDdJ7p3E9YKpxPU69wadhpyGnXTWDgozr2ScO234zrxwnpx2th6e8ao2dFADgbR+n2hZ7637b6afB3t0Sq6RnmecV67NteOPbhadzJ0WnSguwh/4kpadLJ9tHKvsvW4InPCA1p97Huyd+xwcnTafRjC2n4ceqp+2nVyddp6

IAycc9p48nGcf9p4anlODDp58n3yfjp1InlqfTp3In7lvxAJ5bJYfmuYfYTjBPRVjrshCJfpvzAsdt+3Cn26ed+63TXeTd+oEADgAoSq2EDdLCZ2nkomckAOJn4ERBpw4IV6dKZxGnBK2bW9GnvJ5SZ14QpUBfIxcYEmeIgzNtOFBMJ4v7++vpp5+b4dzs0AJS6HB7Q5eDOOj5p3ZnnvhQZyf7JUSLJ57LpIsrJ+1rf8MoZ7Kn9acYZ0cnXwDYZ2

2n6qcdp3HHBGegrncnqcckZwanrycUZ8ano6emp6XHtGdTpzWDLb1liz6k3+n5qK7aUFP1AqnKXFUXwJonnqc+A9/76AASqjNqpl0lZ75pF6eKZ8Sn16dMgypnXl3p87ye5WfuaTCTPSdvYgowLABRRqMQQycRBeiTxZVkom3HlU4CpxBnw2eip2Wn4qecm4slhfvIZ36Anse1p2hn8qeBx35nOwjKp62nuGdBZ/hnNyeEZ+Fneqd9p1nH5GdpKL

Fn1GcJZxanSWf1A509Lf3PC3U2Sam2oAAIygeG3ZxnV51diM6MMcPfQzCt8KfMhz6nIScLGh6IYBT6anmRDdINJ7yaP2eLxqqUlWckpzen1We3pxAHy7sDE1tbjgVfZ0DnWDR/Z06RjZ1IgAeWY8cHGN1nSAVUqdPoCGjHmvb8a0cFu/Zn4GejZ3Bn8IfVO6r7SGeaYF5ndafoZ42nS2cBZ2tn0ccbZ9qnW2e6p72npGd7Z9FnB2cfJyanPyeJZw

CnDGeXZxrbOAulsPR+uioWsA9n3KGpYNxVG6d121onhWdku5LStZ16AHKECF1KhA3SKueThOrnAF3LUeDn+ueW61knnkc5J7DHYWJa52rn6/6IXckDMwCMgEk94IAEyOWAStgOKqyh+EDtAF0FLL3hh/BH9QL0w1MHrHU6uz6UsGeuZ4GL7meD49yblkILXSCATpTjQmKxjejhU6mNcACYAOVxxycrZzhnkcfrZ52nm2dhZ+znkWdkZ9znmmCUZ3

znNGcnZ4Ln1qeMZ9EjlYsm6MIGmwR3Z18ulWsjAcG7X/ntwJtNrU3l8TxHOzsFZ4CDcbu6o7fYZwBbTaMHpgfQ7MkA49YHIFbAPaEzBcDoQgGyuGfCCPSCrLmnOupxh3THqAuJh2HnHExG+VHn06vMALHnWsNRgAnnSef+ZynngWfM5xnnrOdZ58Rn+qe554Onc8iHZ2Onx2eTpyXnF2csx2fbDpXtHn2I9QUzkY/VgBmFftTO6cXW+5w7Hed1TT

unPqffx9qtOzS6NfvT5s02reqH/aMQK+h9l0GVBzIgNud25w7nTudmjC0ABwBu53EehTa4J5AX7odIB61nMUrTACJtxvhFtPPpSf0yymVEXa4ibcQAAB1pRzfwkuWuJRdbr1DZR0lEuUflW+UD6Xsr5xHn6+cx57gAcec754nnSqehx6tnaedH5yFnmedEZxFn5+dc55fnBedxZ/znxedWpw/nxUuf9cYNhjJbQKpogi4jM6B9Z0guKC9n30nX4/

CwykyR57gAfp5t5+kjiudd50JHxriGF2RAed0g466L6/st1YQLSSk2mOfivWBoFfm756CcJwg7W0fk526jngceoxpHPCDh52vnDsDR55vnvBfb57vnghcqpyIXGqdiFyfnEhc7Z5znA6dGp7znchdF53fnihe2g8oXgTsVU/cAcBXetLAi6DV2UTzwZrC5xdvz8L19gxYXOifi5By1q/bG/g5DeLX5nIuYwQNa9aa1zX0NF+HkNkP2bJK1PmOu9g

jncoNVJ/K1oWkrY5IO5NlNFy6HzA7jfYEDHRdHCOMXjIgnx70XPJQMtf0X84qA50MXopoRJzDHbKaEF3XjMMjVAKQXLlhhZGeFVBcfdaaHqA63GXq1XRezg9y1LRevKG0Xk005ajNNGxQ9F/mcfRflYy2agxd8ms61Hod40y0OtzgHAC2UlvV3OKetrQDYAN+pR2qcUllbcEdYqyDkc0XDJeMjfudoRyd7+Uf0B9x1nBehFwcA4Rdb5/HnAhfNpw

fnTOfxF1qnPCDdp5IXu2epFzFn6RdHZ+anWRf0Z6Xnl2dTO5ed75yNeBwMr0PP1VNaIcSa4PHEtYe8Se3KS7mUuC+tznvqo+3nChU1F96n3ed8lw0AApc6RlnOjhfFsAFI4v1ZSpL9r8PwO7fiE4d5R3QH38PqR6zLx1ghF5HnYRcb57iX/Bd758tnQhep52qnohckl5pgZJfJF1FnMhfX5/FntJf/J9kXv93KF+i7vUenIcP4pU0YEf++yW28qL

bGvQNK56UKN4ce/bQjyfN225MDXkcwK6nRxih9QMCXzmLTClC0EJdyeoRAB2pF48kDPofKTDs+gpEHAB5oT3qJvIyAKdCsqAbHnudwl97nMwVZR7GH/ucMy1qXQnswo3/D+pfcFxEXfBfRFwSX5peH58SXoWdJFxzn9pdpFyOnNJcTpy6X9JdKF/nLVdhJqeHJpKJZUAxHrH5cwAqQSIoN5zZ71mSSAN+gKHrHUXP4nP0WK/xn4peAF5KXtqQrl2

uXrKdylzs1W70yR+v9HiM98VzbwOs5hYGrKkflp4iHKP2Nl6vnBpfYl0aXkRd4l6aXjOdxF8Fn1pefALaXvZcX5/2XVGc3586XdGfJZ6p98id9uw17q7W+NurGlUvOSg8OIUJsJbCn0D3vZ1eHcbwAA/DMQANeM3VndXXeR6nRWZfI8JIAuZf5l7DS4lzFlzZY4Uf/h/QjhEtvYiOrzgCGhH2gBWGMgAUeq3ugyiCGY8G0sQiXzvF3wBJkXwMSaO

LnM1T3WwjjIUtBfX+7WEePl1KnM2dbJ6hncqcNp4tnMRfCF5aXXZfiF9tnAFfSF0BXhee358OX4FfvgzXHaHvvc+6Vsf4vO5D22iOqQzNMX3SGfU2L8uf/50QtTL5vYmwsnD7VYNErWOdxlsBp0zFnicQcOPUgZ3+bikf8fWtL7BeVW9ybZpexF8pXP5fdl2pXOecaV1SXA5cgV0OXYFdnZ839ORdjl0Hjo3Ra20w7XR1RWGYM8AVBl5YXtgFnlS

pyEQMa9YEDGN3JckVX8nJJA1/bw3ud29YbsIPxAyAnxVcHx0FTwdNnln6A9FhsRq5XQyWfmzILKRPu9b+bY4eOZ/5XGluBVwEXWdMpBSFXSld4Z8fnpJds52fnFJf7Z/nnjpfyF3SXulfVx/In9XssZ68Orij3VEX8b+epLkdkmPU3B5UXvYPeA53ntRcLa/DH7FsF9WULJQvgBx2r96eWi4+nMAeCW1dXeBeAR5w20WS1AHU5ZACdV1D96JMFa6

EhmTJoLYcDCkcsw9BnbBelg1pbVaf53F+XYVcs5zNXp+fklykXC1efALIXg5cC566XaX0am/EAz3vMlwB+wx2r+7tXNBN+RhwM0Ui5V+dXrvMQg6p0lktmS6jdWdADi0N73BMJW/DLTts929309NeNnV1mpgCVMH6A+nmudZeQjvVUqZiDgA2gScANV5fFp87DYlfNM/+71Y3BV7DXU1cJFwjXPZdRV5SXPOexV06X8VenZ3cD52fJV9jXBvuo1T

xITgennp7SdaNBnsJe+Wdil2dXEpcE20ZctA2MWResK1s8DfKDrA07Fw1nf9uytQ7XyQOJp+iHNbsx6eynjvVnPUgVUg259DIN8vuKC1TH4Ndz2yNXzMuL25d77wzy1+nnitc2l7NXSNd9lzFXwFca1xjXI5e61yRb8QDV+ycHlm7eQjNbM05qqx2JqriyXdZXNvvw3TuXgmc219NV4YOpw24NahWJgyGDHduw5+pnf9sdU43XyQNPCk0AydalwB

7nA+cuYQkNC0Vs3Vg45JKEi7313Nvh25qXXssIZ5Kn6yevIAnXVpcRV9nnUheq14tX1JdxV1nXq1czp/IntbXvc0b7DjjSqaiV+jLOvNpZfvDnACxHVdduPTXX+NtBc3Pd+EMTg9O7HlIzgwzZzScLg3dXpQdVCybnT1dwx+rMb9dS2XMNiI091y6A3dQYGo31v6dzEw6M2YP1/gcNnNvLSzCHTmdR15DXQVeL18nnHZdEl+FXqldr1/NXeeeo10

tXmRc6V4lXNoNul2OXj/0Ne1zw6KYfe/KldlH3s5Vc19d/55bXABe110Fz84PeiRuD3IzsN/3dltvrg8hDBFNbg1/XeCMw59KTHdes16JcfDf4Q9w3jZ3HACTI+AAE00Tzv1fWB5qTp4NpUM8QF4M/VQvnN+vCQ+FLstfoN/vnmDffl/DXydeI13aXgFfp11pXoFda16+DVgN714xnsgc4C1to66EulbQ3kXnkNjF+cuc316CNd9cIp4ODyKoYQ6

qNto1wQ0qNMICOjRhDeo2UQ7i01EOIQ2Vt+EOoQ0MV5EOHaVhD3To4QwbQYTeajc6NkTfYQ9E3LwhIQ3E33o1VV0zXP9ss189XkEPQQ1k3KTc5N1EKEY3hN5k3gCdRNyjHtZ2cN2MVMTeGhI2dfoCk8Ih2sYVKNxVGyEcO+fmN+r1ikHKV6b01l6W7aJcU54hn0NdIXMvXKleJF5FX69co13nHW9eZ1woX2ddkN9jXSQdlxsboq4ZaF9vYl9dwtT

zw2ujgS5unCudW17uXddffjdeNqnQLjQBN541ATaenyzO6zOBNFxiPje5N0E11rQX1+6cnjWeN9v5MMF83jzcbjdPkEE1MMB19BsEfN5knjLtLg7/XbtfiN5c3xa3XN/+NKMeATX83DzdPM2BNQLcvN5BNL31gt++n+xgBgPMKvttpp5MHgrpsllxDFpj29GSr2/3T17PbPvVuZ/PXe6ty14SXRjfTVyY3ytcLN/g3Szfq18tXxDfa10lX6ze518

cHzJcS+xmU7FWte4B9OrO/5zvz3jdnN6w3/QOPeifHCXInxzTXECeGQzZDDNfkp5AH0LdUa3/bNkOKt5QncSBhDfkzNcNVAHnX4s47Cxs0PTcjIG3RChK4zeDi+M2Yvl3jEtcpe1LXmEcGu5Wnlbv/kjM32DdzN7g3yNcct0OnhDfaVwlXvLekN1jXudfkh3jX2qIcmKxxrVUzlyq4edhpwT/nKzteNwCDLDf3134DZN0U7T+sTG2mzS3X1LUvUz

uMCgDu0DZD7D0IQKVXmXKU7ZLt7O15t1PN4rUwRCW3TkN5AG3XojeoJ9sCOcmVt7m3wYP5tx99Krf4Jn8l3SfvV99KyYAGDtpaUkD2F1A3hSYx/l7F6wS2vOalYPaBS6wXKDfDwwzHQRfx10y3cNcst3+XKddmN9FXatcZ19y3wbc2N/cDQucsx8WHJwcmCcPFu1fOpzmo0yaHDE9Hx1cx46dXabe+NxBDex2nmDSny8dUbTQtoQOMDW+39uQftx

/Hx+3ft823a5Nw53/bf7dx5AB3vG3Ad8kD2EiwIFG8UlrhscPXmUewFtIt43SyLQ44Vgfh11n7Q1cq68cro1fkM+NXXrfGN1u3pjfqVxvXBDfLNwe31jdxrW+Da1eMZ4WrBddtLZ94HwN7N64jD7o1mJxIzhcqw+fxhktPt3ZXWZ2S0h4tihZuLZODrnDCd0EtQjcuvZq3D6cwtxJCEndzLckDcb2t2ICIgzAyW9hdagMltnJuRn5YBUf7fleUAw

FXqDcEdxcDxtoTVxaXCte/l+UA/5cq14s3AbdUd0Q3h7e0d7Y3J7fKF1RH73O1oSgSXMch8BuyhOGRxLvAUeMPt4rzqbcCd+WpvDsjvEm8FyjAF+vHwy2bx6yqEy0Rd0SoUXd4J7/HcXeu19q3sLeTLdgXG8f4J6l32MftwOOAzUDrIeKF6neexhk9jpCXLWUXiIpqEndbi7e0t8Hn9Ldcm/o3Znedl963StfzN3g3Dpf2d0G3NHfCOTFdelfyJ6

ZHXlsLuqqWrHfc9WUr7VX2lLtQMKeBd3x32kM+Nx9nrdOkrVitUXc7NA3SS3dLmCt3iLQgd9pT65O8nut3a8erd8kDboD6vErYK+kmB7z7PCwGE9/NMf6JsZoc062eF1PX15c+F6JXTTOut7tHejdTZ2u3hjcbt0nXpHdstx13mlcZF9139+c519Q7xHW+1dDWCPSCTq43XjZE4VQcjYvII43T0gPzd+hXktJWrcl31Kqxp/3pBq3q0D/HmPcmrY

GnaXdUp9hL6Pc4F/j3dq1+p38XL81ESy9kgKR2aOO3RLeld9qDPq0SrgCQim51M8JX13O6u8snDXeTZ1TnNGDEd5u3Vnfbt+R3tndX5113Vjcg9/y3YPcDMx0yoIScMqUgCek9DnC1BlqZC4w3UrfBd35rz6dXN8yI2+Tgt2J3AXza9/C3uvdxFPr3eN24I9J3Ijegd2I3pTdG99WtJvdlrW9XtFeEw1fHw8d2gKPH98eTx0EENBewl5lD7hcLtT

xXYGf8V1+Aa6QIZt4XAX0vd6pH6JfIaUVHgdi0XWCdFu2QncEXjF34HS9tRB2sXWud7F0onVudfm07naDt9B0hnfxdu9cud2OXPUd417qyRWvGGuXLhzBCEKpFnjdMN5HVAAjwGDwo7csxSvEA6JJuaHb5bhAOtFijPAAhvelwEIDyUf73giO+5yM3KJdbbfEd42cux9hH3JssJBu7wlF0lq6pJg6xbIbLocep+E/duR1unbCdLF3FHS7tZR3u7a

id3F27nbxdB524nSQ3tYPxC3ArnkFKZOSSi74AkKnKksIr9fLzM3cusVx+4A7YAL1EfSNrxM2HI7VVsA44Bgda9BA2n/dNAN/3n2HaPiDo3FWa9vqDgiMT59S8nbByLQIuMj2T19YwWje/u2l7y+f6N3P3KQAL9/IusnSZPq4Qq/c12McAG/cp9+6d6fe790id+/ebnVxdufc8XQX3fF2HncX3DJcsx7JDcsvtnqBJdEfb2E4DvPV2LLvAhDYW1+

0NA4ia9uB1WXcxd/gnVG1gF5EzEBfZd7/HEg/QF4zXPRiwF9lx8Be2EB337AdJAN33jEqj+f33rhCD9/xuog8zLZvHcg9U97Qn/bPGhMaHg1C20DyAeWFrSP+gPgAmbuNLujtDI08YreN5/dG1i4vUt6ZzS7fSszqXznlXsN+x2A/1O7gPy/cED6aARA8kDwudTF1p90UdCJ0lHVn3B/c5917tefeYnfudtR1S92G3YPc4G567O7j8CK9DqosOPY

V+zpW8ZxoHjedVAMmAS9Wb8GFk4LvCl2RjoEO54SF3+In2fRUPTCLQNoYuYzFOF0IB4utuF18E74xis+H3nPO+F+M3/hcx1+pGmA8BDzgPS/f4D4QP6/cunTbtUQ+FHfCdXp2Z98idCQ80D0kPdA/BnQwPZ/chtxf3qWfTG3LLaRiG2ALCAyicD6x+4GndMl3hf3tWI/UP4HX1F9cXzRd1faQnTX3zFzcXixf6t1cXZZqG1AdjrRTfF4DFvxcohX

cPdocLF0A3BrX3F3sojxfQzXd9032vDxihSxfvFysXdrVrFxVjvw+MWf8Pttt3p8bnsneeTkZw5g9r8hZAlao2DyEEmgD2D8oAL6Y6tWMXMI8e2baNDw8zF01XoCfPD9YZrxdwj9WaiI+fF0y1sSdO18MX0xrJA4IHrP0OMUKllkKrmlGAO41wLsw1bZ2XPs4PnMAxNYiXTBfVl+P3kfflpzKzow+fdyEg4w9BD5MPK/dhDzMP851zD6n3Cw8rnZ

Pxyw9UD5xd1B2fAMDtyQ97ndidRffn9yln34urAC9LdGk0nd7R1fc2oDEYHY7HN6xHb/e/5DY1twJvuT/3qFc3DxNH0PLej4aUjwRzdUtdvWAlpoqXaYXKlyOHD3dUt0936pe3l+wryvutzijjkldjD/P36o94D5qPa/fED7MPeR16j8udGfdxDysP1A+mj+UA5o8bD6kPoZ2Y13WDa9HOgI/AtkpetOwIS6em+z6XyelGWpVcuakCD2qtgY+de2

793Xt3h+mykZdQt3AXz4enJLyPmoqTys62SXBmPCKP4/lYo4e72wK4de0H6Mvs+37AffcmhnxG3KNwAK6ASdgUAICqpskhBX73GafwNVWXqEdIN7h3ljsJh8Z3aOMEJai4ao+L9zmPoQ95jxEPuo9kDzEPSw+lj8aPFR1H9/n3mw+n9+kP9Y+UDI2PAa6wRa4oniCF/ED1LoPUCmTefGzMcouXaztsgOJhWQLgIpj2NQ/Ru32PS7YND4euG2yoTx

Lqs0RtDyeXXnUmynJHl5eIN54PuuPJj0Ub8Gdpj7o3vUZ/w1gPEw8vj9MP+Y86j4WPn4+LD6udP4++nX+PtA/H9/QPQE91j/ELh4AI2q4sr1VT1vkPMvMnYGzEeheQqwbDtyFETM5HieOPJMOPY0nf10zr44/4VyYFwDgZ/ToO9Cruun6Ae4+UUJ7eR4/iOUe71FdvqeuPblwh0zZGbqQcIErty3UOjJ5XmgPztxz3tXeKDfV3u6uNdyqP5QCb94

ud0Q/cT4aPvE8cXfxP6w+CT4BPaQ8iT2WLREB5KiNmbtKAS2x3mVfWuscStusoV3UPOE8Dkz1t8ZwQj+r1FVclV2GTKvWBA41XICfbdygnS4l1V0+VeU+RA4VPeXdVANAgasQaMO0AP6dqk0EdC7XdV8kTbvXyCwu3qA8YR2oLbrdrJ35P562kD9v35A+xD3v3fE+H9wJPAE81j9aPOw+2j/mrzUBAAXzCZJIulV53KsuiEBiKqmG5Jf2PRWdkpZ

X1wlsG9/blnFtHT+b3nBMFyUU3NVeU20lbN1ctZ4O30PLVAGFk0wD6AKqyjg90GjYOUm1Q4231ANd7A131INfi14NXBnfDV0Z3Iw8Xew+PyfeRD0WPO/cTT5QPU0+JDzQdFo8n99FPazcZD/p792DPBVBVXDIpC953K6dHxctQfYhpbZXXDffYT8pPb0eE2+zXWhDKt1nQCIOFNyZVxTe7d3/bNM+Qg/VPEgCRolGADKiMgNEO7Z0JDULX+wAADX

5Gotc4gwr7QM/qW3h3Vjtgzw+DgtYBT/MPxY8UD+ud2fdrD4jP1Y9Wj4wPNo8QV+5b+EClXT9gjYnxLbdHeM+pLhKuR/AV14j3HqfqxXtPwZeCkwaLVWMig/QNjtebF9XlVKXlT71Lrbewtx7XTs8MQwYAg1zTAPdrTk9XJtoTaDhB17L7qBUbvWqXAw8Kj1P3/Cchi6u3nwAyz9DP40/fj5NPYU/TTxFPs0+qz9sPR7c619L36M+Kiw5rAcbHIP

UV1FveczCMY3TIVy/33mtcJEu2+NkLdxc3XdcVw03XCYNdt3AA6rdG5xSnVhs3T53XwNPd16zP9KgoXXndvqTndxGSW3ZAHRVGcUxJDUtD7N0T1x5PfU8utwNP73dEg3HXsc+jT8xdCc88T0nPis8Vj5AAVY+RT3NPas8LTxrPzwu2wEyTuOsGpOIVNBMoxL+C6Oi7T1XPfmuP19hrfd2DDSOD9t3OJ5/XELejjz/XWI/E90lbd8/Pz9xMz9d0I9

ZP7tsXWapCvTC1ACn4EVNtTyPPP/UY9bA3+w0Xc3p3YNfIN3V389uSz2qVeoWWbVDPXE8Gj+UAbF1ljyaPAZ07z+nPwE+iTxWL/btQVWjso3d4sFLnbigS88EJHDsa9zB9ueHVz6j3YXcSN8ODHDf8N2iN7C88NwX13De0Q+JM0jdE97VX/UuSN/fPXC94jYHdyQOrCuOAEHJwACNcfs/Xkyo3S0NqN/SNoc+g15urAFuTh3WXefseZ2Hny89BT9

gvr61Gj/DPSs9mj1UdQk8oz0wPo5camzvwtkp/yuYtRfx6z+uV+t3lIfQvVRe4lf3FSIx+a003nC/xN4wNPi/5N/RDdM8+/ddPv9uwtwEvYi9+LyYPV2vGuMO3tYAIAP3AoMoKL49Vk16lICvKR9jkt9hNYc/BS5LXr3dzz6sne0fDTxgvH49jT1+Pa89wz8nPCM9mL0jPFi+1j6jPIE+z3MCk16s0ziB1rY41519e6iveOPX3DC8eL7vAYOJ+a7

q38nJKt4pN7w8o5sMvQi8dz7C3gy+9t2dPAC/C2Ea3YlvtwKcAf0yx2d8kIuWQL1JtWZPMeDaYgRJ2t2u+DrfIl1ePwM/iz7ePqC9jV4Cdcc9YLyWP68+rD5vPvabmL1FPdS9WL6D36M+5x/sjdijWIA78gk4tx52PUfmn/gj3jFt8Z6+dZnn8DwOPLs1Lch23Hs01t6W35kMvlc2ExbcnxzCvJ5VBmwVXWbdKgDm3UK9Nz0ivqiX1t4ivjbfIr2

/PGI9tz1AHf9exZtVPkK/Vt1iv+K84rzmEDbf9t1/l3D3U929ixMZV1XSWEDbJL27F0YcXLYQ4wtMWSUybvwnYd8f7Q1c0B4qPK7e6l/vP/XeazyLnBw+pTyHEakXXt0IkPUxH4gF3xM89L3X0lZSkkswvQMvZncMVMmc6Zzrnpl21nXqvKEqW5xrnEy9hLxJCRq/aZyavf51mrz3PLEitAK9kIgA/JLJWy8o5PPHEbPfKcd9kn4DnOzie9VPaWm

kt3FDEkvDxcY+Xc4mP4c/OLiKvkc8Vp0NPgieCqSZR/nkGoeUKqhfLpB9Dnwt+W/RmgRJMhnVLGVhq9OYy7zD92oY0z0Vgr+gAwyzmryU3UYLJA2CA4m0K2L9dy71ur7BTAlJkol6vVyA+r3IQfq/w9IgnDnbBryu1muPxj76Los/cJ5P3qY/PWwvXhfvxr/opia+s6cpMS3opqMIQG0/c9RziaUnBoGXPp9h5r1WMBa+EGgLkgr5ar++rcbxlr8

Ev1/Xt167PEkLJA9qNEaMCuNMKp5ICZEsRVGDpotEYMPdeqZLIS0VHZCnhDqt0mw4gPa/Dh32vYa9OtyExUa8jrxKnDLf7R6oBkel6uZQMOCgijRVT21BLtu/Lezct2eW0cUzjUeuvvdybrxV2IiSiwJxI4HUHr1J3NHPVV8evlU9dbckD4lwSChS+4o8GeTevapLYOCMI7c0VfKKo9VKxYB4IwygfrwfAX68ZhaqX6i8nDcDPAG889z5PfPdTN+

YRxlGTryzpEG/BFC/LnnUlUEr3qBhCGBi+Kq8iBChvdExobwCFMil2LAJHxC2lCjhvhK/Q5/hvLbeEb9hL1TngAHjAXwDhY83kAwBixxNdBsC3kIJQ8wAMAM6NibyazoutUCBwZ+uRUOZfQITUvfQR95ZkIgAQoO5v9m+oZjxvReDeb25vAwD4m9tFLm8+byFvj0wRKuFvwW9ZAFFvKDsxb7kA7m+Xxu/iiW8h3SFvr6AJ+mlv7m8/YqbF2W8hb7

lv94c2b77Arm9JbyFvaaC1yvlvcW9RAF5AqOZ65DtAOwhByFVvahx4gHVvBxiFOCeMUOZ65MMAzW9tb04mC8C6wD1vfkTAgAyANjhDCKJmoSGXwkFVEHAvIIDAW5pQgPgATnieraSQ5zvq4PuZPDLySGoBqUDJoMfzmeCTHKnIaUkCLuPgzW+0JujggMAr5M7wJbgkAGXq/wBXbzMCGHraBHdvnrqjwHxrpoQcwBNId28dXeetUIAHpJUeuABahJ

JiYkkngEDvbTSP8B6E1IDIegvAf28A7+NaDULtNBiwoO/jAODvx28lbxCg8W90ItcoTW92YCVAA4BZQCeSu28YAA32728AbAMC+7Qm0ujJavT20MegLVTHb3YA20xLwAE6BcDG+Ge+zbpE7w2oXwB15IwARVKE/gTvKEhhAMEA2VTBQKn5/W8vt3RwxYAGAPxAAu/vRpecoQBqdN5AXO8GrlqgbUDgADegyljhACHAVcCtQEAAA=
```
%%