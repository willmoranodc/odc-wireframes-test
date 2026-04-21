# [Client Name] — Project Repo

This is a new client project built from the R4 client template. Every client gets their own repo following this same structure, so anyone on the team can move between projects without relearning where things live.

## What goes in this repo

Everything file-based for this client: research, strategy docs, design system notes, prototype code, and the project-specific instructions we give to Claude. Figma files stay in Figma — this repo references them, but doesn't store them.

## Folder structure

```
.
├── README.md              ← You are here. Overview + conventions for this client.
├── CLAUDE.md              ← Project-specific instructions Claude reads every session.
├── .agent/                ← Project-level Claude skills (layered on top of global skills).
│   └── skills/
├── research/              ← Discovery outputs: interview guides, transcripts, synthesis.
├── strategy/              ← Strategy memos, roadmaps, recommendations.
├── design/                ← Design notes, component docs, design system references.
├── prototype/             ← Code for prototypes (Vercel apps, static HTML, etc.).
└── docs/
    └── onboarding.md      ← How to get set up if you're new to this workflow.
```

## Two-layer Claude setup

Claude gets context from two places when working on this project:

1. **Global skills** — general-purpose capabilities and process logic that apply to every R4 project. These live in the team's global skills library, not in this repo.
2. **Project-level skills and context** — client-specific conventions, voice and tone, design system rules, and gotchas. These live in `CLAUDE.md` and `.agent/skills/` in this repo.

When you open this repo in Cursor and start a Claude session, it will automatically pick up `CLAUDE.md`. Each phase (research → strategy → design) passes its outputs forward as context for the next phase, which is why we keep everything in one repo.

## Branching and review

Nothing reaches `main` without a pull request approved by Will.

- `main` is the source of truth. It is protected — you can't push to it directly.
- For any piece of work, create a branch off `main`. Name it after what you're doing: `research/stakeholder-interviews`, `design/homepage-wireframes`, `prototype/auth-flow`.
- Commit as you go. Push your branch. Open a pull request on GitHub when you're ready for review.
- Will reviews and merges.

This applies to everyone — researcher, designers, writers, guest collaborators. The PR is the universal review gate regardless of whether the work is a Figma update pushed via Code Connect, a research synthesis document, or prototype code.

## Figma

This project's Figma files live in the client's Figma team (mirroring this repo's name). Link them in the relevant folder's README rather than duplicating files here.

## Tools

- **Cursor** (with Claude integrated): primary workspace for everyone, not just developers. Open this repo as the root folder.
- **GitHub**: version control and PR review.
- **Figma**: design files, with Code Connect wiring components to prototype code when applicable.
- **Claude / Claude Code**: synthesis, generation, and agent-driven workflow execution.

## Getting started

If this is your first time working in one of these repos, read [`docs/onboarding.md`](docs/onboarding.md) first. It walks through installing everything and making your first pull request.
