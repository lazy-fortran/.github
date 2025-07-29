## Architecture Overview

The *lazy fortran* toolchain splits work across three packages—fortfront, fluff, fortrun, fortc, and fortad — each focused on a clear purpose. This keeps code organized, dependencies minimal, and interfaces straightforward.

---

### 1. fortfront

**Purpose:** Core analysis.

**Includes:**

- AST types for untyped and typed trees
- Lexer and parser to build the untyped AST
- Semantic analyzer to resolve names, infer and check types, and produce a typed AST

**Outputs:**

- Untyped AST
- Typed AST

**API & CLI:**

- Programmatic API for lexing, parsing, and semantic analysis
- API and minimal CLI (`fortfront`) with subcommands to dump tokens, AST, or typed AST in JSON

**Dependencies:** None

---

### 2. fluff

**Purpose:** Source-to-source transformations and static checks.

**Features:**

- AST-based formatter for *lazy fortran*
- Standardize command to emit Standard Fortran from the typed AST for both Lazy and Standard Fortran
- Advanced lints inspired by Rust
- Optional JSON export of ASTs for integration

**Interfaces:** Provides CLI subcommands for formatting, standardization, and linting — all with optional JSON output.

**Dependencies:**

- fortfront only

---

### 3. fortc

**Purpose:** Full compilation backend.

**Features:**

- Lower typed AST to HLFIR (an MLIR dialect)
- Continue lowering to LLVM IR and produce object code

**Interfaces:** Offers a compiler CLI to produce object code.

**Dependencies:**

- fortfront for typed AST
- LLVM/MLIR libraries

---

### 4. fortad

**Purpose:** Automatic differentiation.

**Features:**

- Supports `!$ad` annotations to mark functions/subroutines for differentiation
- Integrates with Enzyme at the IR level during compilation
- Transparently passes AD metadata through fortc or Flang toolchain
- Details of IR integration to be defined

**Interfaces:** Exposes compiler flags or annotations to enable AD on marked routines.

**Dependencies:**

- fortfront for AST and semantic info
- fortc or Flang with Enzyme support

---

### 5. fortrun

**Purpose:** Code runner and package manager.

**Features:**

- Depends on fluff and FPM (Fortran Package Manager)
- Executes *lazy fortran* or Standard Fortran applications
- Global source and object cache for rapid incremental builds and runs
- Manages dependencies, project layout, and environments (inspired by Go, Rust, Julia, Python)

**Interfaces:** Provides high-level run and build commands for development workflows.

**Dependencies:**

- fluff for parsing, analysis, formatting, and standardization
- FPM for package resolution and build metadata
- Any supported Fortran compiler

---

### 6. fortnb

**Purpose:** Notebook interface for Standard Fortran and *lazy fortran*.

**Features:**

- Implements .f and .md files as notebooks similar to jupytext
- Supports both Standard Fortran and *lazy fortran* syntax
- Provides export to Markdown and PDF

**Interfaces:** Offers API and CLI to convert notebooks to Markdown or PDF

**Dependencies:**

- fluff for parsing, analysis, formatting, and standardization
- fortrun for running notebooks and managing cache
