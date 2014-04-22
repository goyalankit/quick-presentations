
#### Conversion from Instruction to MachineInstruction

---

# Removing instructions
## Target Independent
---

#### eraseFromParent()

`llvm/lib/CodeGen/CodeGenPrepare.cpp`

// Build a truncate instruction.

truncation due to casting may lead to deletion of original instruction.
it has an undo method, which erases the instruction

Codegen preparation. Common to all architectures.
We may loose instruction information here.

```
// OptimizeCmpExpression - sink the given CmpInst 
// into user blocks to reduce
// the number of virtual reduce registers
// that must be created and

OptimizeCmpExpression(CmpInst *CI) {}

```
---

```
OptimizeNoopCopyExpression - 
if the specified cast instruction is Noop copy 
(e.g, copy from one pointer type to another, i32->i8)
Also adds another 

```

---

#### CodeGenPrepare::DupRetToEnableTailCallOpts(BasicBlock *BB)

 Look for opportunities to duplicate return
 instructions to the predecessor to enable tail call optimizations. 

```asm
/// bb0:
///   %tmp0 = tail call i32 @f0()
///   br label %return
/// bb1:
///   %tmp1 = tail call i32 @f1()
///   br label %return
/// return:
///   %retval = phi i32 [ %tmp0, %bb0 ], [ %tmp1, %bb1 ], [ %tmp2, %bb2 ]
///   ret i32 %retval

/// bb0:
///   %tmp0 = tail call i32 @f0()
///   ret i32 %tmp0
/// bb1:
///   %tmp1 = tail call i32 @f1()
///   ret i32 %tmp1

```

---

#### CodeGenPrepare::DupRetToEnableTailCallOpts(BasicBlock *BB)

```
// Duplicate the return into CallBB.
(void)FoldReturnIntoUncondBranch(RI, BB, CallBB);
```

---

#### TypePromotion

creates a truncated instruction

```
TruncBuilder(Instruction *Opnd, Type *Ty) : TypePromotionAction(Opnd) {}

```

---

```
class SExtBuilder : public TypePromotionAction {}
```

---

// a = b==b ? 2 : 3 -> if(b==b) 2 else 3
```
CodeGenPrepare::OptimizeSelectInst(SelectInst *SI) {}
```
---
// Some targets have expensive vector shifts if the lanes aren't all
the same
// (e.g. x86 only introduced "vpsllvd" and friends with AVX2). In
these cases
// it's often worth sinking a ShuffleVectorInstr splat down to its
use so that
// codegen can spot all lanes aren identical.

```
bool CodeGenPrepare::OptimizeShuffleVectorInst(ShuffleVectorInst *SVI) {}
```
---

### X86/X86FastISel.cpp
---

X86InstrInfo.cpp

---
bool X86InstrInfo::AnalyzeBranch{}

Two eraseFromParent: 
1. If the block has any instructions after a JMP, delete them. => dead code.
2. Delete the JMP if it's equivalent to a fall-through. //jmp not required.
  
 
Question: can we even instrument this.

only modifies jump instructions which can't even

jCC L1
jmp L2

we could also instrument how many times a particular branch is taken.
This transformation may potentially interfere with that.

---

### optimizeCompareInstr

Check if an instruction already performs the same compare.
---

### optimizeLoadInstr

This may effect the instrumentation

```
// Try to remove the load by folding it to a register
// operand at  the use. We fold the load instructions if load
// defines a virtual
// register, the virtual register is used once in the same BB,
// instructions in-between do not load or store, and have no side
// effects.
```
---

