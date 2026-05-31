# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

`kerygma_templates` — the template engine and announcement library for ORGAN-VII (Kerygma). Renders multi-channel announcements from Markdown templates with YAML frontmatter. Stdlib-only (no Jinja2, no pyyaml for template parsing).

## Package Structure

Source is in `kerygma_templates/`, installed as the `kerygma_templates` package:

| Module | Purpose |
|--------|---------|
| `engine.py` | `TemplateEngine` — loads `.md` templates, renders with `{{ var }}` interpolation, `{{#if}}` conditionals, `{{#channel name}}` blocks. Custom frontmatter parser using regex (not pyyaml). |
| `quality_checker.py` | `QualityChecker` — validates rendered text: char limits per platform (`CHANNEL_LIMITS`), unresolved vars, anti-patterns, hashtag counts, link presence. Returns `QualityReport`. |
| `registry_loader.py` | `RegistryLoader` — loads ORGAN-IV's `registry-v2.json`, builds template context dicts. `EventContext` dataclass carries event metadata. `build_context()` merges event + repo + system data. |
| `cli.py` | CLI entry point (`announce`): `list`, `render <id> <channel>`, `validate`, `check <id> <channel>`. Uses `sample_context()` for demo rendering. |
| `data_export.py` | Generates `data/template-registry.json` — template inventory, channel limits, quality summary with per-failure detail. CLI: `announce-export` or `python -m kerygma_templates.data_export`. |

## Templates

Templates live in `templates/` organized by category: `launch/`, `release/`, `essay/`, `community/`, `institutional/`. Each `.md` file has YAML frontmatter defining `template_id`, `channels` (list), and `variables` (list). Template IDs use kebab-case (e.g., `essay-announce`, `repo-launch`).

Template syntax:
- `{{ var.path }}` — dot-path variable interpolation
- `{{#if condition}} ... {{#else}} ... {{/if}}` — conditionals (supports nesting)
- `{{#channel mastodon}} ... {{/channel}}` — channel-specific blocks (engine extracts matching block, discards others)

## Development Commands

```bash
# Install (from superproject root or this directory)
pip install -e .[dev]

# Tests
pytest tests/ -v
pytest tests/test_engine.py::TestTemplateEngine::test_render_with_channel -v

# Lint
ruff check kerygma_templates/
```

## Key Design Details

- **Frontmatter parser** in `engine.py:parse_frontmatter()` is a minimal custom implementation — handles `key: value`, `- item` lists, inline `[a, b]` lists, booleans, and integers. Not a full YAML parser.
- **Quality checker** treats `severity="error"` checks as blocking (fail the report) and `severity="warning"` as advisory. Platform char limits: Mastodon 500, Discord 4096, Bluesky 300, Twitter 280, LinkedIn 1300, Ghost unlimited.
- **Template loading** via `TemplateEngine.load_directory()` recurses `.md` files that start with `---\n` frontmatter. Templates without frontmatter are skipped.
- **No runtime dependencies** — the package has zero `dependencies` in `pyproject.toml`. Dev dependencies are pytest and ruff.

## Test Structure

Tests in `tests/` with `fixtures/` directory for test templates:
- `test_engine.py` — rendering, interpolation, conditionals, channel blocks
- `test_quality_checker.py` — char limits, anti-patterns, unresolved vars
- `test_registry_loader.py` — registry loading, context building
- `test_cli.py` — CLI subcommand smoke tests
- `test_templates_valid.py` — validates all templates in `templates/` parse and render

<!-- ORGANVM:AUTO:START -->
## System Context (auto-generated — do not edit)

**Organ:** ORGAN-VII (Marketing) | **Tier:** archive | **Status:** GRADUATED
**Org:** `organvm-vii-kerygma` | **Repo:** `announcement-templates`

### Edges
- **Produces** → `ORGAN-IV`: template_pack

### Siblings in Marketing
`social-automation`, `distribution-strategy`, `.github`, `kerygma-pipeline`, `kerygma-profiles`

### Governance
- *Standard ORGANVM governance applies*

*Last synced: 2026-05-23T00:26:31Z*

## Active Handoff Protocol

If `.conductor/active-handoff.md` exists, **READ IT FIRST** before doing any work.
It contains constraints, locked files, conventions, and completed work from the
originating agent. You MUST honor all constraints listed there.

If the handoff says "CROSS-VERIFICATION REQUIRED", your self-assessment will
NOT be trusted. A different agent will verify your output against these constraints.

## Session Review Protocol

At the end of each session that produces or modifies files:
1. Run `organvm session review --latest` to get a session summary
2. Check for unimplemented plans: `organvm session plans --project .`
3. Export significant sessions: `organvm session export <id> --slug <slug>`
4. Run `organvm prompts distill --dry-run` to detect uncovered operational patterns

Transcripts are on-demand (never committed):
- `organvm session transcript <id>` — conversation summary
- `organvm session transcript <id> --unabridged` — full audit trail
- `organvm session prompts <id>` — human prompts only


## System Library

Plans: 269 indexed | Chains: 5 available | SOPs: 8 active
Discover: `organvm plans search <query>` | `organvm chains list` | `organvm sop lifecycle`
Library: `/Users/4jp/Code/organvm/praxis-perpetua/library`


## Active Directives

| Scope | Phase | Name | Description |
|-------|-------|------|-------------|
| system | any | atomic-clock | The Atomic Clock |
| system | any | execution-sequence | Execution Sequence |
| system | any | multi-agent-dispatch | Multi-Agent Dispatch |
| system | any | session-handoff-avalanche | Session Handoff Avalanche |
| system | any | system-loops | System Loops |
| system | any | prompting-standards | Prompting Standards |
| system | any | background-task-resilience | background-task-resilience |
| system | any | context-window-conservation | context-window-conservation |
| system | any | session-self-critique | session-self-critique |
| system | any | the-descent-protocol | the-descent-protocol |
| system | any | the-membrane-protocol | the-membrane-protocol |
| system | any | theory-to-concrete-gate | theory-to-concrete-gate |
| system | any | triangulation-protocol | triangulation-protocol |

Linked skills: SOP-TRIADIC-REVIEW-PROTOCOL, cicd-resilience-and-recovery, continuous-learning-agent, evaluation-to-growth, genesis-dna, multi-agent-workforce-planner, promotion-and-state-transitions, quality-gate-baseline-calibration, repo-onboarding-and-habitat-creation, session-self-critique, structural-integrity-audit, the-membrane-protocol, triple-reference


**Prompting (Anthropic)**: context 200K tokens, format: XML tags, thinking: extended thinking (budget_tokens)


## Atomization Pipeline

Run `organvm atoms pipeline --write && organvm atoms fanout --write` to generate task queue.


## System Density (auto-generated)

AMMOI: 25% | Edges: 0 | Tensions: 0 | Clusters: 0 | Adv: 27 | Events(24h): 37975
Structure: 8 organs / 148 repos / 1654 components (depth 17) | Inference: 0% | Organs: META-ORGANVM:63%, ORGAN-I:53%, ORGAN-II:48%, ORGAN-III:54% +5 more
Last pulse: 2026-05-23T00:26:28 | Δ24h: n/a | Δ7d: n/a


## Dialect Identity (Trivium)

**Dialect:** SIGNAL_PROPAGATION | **Classical Parallel:** Astronomy | **Translation Role:** The Broadcast — structure-preserving projection to external

Strongest translations: III (structural), VI (analogical), I (analogical)

Scan: `organvm trivium scan VII <OTHER>` | Matrix: `organvm trivium matrix` | Synthesize: `organvm trivium synthesize`


## Logos Documentation Layer

**Status:** ACTIVE | **Symmetry:** 0.5 (DREAM)

Nature demands a documentation counterpart. This formation maintains its narrative record in `docs/logos/`.

### The Tetradic Counterpart
- **[Telos (Idealized Form)](../docs/logos/telos.md)** — The dream and theoretical grounding.
- **[Pragma (Concrete State)](../docs/logos/pragma.md)** — The honest account of what exists.
- **[Praxis (Remediation Plan)](../docs/logos/praxis.md)** — The attack vectors for evolution.
- **[Receptio (Reception)](../docs/logos/receptio.md)** — The account of the constructed polis.

### Alchemical I/O
- **[Source & Transmutation](../docs/logos/alchemical-io.md)** — Narrative of inputs, process, and returns.



*Compliance: Record exists without implementation.*

<!-- ORGANVM:AUTO:END -->












## ⚡ Conductor OS Integration
This repository is a managed component of the ORGANVM meta-workspace.
- **Orchestration:** Use `conductor patch` for system status and work queue.
- **Lifecycle:** Follow the `FRAME -> SHAPE -> BUILD -> PROVE` workflow.
- **Governance:** Promotions are managed via `conductor wip promote`.
- **Intelligence:** Conductor MCP tools are available for routing and mission synthesis.