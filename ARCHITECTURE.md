# CARE Trainer Architecture

**Status:** Research / Active Development
**Repository:** `care-trainer`

---

## 1. Overview

`care-trainer` is the model-development and experimentation repository for the **CARE Framework**:

> **Culturally Aligned Responsible Education for adolescent Sexual and Reproductive Health in African contexts.**

The repository is designed to support the complete research lifecycle of CARE models:

```text
Data
  │
  ▼
Validation & Curation
  │
  ▼
Augmentation
  │
  ▼
Model Preparation
  │
  ▼
Training / Fine-Tuning
  │
  ▼
Alignment
  │
  ▼
Evaluation
  │
  ▼
Error Analysis
  │
  ▼
Research Findings
```

The architecture separates **research experimentation** from **reusable ML infrastructure**.

---

# 2. Design Goals

The architecture is guided by six goals.

### 2.1 Reproducibility

A contributor should be able to identify exactly:

* which model was used
* which dataset was used
* which configuration was used
* which training method was used
* which evaluation protocol was used
* which hardware was used
* which experiment produced a result

### 2.2 Modularity

Data processing, augmentation, training, alignment, and evaluation should be independently replaceable.

### 2.3 Experimentability

Researchers should be able to rapidly test hypotheses without modifying the entire codebase.

### 2.4 Traceability

Training and evaluation results should be traceable to their:

```text
dataset → configuration → model → experiment → result
```

### 2.5 Safety

The architecture must prevent accidental publication of sensitive data, credentials, or restricted research artifacts.

### 2.6 Extensibility

The system should support multiple:

* datasets
* model families
* training methods
* alignment methods
* evaluation protocols
* compute environments

without requiring a redesign of the repository.

---

# 3. High-Level Architecture

```text
                         CARE TRAINER
                              │
             ┌────────────────┴────────────────┐
             │                                 │
          Research                         Infrastructure
             │                                 │
      ┌──────┴──────┐                 ┌────────┴────────┐
      │             │                 │                 │
  Notebooks      Reports           src/              configs/
      │             │                 │                 │
      │             │       ┌─────────┼─────────┐       │
      │             │       │         │         │       │
      ▼             ▼       ▼         ▼         ▼       ▼
   Analysis      Results   Data   Training   Evaluation
                            │         │         │
                            └─────────┼─────────┘
                                      │
                                      ▼
                               Model Experiments
                                      │
                                      ▼
                                  Evaluation
```

The architecture has five primary layers:

1. **Data Layer**
2. **Model & Training Layer**
3. **Alignment Layer**
4. **Evaluation Layer**
5. **Research / Experimentation Layer**

---

# 4. Repository Architecture

```text
care-trainer/
│
├── README.md
├── ARCHITECTURE.md
├── LICENSE
├── CITATION.cff
├── CONTRIBUTING.md
├── pyproject.toml
│
├── configs/
│   ├── data/
│   ├── models/
│   ├── training/
│   └── evaluation/
│
├── data/
│   ├── README.md
│   ├── raw/
│   ├── interim/
│   ├── processed/
│   └── evaluation/
│
├── notebooks/
│   ├── README.md
│   ├── 00_environment/
│   ├── 01_data/
│   ├── 02_augmentation/
│   ├── 03_training/
│   ├── 04_alignment/
│   ├── 05_evaluation/
│   └── 06_analysis/
│
├── src/
│   └── care_trainer/
│       ├── __init__.py
│       ├── data/
│       ├── augmentation/
│       ├── training/
│       ├── alignment/
│       ├── evaluation/
│       └── utils/
│
├── scripts/
│   ├── prepare_data.py
│   ├── train.py
│   ├── evaluate.py
│   └── reproduce.py
│
├── tests/
│
└── reports/
    ├── experiments/
    └── figures/
```

---

# 5. Data Layer

The data layer manages datasets throughout their lifecycle.

```text
External Sources
      │
      ▼
   raw/
      │
      ▼
   interim/
      │
      ▼
 processed/
      │
      ├───────────────┐
      ▼               ▼
 training data    evaluation data
```

## `data/raw/`

Original datasets or locally acquired source material.

This directory should generally remain empty in the public repository.

Raw data may be:

* private
* restricted
* too large for Git
* subject to third-party licensing

---

## `data/interim/`

Temporary datasets produced during processing.

Examples:

* filtered datasets
* deduplicated datasets
* intermediate transformations
* augmentation candidates

These are reproducible artifacts rather than authoritative datasets.

---

## `data/processed/`

Datasets prepared for model training.

Examples:

```text
instruction datasets
conversation datasets
preference datasets
SFT datasets
alignment datasets
```

Processed datasets should have documented provenance.

---

## `data/evaluation/`

Evaluation-specific datasets.

Evaluation data should remain logically separate from training data to reduce the risk of contamination.

Where possible:

```text
Training Data ∩ Evaluation Data ≈ ∅
```

---

# 6. Data Provenance

Every important dataset should have identifiable provenance.

A dataset configuration should ideally specify:

```yaml
name:
version:
source:
license:
language:
context:
preprocessing:
augmentation:
intended_use:
restrictions:
```

Dataset transformations should be deterministic where practical.

The objective is to make this relationship traceable:

```text
Source Dataset
      │
      ▼
Transformation
      │
      ▼
Dataset Version
      │
      ▼
Training Experiment
```

---

# 7. Model Layer

The model layer abstracts the underlying language model from the training pipeline.

The architecture should not assume a single model family.

Conceptually:

```text
                   Model Interface
                         │
          ┌──────────────┼──────────────┐
          ▼              ▼              ▼
      Model A         Model B        Model C
```

Potential model sources include Hugging Face-compatible pretrained models.

A model configuration should identify:

```yaml
model_name:
revision:
tokenizer:
context_length:
precision:
quantization:
```

Model weights should not normally be committed to Git.

---

# 8. Training Layer

Training code belongs under:

```text
src/care_trainer/training/
```

The training layer should provide reusable abstractions for:

* model loading
* tokenizer initialization
* dataset loading
* preprocessing
* batching
* training
* checkpointing
* logging
* evaluation hooks

Conceptually:

```text
Configuration
      │
      ▼
Dataset ───────► Preprocessor
                      │
                      ▼
Model ◄──────── Tokenizer
  │
  ▼
Trainer
  │
  ├── Logging
  ├── Checkpoints
  └── Evaluation
```

---

# 9. Training Strategies

The architecture should permit multiple training strategies.

Examples include:

```text
Baseline
   │
   ├── Zero-shot
   │
   └── Few-shot

Fine-Tuning
   │
   ├── SFT
   ├── LoRA
   └── QLoRA

Alignment
   │
   ├── Preference-based methods
   ├── Instruction alignment
   └── Other experimental approaches
```

Specific methods should be implemented as modular components rather than embedded directly inside individual notebooks.

---

# 10. Alignment Layer

Alignment is treated as a separate research layer because the objective is not merely to reduce training loss.

The alignment layer investigates whether model behaviour better satisfies CARE's intended properties.

```text
Base / Fine-Tuned Model
          │
          ▼
    Alignment Method
          │
          ▼
    Aligned Model
          │
          ▼
      Evaluation
```

Alignment experiments may incorporate:

* culturally contextualized data
* preference data
* safety examples
* refusal examples
* response-style constraints
* domain-specific alignment

Alignment should always be evaluated against appropriate baselines.

---

# 11. Evaluation Layer

Evaluation is a first-class component of the architecture.

It should not be implemented solely as a collection of ad-hoc notebook cells.

```text
                 Evaluation
                     │
      ┌──────────────┼──────────────┐
      ▼              ▼              ▼
 Quantitative    Qualitative      Safety
      │              │              │
      └──────────────┼──────────────┘
                     ▼
              Comparative Analysis
```

Potential evaluation dimensions:

* health accuracy
* cultural alignment
* safety
* age appropriateness
* helpfulness
* robustness
* refusal behaviour
* consistency

The exact metrics are an active research concern and should be versioned alongside evaluation protocols.

---

# 12. Evaluation as a Contract

Every model evaluation should identify:

```text
Model
Dataset
Evaluation Version
Metric
Scoring Method
Seed
Prompt Template
Evaluator
```

A result without its evaluation context should not be treated as a reproducible research result.

---

# 13. Experiment Configuration

Configurations belong under:

```text
configs/
```

Configuration should be separated by concern:

```text
configs/
├── data/
├── models/
├── training/
└── evaluation/
```

For example:

```yaml
# configs/training/example.yaml

seed: 42

training:
  epochs: 3
  learning_rate: 2.0e-5
  batch_size: 4
  gradient_accumulation_steps: 8

precision:
  bf16: true

logging:
  project: care-trainer
```

The exact configuration system may evolve.

---

# 14. Experiment Identity

Experiments should have unique identifiers.

Conceptually:

```text
CARE-<stage>-<model>-<method>-<version>
```

Example:

```text
CARE-SFT-QWEN-LORA-001
```

An experiment should allow a contributor to reconstruct:

```text
Experiment ID
      │
      ├── Model
      ├── Dataset
      ├── Configuration
      ├── Code Version
      ├── Hardware
      └── Results
```

Git commit hashes should be recorded for important experiments.

---

# 15. Notebook Layer

Notebooks are the primary research interface.

They belong under:

```text
notebooks/
```

The notebook layer should:

* explore
* visualize
* prototype
* compare
* document
* analyze

It should **not become the primary home for reusable infrastructure**.

The intended relationship is:

```text
Notebook
   │
   ├── imports
   │
   ▼
src/care_trainer/
   │
   ├── data
   ├── augmentation
   ├── training
   ├── alignment
   └── evaluation
```

See [`notebooks/README.md`](notebooks/README.md) for contributor guidance.

---

# 16. Scripts Layer

The `scripts/` directory provides command-line entry points for repeatable workflows.

Examples:

```bash
python scripts/prepare_data.py
python scripts/train.py
python scripts/evaluate.py
python scripts/reproduce.py
```

Scripts should call reusable functionality from `src/`.

They should avoid becoming independent implementations of the same logic.

---

# 17. Source Package

The Python package lives under:

```text
src/care_trainer/
```

This is the **core software layer** of the repository.

```text
src/care_trainer/
│
├── data/
│   └── Dataset processing
│
├── augmentation/
│   └── Data generation and transformation
│
├── training/
│   └── Training infrastructure
│
├── alignment/
│   └── Alignment methods
│
├── evaluation/
│   └── Evaluation infrastructure
│
└── utils/
    └── Shared utilities
```

Code placed here should be:

* reusable
* testable
* documented
* independent of notebook state

---

# 18. Testing

Tests belong under:

```text
tests/
```

Testing should cover critical reusable components.

Examples:

```text
Dataset preprocessing
Tokenizer handling
Data transformations
Configuration loading
Evaluation metrics
Prompt formatting
Model wrappers
Utility functions
```

Training a full model should generally not be required for ordinary unit tests.

Where appropriate, use:

* small fixtures
* mock models
* tiny datasets
* deterministic test inputs

---

# 19. Experiment Outputs

Generated outputs should generally live outside Git.

Examples:

```text
checkpoints/
outputs/
runs/
wandb/
mlruns/
```

These may be stored using:

* experiment tracking platforms
* cloud object storage
* model registries
* Hugging Face Hub
* other controlled research infrastructure

The repository should contain the configuration and code required to reproduce the output, not necessarily the output itself.

---

# 20. Compute Architecture

CARE experiments may execute across different environments.

```text
Developer Machine
       │
       ├──────────────┐
       ▼              ▼
   Local GPU       Cloud GPU
                       │
                       ▼
                Experiment Run
                       │
             ┌─────────┴─────────┐
             ▼                   ▼
        Experiment Log       Artifacts
```

The training code should avoid hard dependencies on a specific machine.

Hardware-specific optimizations should be configurable.

Important hardware information should be recorded for significant experiments.

---

# 21. Hugging Face Integration

CARE Trainer is designed to work naturally with the Hugging Face ecosystem.

Potential integrations include:

```text
Hugging Face Datasets
        │
        ▼
   Data Pipeline
        │
        ▼
Hugging Face Transformers
        │
        ▼
Training / Alignment
        │
        ▼
Evaluation
        │
        ▼
Hugging Face Hub
```

The exact use of the Hugging Face Hub depends on the model and dataset license.

Public release must never be assumed simply because a model can technically be uploaded.

---

# 22. Experiment Tracking

Experiment tracking should record sufficient metadata to compare runs.

At minimum:

```text
experiment_id
git_commit
model
dataset
dataset_version
training_method
configuration
seed
hardware
metrics
```

W&B, MLflow, or another experiment-tracking system may be used.

The tracking backend should remain replaceable.

---

# 23. Security and Privacy Boundary

The repository is public.

Therefore:

```text
PUBLIC
│
├── Source code
├── Documentation
├── Research notebooks
├── Public configurations
├── Reproducible methodology
└── Approved public datasets/references
```

must remain separate from:

```text
PRIVATE
│
├── Credentials
├── Private datasets
├── Sensitive health information
├── Restricted research material
├── Private checkpoints
└── Confidential evaluation data
```

Sensitive resources should be accessed through controlled infrastructure rather than committed to Git.

---

# 24. Data Leakage and Evaluation Contamination

CARE evaluation must explicitly consider contamination.

Potential leakage sources include:

* evaluation examples appearing in training data
* augmented versions of evaluation examples
* benchmark examples copied into instruction datasets
* model-generated evaluation data being reused during training
* manually inspecting evaluation data and subsequently incorporating it into training

The data pipeline should make training and evaluation provenance traceable.

---

# 25. Reproducibility Contract

A significant experiment should be reproducible from:

```text
Git Commit
+
Configuration
+
Dataset Version
+
Model Revision
+
Environment
+
Random Seed
```

Conceptually:

```text
                     Reproduction
                          │
       ┌──────────────────┼──────────────────┐
       ▼                  ▼                  ▼
   Code Version      Data Version       Model Version
       │                  │                  │
       └──────────────────┼──────────────────┘
                          ▼
                    Configuration
                          │
                          ▼
                    Experiment Run
                          │
                          ▼
                       Result
```

---

# 26. Research-to-Production Boundary

`care-trainer` is primarily a **research repository**.

Training and evaluation code should not automatically be interpreted as production-ready inference infrastructure.

A future deployment system may consume:

```text
CARE Model
     ▲
     │
care-trainer
```

but deployment concerns should remain separate until the research results justify them.

In particular, production deployment should introduce additional:

* safety controls
* monitoring
* privacy controls
* access controls
* model governance
* clinical/domain review
* incident handling

---

# 27. Contributor Decision Tree

When adding code, use the following rule:

```text
Is it reusable Python logic?
        │
       YES
        ▼
   src/care_trainer/

Is it a repeatable command-line workflow?
        │
       YES
        ▼
      scripts/

Is it an experiment or exploration?
        │
       YES
        ▼
    notebooks/

Is it configuration?
        │
       YES
        ▼
     configs/

Is it a test?
        │
       YES
        ▼
      tests/

Is it generated output?
        │
       YES
        ▼
External artifact storage
```

---

# 28. Architecture Evolution

This architecture is intentionally designed for an active research project.

The following components may change as CARE develops:

* model families
* training methods
* alignment methods
* evaluation metrics
* dataset schemas
* experiment tracking
* cloud infrastructure
* model registry
* inference architecture

Changes should preserve the core separation between:

```text
Research
   ↕
Reusable Infrastructure
   ↕
Data
   ↕
Experiments
   ↕
Evaluation
```

Major architectural changes should be documented in Git history and, where appropriate, in this document.

---

# 29. Core Principle

The most important architectural rule for CARE Trainer is:

> **Experiments should be easy to run, but important results should be difficult to reproduce accidentally.**

A contributor should always be able to determine **what was trained, on what data, using which method, under which configuration, and according to which evaluation protocol**.

That traceability is essential for responsible research in culturally sensitive and health-related AI systems.
