# Agent-Standards — The "Agent-Replication" Book

> **What this is:** This repository is **one Book** in the wider *Bible* of Hermes standards.
> Its title is **Agent-Replication**. One repo = one project = one Book.
> Other projects (e.g. Podman replication) live in their **own** repos as separate Books.

## Cosmology (Reference Map)
- **The Bible** — the whole collection of standards across all projects (a concept + a reference map, not a single repo).
- **A Book** — one project = one git repo. *This repo is the `Agent-Replication` Book.*
- **A Mark** — a chapter / versioned baseline within a Book. Current active Mark: **Mark II**.

## Current State
- **Active Mark:** Mark II (`marks/MARK_II.md`, tagged `mark-ii`) — the first proper chapter.
- **Mark I:** Legacy genesis baseline (superseded). Not on `main`; recover with `git checkout mark-i`.

## Layout
```
agent-standards/                  ← the Agent-Replication Book
├── README.md                     ← this map
├── MEMORY_INJECTION.md           ← non-negotiable rules a new agent saves to memory
├── marks/                        ← current Mark (Mark II) as top-level Book content
│   ├── MARK_II.md                ← current chapter (active baseline)
│   └── CHANGELOG.md              ← flag ledger for this Book
├── procedures/
│   └── pull-mark.md              ← how an agent ingests a Mark
└── skills/
    └── hermes-constitution-problem-solving/   ← the framework library (Trinity, 5 Pillars, 7-Step Cascade)
```
> **Marks are git tags, not folders.** `main` carries only the current Mark. `git checkout mark-i`
> resurrects the superseded genesis baseline (its `marks/mark-i/MARK_I.md`) as a full restore point.

## Core Policy vs. Local Culture
- **Core Policy (this Book):** the rigid frameworks and protocols governing *how* an agent thinks and acts. Universal.
- **Local Culture:** each agent instance develops its own personality, domain knowledge, and style on top of this base. Left undefined here on purpose.

## How to Ingest (Agent Initialization)
A newly spawned Hermes Agent bootstraps from this Book by:
1. Read `MEMORY_INJECTION.md` and use `memory(action='add', target='memory')` to save the operational rules.
2. Copy `skills/` into local `~/.hermes/skills/` to absorb the analytical frameworks.
3. Follow `procedures/pull-mark.md` to sync with the current Mark (Mark II).
