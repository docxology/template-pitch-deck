# Introduction {#sec:introduction}

A presentation can summarize the same evidence as a computational manuscript
while serving a different purpose. Recommendations for reproducible research
emphasize recording each result's provenance and preserving executable
workflows [@sandve2013ten; @wilson2017good]. FAIR software principles similarly
call for keeping software and its metadata interpretable and reusable
[@lamprecht2020towards]. This project applies those practices to a presentation
by generating its slide text and figures from versioned source and reference
data.

**One content source, six artifacts.** `docs/manuscript/deck_content_{short,medium,long}.yaml`
define the slide-by-slide narrative at three lengths; the live token builder (`src/deck_tokens.py`)
supplies the facts. `scripts/20_render_decks.py` resolves tokens once and calls
both renderers — `infrastructure.rendering.slide_deck.render_pdf` and
`infrastructure.rendering.pptx_deck.render_pptx` — against the identical
resolved content, so PDF and PPTX cannot drift from each other.

**Facts, not fabrication.** Every numeric claim produced by `src/deck_tokens.py` about the
pitch subject (`template_template`) is generated from a live read of that
subject's configuration, the generated coverage report, and the public
exemplar roster — never hand-typed, never a plausible-sounding invented metric.
`src/token_resolution.py` raises loudly if any `{{TOKEN}}` in the content
source has no corresponding resolved value, and a rendered artifact's
extracted text is checked to contain zero leftover `{{` literals.

**Cliché is a lint, not a vibe.** `src/cliche_lint.py` runs a word-boundary
denylist of pitch-deck stock phrases over every resolved slide before
render. A pitch that reads as generic — regardless of how factually accurate
it is — fails the same way an uncovered line of code fails a coverage gate.

The following sections describe the deck architecture, content validation,
and reproduction workflow. The render command builds three lengths as PDF and
PPTX from the same checked content for each subject.
