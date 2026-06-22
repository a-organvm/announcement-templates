# Discovery: organvm/announcement-templates

**Date:** 2026-06-22 · **Verdict:** VALUE FOUND → promote to ranked tier · **Organ:** ORGAN-VII (Kerygma)

## Value Thesis

`announcement-templates` is mislabeled as an `archive`/GRADUATED leaf, but it is in fact a
live, zero-dependency content-rendering engine that is the only reusable cross-channel
publishing primitive in the estate. Stripped of its ORGAN-VII framing it is three general
assets bundled in one stdlib-only package (`kerygma_templates`, 101 passing tests, no runtime
deps): (1) a **template engine** (`engine.py`) with `{{var}}` interpolation, nested
`{{#if}}` conditionals, and per-channel `{{#channel}}` block extraction driven by Markdown +
YAML-ish frontmatter; (2) a **platform quality gate** (`quality_checker.py`) that enforces
real-world constraints any content-producing repo needs — Mastodon/Discord/Bluesky/Twitter/
LinkedIn/Ghost character limits, hashtag caps, unresolved-variable detection, anti-pattern and
broken-link checks, with error/warning severity; and (3) a **registry loader** (`registry_loader.py`)
that hydrates templates from ORGAN-IV's `registry-v2.json`. It already ships a CLI (`announce`)
and a data exporter (`announce-export` → `data/template-registry.json`) that is consumed
downstream. The latent value is leverage: the marketing siblings (`social-automation`,
`kerygma-pipeline`, `distribution-strategy`) and any organ that posts to a social platform
currently have no shared way to render-and-validate outbound copy — they would each reinvent
character limits and variable substitution. This repo already solved that, dependency-free, and
proved it works across 18 templates and 6 platforms. Its highest-value path is to stop being a
static JSON producer for one consumer (ORGAN-IV) and become an **importable rendering library**
consumed directly by every publishing repo. That is a concrete reusable-asset path, not
speculation — the engine, the checker, and the tests already exist and are green.

## What it is (verified)

- **Package:** `kerygma_templates/` — `engine.py`, `quality_checker.py`, `registry_loader.py`,
  `cli.py`, `data_export.py`. Entry points: `announce`, `announce-export` (`pyproject.toml`).
- **Templates:** 18 `.md` files across `launch/`, `release/`, `essay/`, `community/`,
  `institutional/`.
- **Tests:** 101 passing (`pytest tests/`). No runtime dependencies.
- **Produces:** `data/template-registry.json` (template inventory + per-failure quality summary),
  declared in `seed.yaml` as a `template_pack` consumed by ORGAN-IV.

## Why not archival

It is running code with a green test suite, a CLI, and a live downstream consumer. The "archive"
tier reflects organizational status, not technical value: the rendering + validation core is
generic and reusable by the rest of the estate today.

## Single best concrete first task

**Expose a one-call public API and consume it from a sibling.** Add
`kerygma_templates.render_and_check(event, channel) -> (text, QualityReport)` to `__init__.py`
(thin wrapper over `TemplateEngine.render` + `QualityChecker.check`, loading the bundled
`templates/` dir by default), then wire `social-automation` (or `kerygma-pipeline`) to import it
instead of reading the static JSON export. This converts the repo from a single-consumer artifact
producer into an estate-wide publishing library — the smallest change that unlocks the latent
leverage above.

## Follow-on opportunities

- Fix the 6 real quality failures surfaced in `data/template-registry.json` (e.g.
  `community-milestone` unresolved vars) so the exported pack passes clean.
- Publish the package to the internal index so siblings can `pip install` it.
- Add a docs site (`content` pillar in `ecosystem.yaml` is `not_started`) documenting template
  format + channel limits as the canonical reference.
