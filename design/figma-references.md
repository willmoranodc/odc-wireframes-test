# Figma References

This file lists every Figma file used on this project. Keep it up to date — Claude reads this to know where to look, and teammates use it to navigate without hunting through Figma's sidebar.

When you add, rename, or archive a Figma file for this project, update this doc in the same PR.

---

## Primary files

### Design system library
The component library for this project. All components here should have Code Connect mappings in `src/components/` with matching `*.figma.tsx` files.

- **URL:** `https://www.figma.com/design/[file-id]/[Client]-Design-System`
- **Owner:** [Designer name]
- **Linked to repo:** Yes, via Code Connect UI → see `figma.config.json`

### Main design file
Active screen design and flows for this project.

- **URL:** `https://www.figma.com/design/[file-id]/[Client]-Design`
- **Owner:** [Designer name]

### FigJam (research/strategy)
Workshop boards, journey maps, and synthesis artifacts. Referenced during strategy and design phases.

- **URL:** `https://www.figma.com/board/[file-id]/[Client]-Research`
- **Owner:** [Researcher name]

---

## Key frames and flows

Specific frames that come up often. Copy the "Copy link to selection" URL from Figma (right-click a frame → Copy link).

| Name | Purpose | Link |
|---|---|---|
| Homepage — desktop | Main marketing page | `https://www.figma.com/design/[...]?node-id=[...]` |
| Homepage — mobile | Mobile breakpoint | `https://www.figma.com/design/[...]?node-id=[...]` |
| Auth flow | Sign-in and signup screens | `https://www.figma.com/design/[...]?node-id=[...]` |

---

## Conventions for this project

Anything client-specific worth noting here. Examples:

- Breakpoints used in design: 320, 768, 1280, 1920
- Icon system: [e.g., Lucide, custom SVGs in `src/components/icons/`]
- Typography token names use this pattern: `text-heading-[size]`, `text-body-[size]`
- Any gotchas specific to this file (e.g., "the old hero variant in the archive page is deprecated — don't use it as reference")

---

## Figma ↔ code mapping

A quick reference for where Figma components live in code. This is not a substitute for Code Connect mappings — those are the source of truth — but it's useful for human navigation.

| Figma component | Code location |
|---|---|
| Button | `src/components/Button.tsx` |
| Card | `src/components/Card.tsx` |
| NavBar | `src/components/NavBar.tsx` |

---

## Notes

- Don't commit `.fig` exports to the repo. Figma files are not versioned files in the Git sense — Figma's own version history is the source of truth for design changes.
- If a Figma file is archived or deprecated, move it under a "Deprecated" heading below rather than deleting the entry. Someone will eventually ask "what happened to the old homepage file."
