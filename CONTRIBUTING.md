# Contributing to CARE Trainer

## Branch workflow

All work happens on a dedicated branch using one of these prefixes:

- `feat/` for new features and functionality.
- `doc/` for documentation changes.
- `fix/` for bug fixes.

Use a short, descriptive branch name, such as `feat/dataset-validation`,
`doc/notebook-guidelines`, or `fix/data-loading`.

Create work branches from `dev`. Do not make changes directly on `dev` or `main`.
When the work is complete and reviewed, merge the branch into `dev`.
The project maintainer pushes `dev` personally; contributors and automated agents
must leave that push to the maintainer.

## Before merging

- Keep changes focused and use descriptive commit messages.
- Run checks appropriate to the change and record any limitations.
- Update relevant documentation when behavior or workflows change.
- For research work, document the question, data sources, configuration, and
  findings so the experiment can be understood and reproduced.
- Keep reusable Python logic under `src/care_trainer/`; use notebooks for
  exploration, orchestration, and analysis. Follow `notebooks/README.md`.
- Do not commit credentials, private or identifiable health data, restricted
  datasets, model checkpoints, or large generated artifacts.
- Preserve dataset provenance, licensing, and attribution. Distinguish
  model-generated content from expert-reviewed or validated findings.
