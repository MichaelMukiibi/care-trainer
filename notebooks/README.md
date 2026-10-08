# CARE Trainer — Research Notebooks

This directory contains the **research notebooks for CARE Trainer**.

The notebooks are the experimental and analytical interface for the CARE project. They are intended to make research decisions, experiments, intermediate findings, and evaluation procedures inspectable and reproducible by other contributors.

> **Principle:** notebooks should document and orchestrate research; reusable software should live in `src/care_trainer/`.

---

## Purpose

CARE Trainer investigates the development and evaluation of language models for culturally aligned, responsible adolescent sexual and reproductive health (SRH) education in African contexts.

The notebooks provide a structured progression from:

```text
Environment
    ↓
Data
    ↓
Augmentation
    ↓
Baseline Models
    ↓
Training
    ↓
Alignment
    ↓
Evaluation
    ↓
Error Analysis
    ↓
Research Findings
```

Each stage should produce artifacts that can be inspected, reproduced, or consumed by subsequent stages.

---

# Notebook Structure

```text
notebooks/
│
├── README.md
│
├── 00_environment/
│   ├── 00_setup.ipynb
│   └── 01_environment_check.ipynb
│
├── 01_data/
│   ├── 01_dataset_overview.ipynb
│   ├── 02_data_exploration.ipynb
│   └── 03_data_validation.ipynb
│
├── 02_augmentation/
│   ├── 01_augmentation_experiments.ipynb
│   ├── 02_quality_analysis.ipynb
│   └── 03_augmented_dataset_validation.ipynb
│
├── 03_training/
│   ├── 01_baseline.ipynb
│   ├── 02_supervised_finetuning.ipynb
│   └── 03_training_comparison.ipynb
│
├── 04_alignment/
│   ├── 01_alignment_setup.ipynb
│   ├── 02_alignment_experiments.ipynb
│   └── 03_alignment_comparison.ipynb
│
├── 05_evaluation/
│   ├── 01_evaluation_setup.ipynb
│   ├── 02_health_accuracy.ipynb
│   ├── 03_cultural_alignment.ipynb
│   ├── 04_safety.ipynb
│   ├── 05_age_appropriateness.ipynb
│   └── 06_model_comparison.ipynb
│
└── 06_analysis/
    ├── 01_error_analysis.ipynb
    ├── 02_qualitative_analysis.ipynb
    └── 03_results.ipynb
```

The exact filenames may evolve. The numbered directories establish the intended research progression.

---

# 00 — Environment

### Purpose

Establish and verify the computational environment used for CARE experiments.

Typical responsibilities:

* verify Python and package versions
* verify PyTorch installation
* verify CUDA availability
* verify GPU configuration
* verify Hugging Face access
* verify experiment tracking configuration
* establish random seeds
* document hardware used for an experiment

Example:

```python
import torch

print("PyTorch:", torch.__version__)
print("CUDA available:", torch.cuda.is_available())

if torch.cuda.is_available():
    print("GPU:", torch.cuda.get_device_name(0))
```

Environment notebooks should help contributors identify configuration problems **before** running expensive experiments.

---

# 01 — Data

### Purpose

Understand, inspect, validate, and document datasets used by CARE.

Typical work includes:

* dataset loading
* dataset statistics
* schema inspection
* missing-value analysis
* duplicate detection
* class/distribution analysis
* language analysis
* demographic/contextual analysis where appropriate
* source and provenance checks
* train/validation/test splitting
* quality-control checks

Example workflow:

```text
Raw Dataset
    ↓
Schema Validation
    ↓
Quality Checks
    ↓
Deduplication
    ↓
Filtering
    ↓
Train / Validation / Test
    ↓
Versioned Dataset
```

### Important

Do **not** commit private, restricted, identifiable, or otherwise sensitive datasets to this repository.

A notebook may document how to obtain or process a dataset without embedding the dataset itself.

---

# 02 — Augmentation

### Purpose

Investigate methods for expanding or transforming training data while preserving factual, cultural, and safety properties.

Potential experiments include:

* paraphrasing
* contextual transformation
* question generation
* response generation
* synthetic examples
* African-context adaptation
* difficult/edge-case generation
* safety-oriented examples
* refusal examples
* counterexamples

Augmentation experiments must distinguish between:

```text
Original Data
     │
     ├── Human-authored
     ├── Existing public dataset
     └── Expert-reviewed
     
     ↓

Generated / Augmented Data

     ↓

Quality Control

     ↓

Approved Training Data
```

Synthetic data should never automatically be treated as ground truth.

Where feasible, notebooks should report:

* augmentation method
* model used for generation
* generation parameters
* number of generated examples
* filtering criteria
* validation procedure
* known limitations

---

# 03 — Training

### Purpose

Run and compare baseline and fine-tuning experiments.

Potential approaches include:

* pretrained model baselines
* supervised fine-tuning
* parameter-efficient fine-tuning
* LoRA/QLoRA
* instruction tuning
* controlled ablations

A training notebook should make the following explicit:

```text
Base Model
Dataset
Tokenizer
Training Configuration
Hardware
Random Seed
Training Method
Evaluation Dataset
Experiment ID
```

For example:

```python
experiment = {
    "base_model": "...",
    "dataset": "...",
    "method": "SFT",
    "seed": 42,
}
```

Large training jobs should eventually be moved into reusable scripts under:

```text
src/care_trainer/
scripts/
configs/
```

The notebook should then act as the experiment entry point rather than containing hundreds of lines of training infrastructure.

---

# 04 — Alignment

### Purpose

Investigate methods for making model behaviour more culturally aligned, responsible, and appropriate for the intended context.

Alignment experiments may investigate:

* culturally contextualized instruction data
* preference data
* safety behaviours
* refusal behaviours
* response style
* contextual grounding
* alignment objectives
* comparative alignment strategies

Alignment should not be evaluated solely through loss.

A successful alignment experiment should be examined through downstream behavioural evaluation.

---

# 05 — Evaluation

### Purpose

Evaluate models across the dimensions relevant to CARE.

Evaluation should be modular rather than represented by a single score.

Potential dimensions include:

### Health Accuracy

Does the model provide medically responsible information?

### Cultural Alignment

Does the model appropriately respond within the relevant cultural context?

### Safety

Does the model avoid harmful or inappropriate responses?

### Age Appropriateness

Is the language and information appropriate for the intended adolescent audience?

### Helpfulness

Does the model meaningfully answer the user's question?

### Robustness

Does the behaviour remain appropriate when prompts are rephrased or intentionally stress-tested?

---

## Evaluation Matrix

Where possible, experiments should produce a matrix such as:

| Model    | Health | Cultural | Safety | Age | Helpfulness | Robustness |
| -------- | -----: | -------: | -----: | --: | ----------: | ---------: |
| Baseline |      — |        — |      — |   — |           — |          — |
| SFT      |      — |        — |      — |   — |           — |          — |
| Aligned  |      — |        — |      — |   — |           — |          — |

The precise metrics and scoring methodology should be defined by the CARE research team rather than assumed by the training code.

---

# 06 — Analysis

### Purpose

Turn experiment outputs into interpretable research findings.

This directory is for:

* error analysis
* qualitative response analysis
* model comparisons
* visualization
* ablation analysis
* failure-mode analysis
* research figures
* final experiment summaries

A useful analysis notebook should answer questions such as:

> What changed?

> Why did it change?

> Did the change improve the intended behaviour?

> What new failure modes appeared?

> Does the result generalize beyond the evaluation set?

---

# Notebook Naming Convention

Use numbered filenames so that the intended execution/research order is obvious.

Recommended:

```text
01_dataset_overview.ipynb
02_data_exploration.ipynb
03_data_validation.ipynb
```

Avoid:

```text
final.ipynb
new.ipynb
test2.ipynb
michael_experiment.ipynb
latest_final_v3.ipynb
```

For experiments, prefer descriptive names:

```text
03_sft_qwen_baseline.ipynb
04_lora_ablation.ipynb
05_cultural_alignment_comparison.ipynb
```

If an experiment becomes important enough to reproduce repeatedly, migrate its implementation into `src/` or `scripts/` and keep the notebook as the research record.

---

# Notebook Standards

Every substantive notebook should begin with a short metadata section.

Example:

```markdown
# Experiment: <name>

**Purpose:** <what this experiment investigates>

**Research question:** <question>

**Base model:** <model>

**Dataset:** <dataset/version>

**Method:** <method>

**Seed:** <seed>

**Hardware:** <hardware>

**Experiment ID:** <ID>

**Status:** exploratory / baseline / final
```

---

# Reproducibility

A notebook should ideally be runnable from a clean environment.

Avoid relying on:

* variables created in another notebook
* hidden local files
* undocumented paths
* manually modified datasets
* credentials embedded in cells
* local-only model checkpoints

Prefer:

```python
from care_trainer.data import ...
from care_trainer.evaluation import ...
```

over copying large implementations between notebooks.

Configuration should eventually be centralized under:

```text
configs/
```

and reusable functionality under:

```text
src/care_trainer/
```

---

# Paths

Do not hard-code machine-specific paths such as:

```python
"/home/michael/Desktop/care/"
```

or:

```python
"C:/Users/Michael/Downloads/model/"
```

Prefer repository-relative paths or configuration variables.

For example:

```python
from pathlib import Path

ROOT = Path.cwd().resolve().parents[1]
DATA_DIR = ROOT / "data"
```

For larger experiments, use configuration files.

---

# Compute

Notebooks may be run locally, on university infrastructure, or on cloud GPU infrastructure.

Do not assume a particular GPU is available.

Always record:

* GPU type
* VRAM
* precision
* batch size
* gradient accumulation
* sequence length
* training steps/epochs
* estimated training time

For expensive experiments, the notebook should make it clear how to reproduce the run without accidentally launching a large job.

---

# Outputs

Generated artifacts should not normally be committed directly to Git.

Examples:

```text
model checkpoints
large datasets
training logs
W&B runs
TensorBoard logs
generated datasets
large figures
temporary evaluation outputs
```

These belong in appropriate external storage or experiment-tracking systems.

The notebook should contain the **code and analysis required to reproduce or interpret them**.

---

# Public Repository Rules

Because `care-trainer` is public:

### Never commit

* API keys
* Hugging Face tokens
* AWS credentials
* passwords
* private datasets
* identifiable health information
* confidential research material
* private model weights
* restricted third-party datasets

Use environment variables or secret-management systems instead.

For example:

```python
import os

HF_TOKEN = os.environ.get("HF_TOKEN")
```

Never:

```python
HF_TOKEN = "hf_xxxxxxxxxxxxxxxxx"
```

---

# Research Integrity

CARE operates in a sensitive health-education domain.

Contributors should distinguish clearly between:

* experimental observations
* model-generated content
* expert-reviewed content
* medically validated information
* hypotheses
* established findings

Model outputs should not be presented as medical truth merely because a model generated them.

Where health-related evaluation is performed, the methodology and source of ground truth should be documented.

---

# Relationship to `src/`

The intended division is:

```text
notebooks/
    Research orchestration
    Exploration
    Visualization
    Experiment documentation
    Qualitative analysis

src/care_trainer/
    Reusable Python code
    Dataset utilities
    Training utilities
    Alignment implementations
    Evaluation implementations

scripts/
    Command-line experiment entry points

configs/
    Experiment configuration
```

A good rule is:

> **If you copy the same code into a second notebook, it probably belongs in `src/`.**

---

# Recommended Contributor Workflow

Before creating a new notebook:

1. Identify the research question.
2. Determine which stage of the CARE pipeline it belongs to.
3. Check whether reusable functionality already exists under `src/`.
4. Create the notebook using the appropriate numbered directory.
5. Record the model, dataset, configuration, seed, and hardware.
6. Run the experiment.
7. Record observations and limitations.
8. Move reusable code into `src/` where appropriate.
9. Keep large artifacts outside Git.
10. Commit the notebook with a descriptive message.

Example:

```bash
git add notebooks/05_evaluation/03_cultural_alignment_comparison.ipynb
git commit -m "eval: compare cultural alignment across models"
```

---

# Notebook Philosophy

The notebooks are not merely scratch space.

They form part of the **research record of CARE**.

A future contributor should be able to open a notebook and understand:

```text
What question were we asking?
        ↓
What data did we use?
        ↓
What model did we use?
        ↓
What did we change?
        ↓
What did we measure?
        ↓
What happened?
        ↓
What did we learn?
        ↓
What should we investigate next?
```

That is the standard we aim for throughout the CARE Trainer research workflow.
