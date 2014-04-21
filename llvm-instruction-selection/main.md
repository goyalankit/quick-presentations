
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



