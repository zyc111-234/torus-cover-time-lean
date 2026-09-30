# Cover-Time Limit on the Two-Dimensional Torus

A Lean 4 formalization of the scalar cover-time limit for continuous-time simple random walk on the two-dimensional discrete torus.

## The theorem

Let $T_N$ be the cover time of total-rate-one simple random walk on $(\mathbb Z/N\mathbb Z)^2$. Set

```math
b_N=\frac{2}{\pi}N^2\log N,
\qquad
a_N=b_N(2\log N-\log\log N).
```

There exist a critical Gaussian multiplicative chaos measure $Z$ on the unit torus and a deterministic constant $\kappa>0$ such that

```math
\sup_{x\in(\mathbb Z/N\mathbb Z)^2}
\sup_{z\in\mathbb R}
\left|
\mathbb P_x\!\left(\frac{T_N-a_N}{b_N}\le z\right)
-\mathbb E\!\left[e^{-\kappa Z(\mathbb T^2)e^{-z}}\right]
\right|
\longrightarrow 0.
```

The limiting law is a randomly shifted Gumbel distribution. The measure $Z$ is constructed from the mean-zero periodic Gaussian free field with covariance $4(-\Delta)^{-1}$, using the heat regularization specified in the formal definitions.

The formalized result concerns the scalar cover-time distribution. The state space is discrete; time is continuous, with independent exponential holding times of mean one.

## Formal statement

The public entry point is `Scalar.lean`. The main theorem, `TorusCoverTime.scalar_cover_time_limit`, has the following type:

```lean
∃ Z : FieldSample → TorusMeasure, CriticalChaosLimit Z ∧
  ∃ κ : ℝ, 0 < κ ∧ ScalarCoverageLimit Z κ
```

Here, the names are in the `TorusCoverTime` namespace.

## Verification

Lean and mathlib versions are pinned in the project configuration. With elan installed, run these commands in the extracted source directory:

```sh
lake exe cache get
lake exe cache clean!
lake build Scalar
lake env lean scripts/CheckContracts.lean
lake env lean scripts/CheckAxioms.lean
```

The final audit checks the theorem's proof dependencies and permits only Lean's standard axioms: `propext`, `Classical.choice`, and `Quot.sound`.

The accompanying GitHub Actions workflow builds the theorem and runs both checks. Its result applies to the source archive at the corresponding commit.

The archive contains the Lean sources, pinned dependency configuration, this README, and verification scripts. Local caches, development records, and manuscript files are excluded.

## Source layout

`Scalar.lean` remains the public entry point. Directory names organize the
source files; theorem names and namespaces are unchanged.

```text
Solutions/
├── Main/                 # Assembly of the scalar theorem
├── Core/                 # Shared top-level lemmas
├── Blocks/
│   ├── A/
│   ├── B/
│   ├── C/
│   │   ├── Base/
│   │   └── C4/
│   │       ├── Base/
│   │       ├── M2/
│   │       ├── M3/
│   │       ├── M4/
│   │       └── M5/
│   ├── D/
│   │   ├── Base/
│   │   ├── Preparation/
│   │   ├── R23/
│   │   ├── R24/
│   │   └── R25/
│   │       ├── Base/
│   │       └── S09/
│   ├── E/
│   ├── F/
│   └── M/
└── CoverTime/
    ├── Interfaces/
    ├── Estimates/
    └── Terminal/
```

`Base/` holds the files previously placed directly beside a block's subdirectories;
it does not assert a new mathematical dependency hierarchy.

The main proof is assembled in `Solutions/Main/ScalarAssembly.lean`, using
`Solutions/Blocks/C/Base/CriticalChaosExistence.lean` and
`Solutions/Blocks/D/R25/Base/N5.lean`. The latter reaches the scalar distribution
limit through `Solutions/CoverTime/Terminal/TerminalCDF.lean`.
