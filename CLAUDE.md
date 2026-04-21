# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A client project workspace for One Design Company (ODC). This is not a software application — it's a collaborative repository holding all file-based deliverables for a client: research, strategy, design notes, and prototype code. Every client gets the same folder structure so the whole team can move between projects without relearning where things live.

## Folder structure

- `research/` — Discovery outputs: interview guides, transcripts, synthesis
- `strategy/` — Strategy memos, roadmaps, recommendations
- `design/` — Design notes, component docs, design system references
- `prototype/` — Prototype code (Vercel apps, static HTML, etc.)
- `docs/` — Internal process docs (e.g., onboarding guide)
- `.agent/skills/` — Project-level Claude skills, layered on top of global skills
- `CLAUDE.md` — This file. Project-specific context Claude reads every session.

Figma files stay in Figma — link them from the relevant folder's README rather than storing them here.

## Git workflow

- `main` is protected. Never push directly to it.
- Branch naming: `{area}/{description}` — e.g., `research/stakeholder-interviews`, `design/homepage-wireframes`, `prototype/auth-flow`
- Every change reaches `main` via a pull request reviewed and approved by the project lead.
- This applies to all work types: research docs, strategy memos, design notes, and prototype code alike.

## Two-layer Claude context

Claude gets context from two places:
1. **Global skills** — reusable across all projects, live outside this repo
2. **Project-level context** — this `CLAUDE.md` and any skills in `.agent/skills/`

When you develop a repeatable process for this client, save it as a skill in `.agent/skills/`. If Claude makes a recurring mistake, add a correction to this file.

## Prototype code

The `prototype/` folder may contain Vercel apps, static HTML, or other runnable code. If a prototype is present, look for its own README or `package.json` inside that subdirectory for build and run instructions — there are no top-level build commands for the repo itself.
