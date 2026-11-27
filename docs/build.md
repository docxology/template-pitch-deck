# Build and Verify

The standalone project uses the pinned `docxology/template` checkout for shared
renderers and reference facts about the pitch subjects. Use a separate project
checkout so the release snapshot and development outputs stay attributable.

## Set up the pinned checkout

Install uv, Pandoc 3.14, pandoc-crossref 0.3.26, XeLaTeX, and BibTeX. For the
deck diagrams, install Mermaid CLI 12.2.0 with Chromium or Chrome Headless
Shell. Add the CLI to `PATH`. Set `PUPPETEER_EXECUTABLE_PATH` to the browser
executable when Mermaid needs an explicit path.

```bash
git clone https://github.com/docxology/template.git
cd template
git checkout --detach 87704feae0a4490ef7dad9c931d268e8c30273c2
uv sync --locked --no-dev --python 3.14
renderer="$PWD"
mkdir -p projects/working
git clone https://github.com/docxology/template-pitch-deck.git projects/working/template-pitch-deck
cd projects/working/template-pitch-deck
uv sync --locked --extra dev --extra rendering-pptx --python 3.14
unset NODE_PATH NODE_OPTIONS
```

The optional `rendering-pptx` extra installs the pinned provider in the
project environment for the PDF/PPTX parity checks. A project environment
without that extra supports the PDF-only path. The shared renderer has its
own environment.

## Run the native tests and build the decks

From the disposable project checkout, move the archived outputs aside before
running the generators.

```bash
mv output ../template-pitch-deck-published-output
uv run --no-sync python -m pytest tests/ --cov=src --cov-fail-under=90
uv run --no-sync python scripts/10_audit_deck_content.py
uv run --no-sync python scripts/15_generate_diagrams.py
uv run --no-sync python scripts/16_generate_charts.py
uv run --no-sync python scripts/20_render_decks.py
uv run --no-sync python scripts/30_audit_diligence.py
uv run --no-sync python scripts/20_render_decks.py --pitch-subject template_methods_paper
```

Each subject produces a PDF and PPTX for each length in `output/pdf/` and
`output/pptx/`. Filenames contain the subject and length. Generated Markdown
slide pages are in `output/slides_standalone/`. Their local content links point
to the specific YAML file used for that subject and length.

## Render the manuscript

Run the pinned stages from the shared renderer checkout.

```bash
cd "$renderer"
uv run --no-sync python scripts/pipeline/stage_03_render.py --project working/template-pitch-deck
uv run --no-sync python scripts/pipeline/stage_04_validate.py --project working/template-pitch-deck
```

The [manuscript configuration](manuscript/config.yaml) enables the descriptive
paper's combined PDF and HTML. Find them under
`projects/working/template-pitch-deck/output/pdf/` and `output/web/index.html`
within that project. Validation and render provenance are in `output/reports/`.
