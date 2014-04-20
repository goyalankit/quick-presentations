
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
OptimizeNoopCopyExpression - if the specified cast instruction
is Noop copy (e.g, copy from one pointer type to another, i32->i8)

```
