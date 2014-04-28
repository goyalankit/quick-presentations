## Pragmas in LLVM

---

Files related to pragmas

1. llvm/tools/clang/lib/Parse/ParsePragma.cpp
2. llvm/tools/clang/lib/Sema/SemaAttr.cpp

---

llvm/tools/clang/lib/Parse/ParsePragma.cpp

1. defines a custom struct for each pragma that inherits `PragmaHandler` and implements a virtual method `HandlePragma()` that checks the syntax of pragma

2. defines `Pragma::handlePragmaCustom()` that is called while parsing
   the statements. declared in `pragma.h` that calls the Sem method for
   semantic check

---

## Example 
\#pragma unused(id)

---

`llvm/tools/clang/lib/Parse/ParsePragma.cpp` contains various examples.

```
// inherit ProgmaHandler and implement virtual method
struct PragmaUnusedHandler : public PragmaHandler {

  virtual void HandlePragma(
                              Preprocessor &PP, 
                              PragmaIntroducerKind
                              Introducer,
                              Token &FirstToken
                           ); {}
}

```
Parsing of the pragma happens inside `HandlePragma()` method.

---

`llvm/tools/clang/lib/Parse/ParsePragma.cpp` contains various examples.

```
// clang::Parser::ParseCompoundStatementBody
// clang::Parser::ParseTopLevelDecl
// both call HandlePragmaUnused 
// defined in llvm/tools/clang/lib/Parse/ParsePragma.cpp

void Parser::HandlePragmaUnused() {
   assert(Tok.is(tok::annot_pragma_unused));
   SourceLocation UnusedLoc = ConsumeToken();

   // ActOnPragmaUnused defined in Sem
   Actions.ActOnPragmaUnused(Tok, getCurScope(), UnusedLoc);
   ConsumeToken(); // The argument token.
 }

```

---

## Semantic Analysis

llvm/tools/clang/lib/Sema/SemaAttr.cpp

---

```

Sema::ActOnPragmaUnused(
                        const Token &IdTok, 
                        Scope *curScope,
                        SourceLocation PragmaLoc
                        ) {}

```

This is where they check that:

1. the variable exists
2. only variable can be argument.
3. variable used but marked unused. 
4. Store information in a vector

---

LLVM has no knowledge of pragmas. Clang passes them on in form of
metadata.

### ivdep pragma implementation

http://lists.cs.uiuc.edu/pipermail/cfe-dev/attachments/20130305/17027ddc/attachment-0001.obj

http://clang-developers.42468.n3.nabble.com/Emitting-loop-metadata-in-codegen-td4030580.html

---

lvm/lib/Transforms/Vectorize/LoopVectorize.cpp

You create hints in the constructor using metadata.
```
925 getHints() // gets the hints from metadata.
```

---

### References

1. `http://llvm.org/devmtg/2013-04/pellegrini-slides.pdf`

