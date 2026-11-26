# Reproducibility {#sec:reproducibility}

The build uses versioned slide YAML, a pinned Template checkout, and generated
figures. Keep the reference commit and toolchain with the output receipt. The
subject's coverage and publishing metadata come from that reference. The deck's
own configuration supplies its DOI and the links to its standalone slide pages.

From this project checkout, after preparing the shared Template reference:

```bash
uv run --no-sync python scripts/15_generate_diagrams.py
uv run --no-sync python scripts/16_generate_charts.py
uv run --no-sync python scripts/20_render_decks.py
uv run --no-sync python scripts/20_render_decks.py --pitch-subject template_methods_paper
uv run --no-sync python -m pytest tests/ --cov=src --cov-fail-under=90
```

Read back the PDF and PPTX to check slide counts, titles, and resolved text.
Check the per-slide Markdown links to identify the exact YAML source. Changes
to the input facts, fonts, or renderer can change a rebuild. The project
requires no model call to generate the deck.
