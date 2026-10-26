# docs/ — template_pitch_deck

Human-facing documentation entry point. Agent-facing rules:
[`AGENTS.md`](AGENTS.md). Root overview: [`../README.md`](../README.md).

## What this repo is

Exemplar pitch-deck generation project (`template_pitch_deck` v1.0.0, MIT —
see `../pyproject.toml`): one token-resolved content source renders to six
artifacts (short/medium/long × PDF/PPTX), with every fact traced live to the
repository and every slide checked for pitch-deck cliché. The flagship deck
pitches `template_template` as meta-science research infrastructure.

## Directory map

- `src/` — content loading, `{{TOKEN}}` resolution, cliché lint, chart and
  diagram rendering, deck audit, diligence audit, render orchestration
  (see `src/README.md`)
- `scripts/` — numbered thin orchestrators: `10_audit_deck_content.py`,
  `15_generate_diagrams.py`, `16_generate_charts.py`,
  `20_render_decks.py`, `30_audit_diligence.py` (see `scripts/README.md`)
- `manuscript/` — deck content YAML + a standard descriptive manuscript
  (see `manuscript/README.md`)
- `tests/` — project test suite
- `output/` — rendered artifacts (local pipeline outputs)
- `docs/` — this documentation folder

## How to run / test

Entry points come from the numbered scripts (run via `uv run`, consistent
with the template workspace): `scripts/20_render_decks.py` (render) and
`scripts/10_audit_deck_content.py` (content audit). Tests run with pytest
(`tests/`); coverage configuration lives in `../pyproject.toml`.

Both the scripts and the test suite import `infrastructure.*`
(`src/deck_tokens.py`, `src/content_loader.py`, `src/infra_facts.py`,
`src/render_orchestration.py`, `tests/conftest.py`), which ships only in the
[`docxology/template`](https://github.com/docxology/template) monorepo.
`tests/conftest.py` resolves the repo root via `src/paths.py::locate_repo_root`,
which walks up from the project looking for a directory that contains both
`infrastructure/` and `pyproject.toml` — so run the suite where that lookup
succeeds: the monorepo's own `projects/templates/template_pitch_deck/tests/`
(see [`../README.md`](../README.md) for the exact invocation), or this
repository cloned *inside* a `docxology/template` checkout. With no
`infrastructure/` anywhere above the project, neither the numbered scripts nor
the test suite can run; the tracked `output/` artifacts come from the monorepo
pipeline.

This checkout has no workspace parent: `../pyproject.toml` is the uv root, so
`uv lock`/`uv sync` run against this repo directly and `.venv/` is
project-local.

## Maintenance

Docs here are short and factual, derived from the repo's own files. When
scripts or `src/` modules change, update the directory map above.
