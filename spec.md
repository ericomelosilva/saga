# Saga: A Minimal, Probabilistic language for Biological Modeling
---

## 1. Overview

Saga is a language for modeling complex biological processes, with a focus on gene regulation, transcription, and translation. Inspired by Lean and Haskell, Saga is built on a minimal kernel that integrates a strong static type system with inbuilt support for probabilistic reasoning. By keeping the core kernel small and extensible, we aim to support high-level abstractions that can accommodate uncertainty, fuzzy predicates, and probabilistic inference.

---

## 2. Design Philosophy

- **Minimal Kernel:**  
  Saga’s core is built on a minimal set of primitives:
  - **Universal Type (`Type`):** The universe of all (small) types.
  - **Proposition Type (`Prop`):** For logical statements and proofs.
  - **Probability Type (`Prob`):** For representing probabilistic values (constrained to the interval [0, 1]).

  These primitives are static and do not participate in runtime evaluation—they serve as the foundation for our type system and logical inference.

- **Domain-Specific Extensions:**  
  On top of this minimal kernel, we define domain-specific types (e.g., `Gene`, `DNASequence`, `mRNA`, `Protein`) using inductive definitions or records. These types capture essential biological concepts and come with methods (e.g., `transcribe`, `translate`) for simulating biological processes.  
  Additionally, we incorporate probabilistic extensions:
  - **Fuzzy types:** E.g., `Fuzzy<T>` to wrap values with an associated confidence level.
  - **Operations on Probabilities:** Built-in operations (such as addition and multiplication) that ensure computed values remain within the valid interval.

- **Haskell/Lean Flavor:**  
  The syntax will be influenced by Haskell’s functional style and Lean’s minimalism. We aim for clarity and expressivity—using keywords for domain-specific entities while maintaining a rigorous type system.

---

## 3. Core Components

### 3.1 Minimal Kernel

- **`Type`:**  
  - *Definition:* The universe containing all small types.  
  - *Role:* Used for type annotations and compile-time checks.  
  - *Operational Semantics:* Static; no runtime behavior.

- **`Prop`:**  
  - *Definition:* The type of logical propositions.  
  - *Role:* To express statements and assertions about programs.  
  - *Operational Semantics:* Propositions are not reduced during computation; they serve as proof-carrying type annotations.

- **`Prob`:**  
  - *Definition:* A primitive type representing a probability, values in `[0, 1]`.  
  - *Role:* To incorporate uncertainty directly into the type system.  
  - *Operational Semantics:* Arithmetic operations (e.g., `prob_mul`, `prob_add`) follow standard rules, with checks ensuring results remain in `[0, 1]`.

### 3.2 Domain-Specific Types

- **`DNASequence`:**  
  A list of nucleotides representing a DNA strand.

- **`Gene`:**  
  A record with attributes like the gene’s name, sequence (`DNASequence`), regulatory elements, and a method `transcribe` returning a fuzzy `mRNA`.

- **`mRNA`:**  
  A record representing a transcript with potential isoforms, stability metrics, and a `translate` method that produces a fuzzy `Protein`.

- **`Protein`:**  
  A record that includes its amino acid sequence, structural predictions, and a method `function` that computes its functional outcome in a given cellular context.

- **Context Types:**  
  Types such as `TranscriptionContext`, `TranslationContext`, and `CellularContext` encapsulate parameters affecting biological processes (e.g., cell type, signal strength, pH, temperature).

- **Fuzzy Wrapper:**  
  A generic type (e.g., `Fuzzy<T>`) that attaches a probability (confidence) to a value of type `T`, allowing operations to propagate uncertainty.

### 3.3 Operational Semantics

Saga uses a call-by-value, small-step reduction strategy inspired by lambda calculus:
- **Values:** Include lambda abstractions, literals (numbers, booleans, strings, `Prob`), and built-in constants.
- **Beta Reduction:**  
  - The rule `(λx: T. e) V  →  e[x := V]` applies when a function is called.
- **Evaluation Contexts:**  
  - Reductions occur within contexts, ensuring subexpressions are evaluated appropriately.
- **Kernel Primitives:**  
  - `Type`, `Prop`, and `Prob` do not reduce further and are solely used during compile-time and built-in operations.

---

## 4. TODO

1. **Specification:**  
   Finalize the language’s specification (this document) including grammar, minimal kernel definitions, and domain-specific extensions.

2. **AST and Parser Development:**  
   Define the Abstract Syntax Tree in Rust (`src/ast.rs`) and implement the parser (using libraries like Pest or Nom (`src/parser.rs`, `src/lexer.rs`).)

3. **Type Checker and Interpreter:**  
   Develop a type-checker (`src/type_checker.rs`) enforcing the minimal kernel’s rules, and implement an interpreter or code generator (`src/interpreter.rs`).

4. **Probabilistic and Domain Extensions:**  
   Build a module for probabilistic operations (`src/prob.rs`) and extend domain-specific types (Gene, Protein, etc.) with methods reflecting biological processes.

5. **Testing and Iteration:**  
   Create sample programs (e.g., in `tests/sample_program.saga`) to test the DSL’s core functionality. Iterate based on feedback and progressively add features such as dependent types or fuzzy predicates.

6. **Integration with ML Components:**  
   Eventually (big pie in the sky dream) integrate with neural ODE/PDE solvers and ML modules for advanced hypothesis generation and inference.

**End of Specification**
