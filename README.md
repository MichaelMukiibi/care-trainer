# CARE Trainer

**Training and experimentation framework for the CARE project — Culturally Aligned Responsible Education for adolescent Sexual and Reproductive Health (SRH) in African contexts.**

[![Status](https://img.shields.io/badge/status-research-orange)](#status)
[![License](https://img.shields.io/badge/license-see%20LICENSE-blue)](#license)
[![Hugging%20Face](https://img.shields.io/badge/🤗-Hugging%20Face-yellow)](https://huggingface.co/)

> **CARE** explores how large language models can provide adolescent SRH education that is medically responsible, culturally aligned, age-appropriate, and useful within African contexts.

`care-trainer` contains the training, fine-tuning, data-processing, evaluation, and experimentation workflows used to develop CARE models.

---

## Project Overview

Large language models can provide accessible health information, but generic models may not adequately account for:

* African cultural and social contexts
* adolescent-specific needs
* local health terminology and communication patterns
* culturally sensitive topics
* medical safety and factuality
* age-appropriate communication
* harmful stereotypes and cultural assumptions
* appropriate refusal and escalation behaviour

CARE investigates these challenges through a multidisciplinary approach combining **machine learning, alignment research, social and cultural analysis, and health sciences**.

This repository focuses specifically on the **model development and experimentation pipeline**.

---

## What This Repository Does

`care-trainer` is intended to support:

* dataset preparation and validation
* data augmentation
* supervised fine-tuning
* alignment experiments
* parameter-efficient fine-tuning
* model evaluation
* safety and refusal evaluation
* cultural-alignment evaluation
* reproducible Hugging Face experiments
* experiment tracking and configuration
* model and tokenizer preparation
* research notebooks
* reproducible training pipelines

The repository is designed so that individual experiments can be reproduced, compared, and extended without coupling the research to a single model family.

---

## CARE Research Pipeline

The repository follows a research pipeline approximately structured as:

```text
Research Questions
       │
       ▼
Data Collection / Curation
       │
       ▼
Data Validation
       │
       ▼
Data Augmentation
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
       ├── Medical / Health Accuracy
       ├── Cultural Alignment
       ├── Age Appropriateness
       ├── Safety
       ├── Helpfulness
       └── Robustness
       │
       ▼
Analysis
       │
       ▼
Model / Experiment Report
```

The exact training strategy is an active research question and may evolve as experiments are conducted.

---

## Team

CARE is a multidisciplinary project involving contributors from computing, social sciences, and health sciences.

### Technical Research Team

| Contributor                  | Focus                                                         |
| ---------------------------- | ------------------------------------------------------------- |
| **Michael Kirinnya Mukiibi** | Alignment Training & Research Lead                            |
| **Elton Felix Mubiru**       | Data & Augmentation Engineering                               |
| **Patricia Keitetsi**        | Social/Cultural Alignment & Human-Centred Research            |
| **Scovia Katusiime**         | LLM/ML Operations & Reproducible Hugging Face Experimentation |

### Health Sciences Contributors

| Contributor          | Focus                                     |
| -------------------- | ----------------------------------------- |
| **Luttamaguzi Mark** | Health Sciences / SRH domain contribution |
| **Bayiga Leah**      | Health Sciences / SRH domain contribution |

The multidisciplinary structure is intentional: CARE is not solely a model-training problem. Responsible SRH education requires technical, cultural, social, and health-domain perspectives.

---

## Repository Structure

The repository is organized around reproducible research rather than a single training script.

```text
care-trainer/
│
├── README.md
├── LICENSE
├── CITATION.cff
├── CONTRIBUTING.md
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

> The repository structure is intentionally modular. Individual directories may be introduced incrementally as the research matures.

---

## Notebooks

Notebooks will serve as the **research interface** for CARE experiments.

They are intended for:

* exploratory data analysis
* dataset inspection
* augmentation experiments
* baseline experiments
* fine-tuning experiments
* alignment experiments
* evaluation
* visualization
* qualitative analysis
* experiment reporting

Production/reusable logic should eventually live under `src/`, while notebooks should primarily orchestrate experiments and document research decisions.

A planned notebook sequence is:

```text
00 — Environment & Reproducibility
01 — Dataset Exploration
02 — Data Preparation
03 — Data Augmentation
04 — Baseline Models
05 — Fine-Tuning
06 — Alignment Experiments
07 — Safety & Cultural Evaluation
08 — Comparative Evaluation
09 — Error Analysis
10 — Results & Visualization
```

The notebook plan will evolve alongside the CARE research methodology.

---

## Data

This repository may contain code and **non-sensitive research datasets**, but it should not be used to publish private, identifiable, confidential, or otherwise restricted health information.

Sensitive datasets should remain outside the public repository and be accessed through an appropriate controlled workflow.

Publicly redistributable datasets should retain their original licenses and attribution requirements.

Dataset documentation should record, where appropriate:

* source
* license
* provenance
* intended use
* preprocessing
* augmentation
* known limitations
* demographic/contextual considerations

---

## Models

CARE experiments may use publicly available pretrained language models and Hugging Face-compatible tooling.

Model selection is intentionally separated from the training framework so that different model families can be evaluated under comparable experimental conditions.

Models produced through this repository should **not automatically be interpreted as clinically validated systems**.

A research checkpoint is not a medical device, diagnostic system, or substitute for professional healthcare.

---

## Evaluation

CARE evaluation should go beyond conventional language-model metrics.

Experiments may evaluate dimensions including:

### Health Accuracy

Does the model provide medically responsible and factually appropriate information?

### Cultural Alignment

Does the response appropriately account for the cultural and social context in which it is being used?

### Age Appropriateness

Is the response suitable for the intended adolescent audience?

### Safety

Does the model avoid harmful, misleading, unsafe, or inappropriate responses?

### Helpfulness

Does the model actually answer the user's question clearly and constructively?

### Robustness

Does model behaviour remain appropriate under variations in phrasing, ambiguity, adversarial prompts, and culturally sensitive scenarios?

Evaluation criteria and datasets should be documented alongside experiments rather than treated as hidden implementation details.

---

## Reproducibility

Experiments should record, where applicable:

* base model
* model revision
* tokenizer
* dataset version
* dataset configuration
* preprocessing configuration
* augmentation strategy
* training configuration
* random seed
* hardware
* software dependencies
* evaluation configuration
* experiment identifier

Where possible, experiments should be reproducible from configuration files and scripts rather than relying exclusively on notebook state.

---

## Research Principles

CARE follows several principles:

**Multidisciplinary**
Technical decisions should be informed by social, cultural, and health-science perspectives.

**Culturally grounded**
African contexts should not be treated as a single homogeneous culture.

**Safety-conscious**
The system should prioritize responsible health communication over maximizing answer completion.

**Evidence-oriented**
Training and alignment decisions should be supported by measurable evaluation.

**Reproducible**
Research workflows should be documented and repeatable.

**Transparent**
Important datasets, assumptions, limitations, and experimental decisions should be documented whenever disclosure is appropriate.

---

## Status

🚧 **Active research project**

`care-trainer` is under active development. APIs, datasets, training methods, evaluation protocols, and repository structure may change as the CARE research progresses.

Results generated from early experiments should be considered **research results**, not validated clinical evidence.

---

## Contributing

Contributions are welcome, particularly in:

* dataset engineering
* evaluation methodology
* alignment research
* cultural-context analysis
* health-domain review
* ML engineering
* reproducibility
* documentation

Before contributing data or model outputs, contributors should ensure that they have the appropriate rights and permissions to share them.

See [`CONTRIBUTING.md`](CONTRIBUTING.md) for project contribution guidelines.

---

## Citation

If you use CARE research, datasets, tooling, or models in academic work, please cite the project using the citation information provided in [`CITATION.cff`](CITATION.cff).

---

## Disclaimer

CARE is a research project exploring culturally aligned AI-assisted SRH education.

**CARE models are not a substitute for qualified medical professionals, clinical services, emergency care, or individualized medical advice.**

Model outputs may contain errors. Users should seek appropriate professional healthcare guidance for medical concerns.

---

## License

The software in this repository is distributed under the license specified in [`LICENSE`](LICENSE).

Individual datasets, pretrained models, and third-party resources may have separate licenses and terms of use. Always review the applicable license before using or redistributing them.

---

**CARE Framework**
*Culturally Aligned Responsible Education for adolescent Sexual and Reproductive Health in African contexts.*
