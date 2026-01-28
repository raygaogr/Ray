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

QL8ScnW3: [[Attachment/Canvas/CUTLASS.excalidraw.md#^WZGr9W59]]

%%
## Drawing
```compressed-json
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

5fUcKCITzNhAoAINGJGrUrI5VfVkLQD5aSUZO6Hb9IsAtyR278EYRKhSlgKxzRgoHI+X99ZjUlYar5L9G0uR/RuE9NYdNynW8hmp5s05HwgkZR5lHWUfZR+IBOUe/QblHeUbLTP9HKoAAx1Q707XUOsBVXi2cAXmdGUfGAOhEZfRpSISzSAGPRw36hUejsVNad/0kcOh88LH+WqRhA4cVCtiGOAa1q2VGhkc1C3tH+0bfdQdH1UdHATVHR0doa8d

GDUa7OadHZ0fNRhdGKMAM9ZdH7UbXR0UFnUa3Rt1GPUdma/dGfUb9R49HA0ca0c9GeYYzvGxh1Ej+Y5GVtuIndR2rc4bX4EBkdoDg1SfJ00cSi7Yts0YyzZGGQIvMVEzGAenoSDGGLQZcNJYrvKyk80Js9v0vjI7K/YIae/NbPgb4R7uHO0cEC/+7TUnYx5VHOMaHRnjGR0e1R/jGjYQnRqdHjUaKCOdGLUdW2pdG7UdXRx1HZMddRndGFMa9Rg9

GVMYDR09H1MYU6vHIvZSqA98AM4evJMZBEsxT+CVQbXpnujRGRUzfRmzHqkcmbKsKmTUZKFjCmTRXNFk1jEvHa2E1YrKnawk0ErNLalKzgzWsRHfsDajckqQK4AEV7LyAN8lFE2bH5sZf6EKB6AGmxpbHUIBWxnopsHC1AdfIWRKEMzH722or/IA8w/GUwnrHPtT6x/NqjoLysr00i2oHHLqz4hujaxEzJsZ3KDbHvJOWxmEBSj02xubGvsczqKb

GmHp+x7bGYCl2x2AB9sfFEoQzDEa2uzor8IfbXUxQIwGwx+Tjzrnwx7pg7/sZAYjH4bx5+47G18lOx7rGSSkux9fJrsdysobHiimAtR7GcwbjNWC0AwjWx97HR4E+xhbHwpPpx1bGAccZxrbGvsZ2xqsMwceUKA7HhHvI+FwBjgDYAQeBovzEAQgBjQH98CgBwQEmYjpju0zS+ii0yMdmgafqBZO0Y/TxaMfN+1iGAsfbR637gsYjh+SsixEYmvt

GIsdVR7jHeMdixz9qBMcnRoTGksdNR0TG0sYkxjLGHUfXR7LHt0aVxXdHFMe9Rw9H/UZPRs9HSsZpG+0SSoTA8Mus/SWf63fVV2lKrKlGo/vUa35gV1VBiP8BkoQsx3RrX0esxq3rzFV0O0ZlMOTjx+4HDesuqzm18/tN4wv7OEd2S3qCekbbRvpHsm1bBqJHe4fOfcLGB0aixk3Gx0fixwTGjUZnR5LGbcf8O9LGV0YdxmTHN0Zyxl3G8saUxj3

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

Xt7B2GrfXtuO3N7blt+rWU5dvVrXRQchLksKcMLY/vX7AG6RujN9W8LYR16CWluajJnWXR1IU12dTJ1JVlgLX4zJXGjpLK7bYOgA3Envft/zXdZZeMo0G16YlOiuniLTs0W8ypiC6u1wh2gBD8dIX0AC+beb5F8VjsMlXbVZzMPw1f8edl063JLfOt6S2FlfLGDR2ebcDt2e3g7d0d6M39HYjt762V7cltmO28Hbulgh3gbaIdoUXukjxU/gkfdX

YUyJcHsDLOdotNpRcdjLjL7eTxi4YalghubkEwiGJSLfhtLEIAEj62X2cAJOHnBOyJ2yA/YejGduW4NE6iQxce5daHYXWLpU/eznaR7ehRulXNxZ9N3paFGDl1K+6qw2JSeVcU7uleOQB8wdqdnR33rYad7S3Rbeadox217eltje2zLfMd2E2RuYHBqDlrXwWZOkdlAa5gEqg3ldM+qCHoYf31pyVD9bYJipHv5ZOYBLXUbpN1+EIkXa71403iKQ

LQD+bjukp8O+as2kiSA6iYFfbYPYBL0C7IVYqUiOv3TZ8UFcNiSmGT6doNmjm9HBgdpdXk5tudvr96tEIAR53alYA7drQ/JRh9AD6BbbQdr53w7fMQQx2DLf+d2O3ITa0Nwh3E7cQ7L+4xeasdjhUUZSFIY7i0aN1Qu6lRCIYd7tnXOHP10fXfIsqGY12e+11Y+RXK7dAxyS9wMcSyGZ3R8e6uhZ2UyNZlFZ3lADWdil9cAAgN6AdhHrB6IGKUgG

dAVQBjLV1R3phqmGbsP0ByRpKonJ3TiXcVrah7TdEwST9rV18VsmtdaJON/NnXRZfJ8p32wfjGnl37nf5duCFBXZedkV33nfKtoW3PrZ+drB3ZXalt+V2WrcVdrp3lXb9or+4L5cLFizFJCGJRzHDa9bWLeGggSD6+qpWpeMwcM+CCudRcD+WmlYoR/WTJnbaVzYkNL19gZ0Bh3ditqpdnhk7VursX4AGV8ujXDGGV6iWcdCZdtJIWXZQitgHTBb

Jl7xr5lezd9ybc3b5dgV3nneFdt52UHbqdz53hbe+dpe3fnardtp2ZbehNkG3iHcjd6y3XYgs8wt1AorHBhDr6dlVkg13P1c0B4+i/lePogFXZmytd4FX72YCJzf721z9digAA3aDd50AQ3bEFdQAeAAjdg9NYidnfQcX/dZMVi4YYvyTabMB+4Fv0WMpRhYqmKSAoAAnsZwAw9Ztl41WlzHqHKoHf3xVUSlXihZF11aWxdcudjaWfbZzd44A7nf

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

f5byBYt+f7LVA2oHAAG9AArAqWnDkq4FagIAA===
```
%%