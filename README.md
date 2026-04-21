# design-critiques

Design critique briefs produced by the Gifthealth Digital Experience plugin. Critics persist here; readers query here.

## What lives in this repo

Briefs are written by agents, not humans. Every brief is a structured markdown document with YAML frontmatter (14+ keys) plus the full critique body. Two kinds live here:

- **Multi-lens** (`/design-review`) — orchestrated review across patient, commercial, and craft lenses with synthesized consensus, tensions, and a ranked top-5 action list. Path: `design/critiques/YYYY-MM-DD-slug.md`.
- **Single-lens** (`/critique-patient`, `/critique-commercial`, `/critique-craft`) — one lens, opt-in persist. Path: `design/critiques/single/{lens}/YYYY-MM-DD-slug.md`.

## Structure

```
design/critiques/
├── _index.md                 human-readable table of all briefs
├── _index.json               machine-readable corpus index
├── YYYY-MM-DD-slug.md        multi-lens briefs (flat)
└── single/
    ├── patient-advocate/     one file per single-lens patient critique
    ├── commercial-lens/
    └── craft-critic/
```

Both index files are **regenerated from a fresh directory scan** on every write. Don't hand-edit them.

## Querying the corpus

`_index.json` is the authoritative machine-readable source. Each entry carries `slug`, `date`, `scope`, `reviewed_lenses`, `artifact`, `artifact_type`, `invoked_by`, `summary`, `top_heuristics`, `top_biases`, and `link`. Filter by `scope` to separate casual single-lens passes from synthesized multi-lens reviews.

## How briefs get here

Briefs are written by the `gifthealth-digital-experience` plugin ([giftHEALTH/agents](https://github.com/giftHEALTH/agents)) running in Claude Code. Each commit is authored by the invoking user's GitHub account via their own `gh` CLI token; no shared credential. Authorship is split:

- `author` in frontmatter is always the static label `"Digital Experience Critique (Claude)"` — the content is AI-generated.
- `invoked_by` captures the GitHub login of the user who ran the command.

## Access

Public repository. Write access is granted to Gifthealth org members — ask the Digital Experience team or a repo admin if you need push permissions.

## Read-only sources

The plugin reads strategy, research, and design-foundation files from [giftHEALTH/product-operating-system](https://github.com/giftHEALTH/product-operating-system). That repo is not modified by the plugin — this repo is the only write target.