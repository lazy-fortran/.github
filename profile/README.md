## The Vision of *lazy fortran*
**Make Python Fortran again!**

**Disclaimer: This is an experimental work-in-progress programme that is only partially implemented and where interfaces at all levels could undergo substantial changes.**

*lazy fortran* solves the two-language problem in Fortran by providing the experience and tooling of modern languages while maintaining, performance and long-term stability of Fortran. It is inspired by the best features of Python, Rust, Julia and Go, builds on the `fpm` ecosystem, and integrates with standard Fortran compilers and tools.

### Toolchain Overview

The *lazy fortran* toolchain splits work across several packages: fortfront, fluff, ffc, fad, fortrun, and fnb — each focused on a clear purpose. This keeps code organized, dependencies minimal, and interfaces straightforward.

---

#### 1. fortfront

**Purpose:** Core analysis.

**Includes:**

- AST types for untyped and typed trees
- Lexer and parser to build the untyped AST
- Semantic analyzer to resolve names, infer and check types, and produce a typed AST

**Outputs:**

- Untyped AST
- Typed AST
- Standard Fortran (F90) code emission
- JSON export of tokens, ASTs

**API & CLI:**

- Programmatic API for lexing, parsing, and semantic analysis
- API and minimal CLI (`fortfront`) with subcommands to dump tokens, AST, typed AST in JSON, or emit Standard Fortran

**Inspired by:** go, julia, rust, python, lfortran

**Dependencies:** None

---

#### 2. fluff

**Purpose:** Source-to-source transformations and static checks.

**Features:**

- Source code formatter for *lazy fortran* and Standard Fortran
- Advanced linting with diagnostics (inspired by Rust analyzer)
- Static analysis checks and code quality enforcement
- Code transformations and refactoring suggestions

**Interfaces:** Provides CLI subcommands for formatting, linting, and static analysis.

**Inspired by:** ruff (code formatting and static analysis)

**Dependencies:**

- fortfront only

---

#### 3. ffc

**Purpose:** Full compilation backend.

**Features:**

- Lower typed AST to HLFIR (an MLIR dialect)
- Continue lowering to LLVM IR and produce object code
- Includes the MLIR backend implementation

**Interfaces:** Offers a compiler CLI to produce object code.

**Inspired by:** flang (HLFIR -> FIR -> LLVM IR or SPIR-V for GPU -> OMP target), lfortran (typed AST -> LLVM IR)

**Dependencies:**

- fortfront for typed AST
- LLVM/MLIR libraries

---

#### 4. fad

**Purpose:** Automatic differentiation.

**Features:**

- Supports `!$ad` annotations to mark functions/subroutines for differentiation
- Integrates with Enzyme at the IR level during compilation
- Works with any Enzyme-enabled compiler (ffc, Flang, etc.)
- Details of IR integration to be defined

**Interfaces:** Exposes compiler flags or annotations to enable AD on marked routines.

**Inspired by:** tapenade (decorators, e.g. !$ad mode=reverse)

**Dependencies:**

- fortfront for AST and semantic info
- Any Enzyme-enabled Fortran compiler (ffc, Flang, LFortran, etc.)

---

#### 5. fortrun

**Purpose:** Code runner and package manager.

**Features:**

- Executes *lazy fortran* or Standard Fortran applications
- Global source and object cache for rapid incremental builds and runs
- Manages dependencies, project layout, and environments (inspired by Go, Rust, Julia, Python)
- Provides caching API for other packages

**Interfaces:** Provides high-level run and build commands for development workflows, plus API for cache management.

**Inspired by:** go, julia, lfortran

**Dependencies:**

- fortfront for parsing and analysis
- FPM for package resolution and build metadata
- Any supported Fortran compiler

---

#### 6. fnb

**Purpose:** Notebook interface for Standard Fortran and *lazy fortran*.

**Features:**

- Implements .f and .md files as notebooks similar to jupytext
- Supports both Standard Fortran and *lazy fortran* syntax
- Provides export to Markdown and PDF

**Interfaces:** Offers API and CLI to convert notebooks to Markdown or PDF

**Inspired by:** jupyter, jupytext, weave, blocks (caching runner), lfortran

**Dependencies:**

- fortrun for running notebook cells and managing cache
- fluff CLI for formatting and standardization (optional)

The *lazy fortran* toolchain splits work across several packages: fortfront, fluff, ffc, fad, fortrun, and fnb — each focused on a clear purpose. This keeps code organized, dependencies minimal, and interfaces straightforward.


### Packages for Scientific Computing

---

#### 1. fortio

**Purpose:** Unified I/O interface for scientific data formats.

**Features:**

- Low-level I/O routines for NetCDF/HDF5, parquet, csv formats under one API
- Memory-efficient streaming and chunked data access
- Format-agnostic data type mapping and conversion
- Error handling and data validation across formats

**Interfaces:** Provides standardized read/write API with format detection and uniform error handling.

**Inspired by:** Low-level I/O routines for NetCDF/HDF5, parquet, csv under one API

**Dependencies:** NetCDF, HDF5, and parquet libraries

---

#### 2. fortsql

**Purpose:** Database connectivity and SQL operations.

**Features:**

- Low-level SQL API for various databases (SQLite, PostgreSQL, MySQL)
- Connection pooling and transaction management
- Prepared statements and parameter binding
- Result set iteration and type-safe data extraction

**Interfaces:** Offers database-agnostic SQL execution API with connection management.

**Inspired by:** Low-level SQL API for various databases

**Dependencies:** Database-specific client libraries

---

#### 3. fortplot

**Purpose:** Scientific plotting and visualization.

**Features:**

- 2D/3D plotting with customizable styles and layouts
- Statistical plots, histograms, and scientific visualizations
- Export to various formats (PNG, PDF, SVG, EPS)

**Interfaces:** Provides high-level plotting API with figure and axis management.

**Inspired by:** matplotlib, pyplot-fortran, gnuplot, plotly

**Dependencies:** Graphics libraries (Cairo, FreeType), optional LaTeX for typesetting

---

#### 4. fortarray

**Purpose:** Multi-dimensional array operations and data analysis.

**Features:**

- xarray/ndarray-inspired interface: load, aggregate, plot data with ds%plot(), ds%var%mean()
- Labeled arrays with coordinate indexing and alignment
- Broadcasting, reduction, and transformation operations
- Integration with fortio for data loading

**Interfaces:** Exposes array API with method chaining for data manipulation and analysis.

**Inspired by:** xarray/ndarray (load, aggregate, plot data with ds%plot(), ds%var%mean())

**Dependencies:**

- fortio for data I/O

---

#### 5. fortframe

**Purpose:** Table-oriented data manipulation and analysis.

**Features:**

- pandas/excel-inspired interface using fortarray as backend
- SQL integration through fortsql for database operations
- Table joins, grouping, and aggregation operations
- Import/export from various tabular formats

**Interfaces:** Provides DataFrame-like API with SQL query capabilities and statistical methods.

**Inspired by:** pandas (as fortarray backend but with SQL and table oriented)

**Dependencies:**

- fortarray for underlying array operations
- fortsql for database connectivity

---

#### 6. fortfem

**Purpose:** Finite element method computations.

**Features:**

- FreeFEM-inspired finite element framework
- Mesh generation and adaptive refinement
- Variational formulation and solver integration
- Post-processing and visualization of FEM results

**Interfaces:** Offers high-level FEM API with mesh management and solver integration.

**Inspired by:** FreeFEM, FEniCS, scikit-fem

**Dependencies:** BLAS/LAPACK, sparse matrix libraries, mesh generation tools

---

#### Cthulhu Path of Madness

#### 1. fortos

**Purpose:** Bootstrap Fortran and an operating system from machine language.

**Features:**

- RISC-V architecture support with QEMU integration
- Kernel development framework in Fortran
- Hardware abstraction layer and device drivers
- System call interface and memory management

**Interfaces:** Provides OS development API with hardware abstraction and system service layers.

**Inspired by:** stage0 (bootstrap compiler from binary and hex code on bare metal)

**Dependencies:**

- RISC-V toolchain
- QEMU for emulation and testing
