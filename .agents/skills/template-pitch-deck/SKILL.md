---
name: template-pitch-deck
description: Pitch-deck generation exemplar — short/medium/long PDF+PPTX decks from one token-resolved, diligence-cited content source.
version: 1.0.0
author: docxology
license: MIT
tags: [exemplar, slides, reportlab, pptx, mermaid, pitch-deck]
---

# template-pitch-deck

Project-scoped skill for the in-repo exemplar at
`projects/templates/template_pitch_deck/`. Load this when working inside the project.

## When to Use

- Working inside the `template_pitch_deck` exemplar — running scripts, editing content, or regenerating outputs.
- Forking this exemplar as the starting scaffold for a new pitch/grant-report/stakeholder-update deck.
- Adding a new format-agnostic slide renderer to `infrastructure/rendering/` (e.g. this is the reference for `DeckContent`/`Slide`/`DeckTheme`).
- Validating that the exemplar's contracts (thin-orchestrator, token honesty, cliché lint, diligence citation coverage, no-mocks testing) still hold after changes.

## Quick Reference

Use the [pinned build workflow](../../../docs/build.md) for generators and tests.
See [authoring](../../../docs/authoring.md) for subject configuration and source links.

## Pitfalls

- **Keep scripts thin.** Content/validation logic belongs in this project's `src/`; layout/drawing logic belongs in `infrastructure/rendering/{slide_deck,pptx_deck,mermaid_figure}.py` — never in `scripts/`.
- **No mocks.** All tests must use real YAML, real rendered files (read back with `pypdf`/`python-pptx`), real repo introspection.
- **Every fact-bearing slide needs a `source` citation.** `src/diligence_audit.py` fails the build otherwise (title-kind slides are exempt).
- **Never hand-type a fact.** All `PITCH_SUBJECT_*`/`EXEMPLAR_*` token values come from `src/deck_tokens.py::build_deck_tokens`, which reads them live from the repo — adding a new fact means adding a new live read, not a literal.
- **PDF/PPTX parity is load-bearing.** The two renderers must always produce identical slide counts, text, AND font sizes for the same `DeckContent` — a change to one renderer's slide-kind dispatch or font-size constant needs the matching change in the other (see `test_render_pptx_slide_count_matches_render_pdf_page_count`; font sizes are imported from `slide_deck.py` into `pptx_deck.py`, never re-declared).
- **Configure the slide-page URL.** Set `project_config.deck.slides_base_url`
  for QR links and `source_base_url` for shared reference citations.
- **Run tests in the prepared project environment.** The build guide installs the
  declared project dependencies and PPTX provider before running pytest.
- **Regenerate artifacts from source.** Use a disposable checkout to preserve
  the archived output committed to this repository.


## Cross-refs

- Project contract: [`AGENTS.md`](../../../AGENTS.md)
- README: [`README.md`](../../../README.md)
- TODO: [`TODO.md`](../../../docs/backlog.md)
- Rendering primitives: `infrastructure/rendering/slide_deck.py`, `pptx_deck.py`, `mermaid_figure.py`
- Exemplar roster: `projects/AGENTS.md` (monorepo-relative path; not present in this standalone checkout)
