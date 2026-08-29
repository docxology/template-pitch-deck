# AGENTS.md — docs/ (template_pitch_deck)

## Layout

- `README.md` — human entry point (what this repo is, directory map, run/test)
- `AGENTS.md` — this file: conventions for maintaining docs/

## Key modules (pointer, not duplication)

Authoritative per-module rules live in each package's own `README.md` /
`AGENTS.md` (`src/`, `scripts/`, `tests/`, `manuscript/`). The root
`../AGENTS.md` holds the technical specification; `../STANDALONE.md`
documents standalone operation.

## Conventions observed in this repo

- Token convention: deck content uses `{{TOKEN}}` placeholders; every token
  must resolve from live repository facts (see `src/token_resolution.py`
  contract) — unresolvable tokens fail loudly.
- Cliché lint: slides are linted against a word-boundary denylist
  (`src/cliche_lint.py`).
- Every claim in deck content may be deep-linked to its source file
  (`config.yaml` `source_base_url`).
- Business logic in `src/`; `scripts/` are thin orchestrators.

## Maintaining these docs

Keep both files short (30–80 lines), factual, and derived only from repo
files. Update the directory map when top-level layout changes. Do not
duplicate module-level rules here.
