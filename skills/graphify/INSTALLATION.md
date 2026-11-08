# Graphify for Codex

Graphify turns code and documents into a persistent, queryable knowledge graph. This folder contains Graphify's Codex-specific instructions and their reference files.

## Install

Install this entire folder into your Codex skills directory, then install the separate CLI:

```bash
pipx install graphifyy==0.9.104
graphify --help
```

If you use uv instead of pipx:

```bash
uv tool install graphifyy==0.9.104
```

Python 3.10 or newer is required. Restart Codex if the skill is not visible, then ask:

```text
Use $graphify . to map this project and explain its main components.
```

Code extraction uses local AST parsing and needs no API key. Semantic extraction for documents can use the running assistant; Gemini integration is optional. The Codex workflow uses parallel agents for semantic work when that capability is available. The upstream README describes the `multi_agent` feature setting for Codex versions that require it; this installation does not change your Codex configuration.

To install the skill directly from the upstream CLI rather than this collection, run `graphify install --platform codex`. That command writes the Codex skill and references; avoid keeping duplicate installations in different discovery directories.

## Source and version

- Project: https://github.com/Graphify-Labs/graphify
- Package: `graphifyy==0.9.104`
- Upstream source reviewed: `6c8002eca4acad58db6160eabcb466e61fc35afb` (branch `v8`)
- Skill: `graphify/skill-codex.md`
- References: `graphify/skills/codex/references/`
- Added to this collection: 2026-11-08

The skill and reference files are copied from the installed upstream package. They are kept separate from the CLI runtime, which is not included here. Review upstream release notes before upgrading the package and refresh the skill files together with the CLI.

## Attribution

Copyright 2026 Safi Shamsi and the Graphify contributors. Graphify is licensed under Apache-2.0, with earlier MIT-licensed contributions retained under their original terms. See `LICENSE`, `LICENSE-MIT`, and `NOTICE` in this folder.

