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
(`tests/`); coverage configuration lives in `../pyproject.toml`. For exact
invocation of the canonical monorepo pipeline stages, see the root monorepo
documentation — this standalone checkout inherits workspace settings from
the root `pyproject.toml`.

## Maintenance

Docs here are short and factual, derived from the repo's own files. When
scripts or `src/` modules change, update the directory map above.
