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
