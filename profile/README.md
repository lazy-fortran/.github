## The Vision of *lazy fortran*
**Make Python Fortran again!**

**Disclaimer: This is an experimental work-in-progress programme that is only partially implemented and where interfaces at all levels could undergo substantial changes.**

*lazy fortran* solves the two-language problem in Fortran by providing the experience and tooling of modern languages while maintaining, performance and long-term stability of Fortran. It is inspired by the best features of Python, Rust, Julia and Go, builds on the `fpm` ecosystem, and integrates with standard Fortran compilers and tools.

### Toolchain Overview

The *lazy fortran* toolchain is being aligned with [LFortran](https://lfortran.org/). Core compiler functionality (type inference, formatting) will be contributed to LFortran's `--infer` and `--fmt` modes, while *lazy fortran* focuses on higher-level tooling built on top.

---

#### fortrun *(on hold)*

**Status:** On hold pending alignment with LFortran tooling.

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

#### fluff *(retired)*

**Status:** Retired in favor of [LFortran](https://lfortran.org/) `fmt` mode. LFortran provides a robust, trivia-preserving AST that enables proper source formatting while maintaining comments and whitespace.

**Purpose:** Source-to-source transformations and static checks.

**Inspired by:** ruff (code formatting and static analysis)

---

#### fortnb *(on hold)*

**Status:** On hold pending alignment with LFortran tooling.

**Purpose:** Notebook interface for Standard Fortran and *lazy fortran*.

**Features:**

- Implements .f and .md files as notebooks similar to jupytext
- Supports both Standard Fortran and *lazy fortran* syntax
- Provides export to Markdown and PDF

**Interfaces:** Offers API and CLI to convert notebooks to Markdown or PDF

**Inspired by:** jupyter, jupytext, weave, blocks (caching runner), lfortran

**Dependencies:**

- fortrun for running notebook cells and managing cache

---

#### fortfc *(retired)*

**Status:** Retired in favor of [LFortran](https://lfortran.org/) compiler. LFortran already provides a complete compilation pipeline from Fortran source to LLVM IR with excellent architecture.

**Purpose:** Full compilation backend.

**Inspired by:** flang, lfortran

---

#### fortad *(planned)*

**Status:** Planned for future development.

**Purpose:** Automatic differentiation.

**Features:**

- Supports `!$ad` annotations to mark functions/subroutines for differentiation
- Integrates with Enzyme at the IR level during compilation
- Works with any Enzyme-enabled compiler (Flang, LFortran, etc.)

**Inspired by:** tapenade (decorators, e.g. !$ad mode=reverse)

---

#### fortfront

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


### Packages for Scientific Computing

---

#### fortarray *(planned)*

**Status:** Planned for future development.

**Purpose:** Multi-dimensional array operations and data analysis.

**Features:**

- xarray/ndarray-inspired interface: load, aggregate, plot data with ds%plot(), ds%var%mean()
- Labeled arrays with coordinate indexing and alignment
- Broadcasting, reduction, and transformation operations
- Integration with fortio for data loading

**Inspired by:** xarray/ndarray

---

#### fortframe *(planned)*

**Status:** Planned for future development.

**Purpose:** Table-oriented data manipulation and analysis.

**Features:**

- pandas/excel-inspired interface using fortarray as backend
- SQL integration through fortsql for database operations
- Table joins, grouping, and aggregation operations

**Inspired by:** pandas

---

#### fortfem

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

#### fortio *(planned)*

**Status:** Planned for future development.

**Purpose:** Unified I/O interface for scientific data formats.

**Features:**

- Low-level I/O routines for NetCDF/HDF5, parquet, csv formats under one API
- Memory-efficient streaming and chunked data access
- Format-agnostic data type mapping and conversion

**Inspired by:** NetCDF/HDF5/parquet unified I/O

---

#### fortsql *(planned)*

**Status:** Planned for future development.

**Purpose:** Database connectivity and SQL operations.

**Features:**

- Low-level SQL API for various databases (SQLite, PostgreSQL, MySQL)
- Connection pooling and transaction management
- Prepared statements and parameter binding

**Inspired by:** Low-level SQL APIs

---

#### fortplot

**Purpose:** Scientific plotting and visualization.

**Features:**

- 2D/3D plotting with customizable styles and layouts
- Statistical plots, histograms, and scientific visualizations
- Export to various formats (PNG, PDF, SVG, EPS)

**Interfaces:** Provides high-level plotting API with figure and axis management.

**Inspired by:** matplotlib, pyplot-fortran, gnuplot, plotly

**Dependencies:** Graphics libraries (Cairo, FreeType), optional LaTeX for typesetting

---

#### Cthulhu Path of Madness

#### fortos *(planned)*

**Status:** Planned for future development (experimental).

**Purpose:** Bootstrap Fortran and an operating system from machine language.

**Features:**

- RISC-V architecture support with QEMU integration
- Kernel development framework in Fortran
- Hardware abstraction layer and device drivers

**Inspired by:** stage0 (bootstrap compiler from binary and hex code on bare metal)
