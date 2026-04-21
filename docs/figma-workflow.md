# Figma ↔ Code Workflow

This doc covers how Figma, the repo, and Claude work together on this project. If you've gone through `docs/onboarding.md` and you're comfortable opening a branch and pushing a PR, you're ready for this.

The short version: the design system lives in Figma, the code lives in the repo, and three things connect them — the Figma MCP server (which lets Claude read and write Figma), Code Connect (which maps Figma components to code components), and your PR workflow (which gates all changes to `main`).

## The mental model

Three pieces do three different things. Keep them straight.

**Figma** is where design files live. Full stop. A Figma file is cloud-hosted — there's no `.fig` file that lives in the repo. Version history is in Figma itself.

**Figma MCP server** is the pipe between Claude and Figma. When Claude is running in Cursor or Claude Code, the MCP server lets Claude read design context out of a Figma file — frames, components, variables, auto-layout — and write UI back into Figma as native editable layers.

**Code Connect** is the dictionary the pipe uses. It's a set of files in this repo (`*.figma.tsx` plus `figma.config.json`) that tell Figma "this component in the design file is this component in the code." When Claude generates code from a Figma frame, Code Connect is why it uses the real `<Button />` component instead of generating a new one from scratch.

```
Figma (cloud)  ←→  MCP server  ←→  Claude (in Cursor)  ←→  Repo (code + Code Connect mappings)
                                                                ↑
                                                          PR review gate
```

---

## One-time setup

Do this once per machine. If you've already set up Cursor and GitHub from `docs/onboarding.md`, skip to step 2.

### 1. Install the Figma plugin for Cursor

In Cursor's agent chat, run the install command for Figma's official plugin. This bundles the remote MCP server configuration plus a set of agent skills for common Figma workflows — things like "implement this design," "connect a component via Code Connect," and "create design system rules."

The remote server is preferred over the local/desktop version. It connects directly to Figma's hosted endpoint at `mcp.figma.com/mcp` and doesn't require the Figma desktop app.

### 2. Connect your Figma account

When prompted by Cursor, click Connect next to Figma and authenticate. You'll need a Dev or Full seat on the Organization plan for write access and Code Connect features.

### 3. Verify Code Connect is set up for this repo

Every client repo has a `figma.config.json` at the root. Confirm it's there. It looks something like:

```json
{
  "codeConnect": {
    "include": ["src/**/*.figma.tsx", "src/**/*.figma.ts"],
    "label": "React",
    "language": "jsx"
  }
}
```

If this is a brand-new client project and Code Connect hasn't been wired up yet, see "First-time Code Connect setup" at the bottom of this doc.

### 4. Check the project's Figma files

Open `design/figma-references.md`. This lists every Figma file for this project. Make sure you have access to them — if you don't, ping the file owner.

---

## Rule: don't run two Figma MCP servers at once

If you've installed Figma MCP globally and this repo also configures one, or if you have a third-party Figma MCP connected, disable all but one per session. Two servers accessing the same Figma data confuses the agent and produces inconsistent output. This is a known gotcha.

---

## Workflow A: Design → Code

You want to implement a Figma design as code in the repo. This is the most common direction.

### 1. Start on a branch

Before touching anything, create a branch. `design/homepage-hero`, `feature/auth-flow`, whatever fits. See `docs/onboarding.md` if you're fuzzy on branches.

### 2. Point Claude at the Figma frame

Two ways to do this:

- **Copy link from Figma.** In Figma, right-click the frame or component you want to build → "Copy link to selection." Paste that URL into Cursor's chat and ask Claude to implement it: "Implement this frame as a React component in `src/pages/homepage.tsx`, using the design system components we already have."
- **Select in Figma, prompt in Cursor.** Select a frame in Figma's UI, then prompt Claude in Cursor with something like "Implement the current Figma selection." Claude's MCP call will pick up what you have selected.

### 3. Let Claude use Code Connect

Claude reads the frame, sees that (say) the frame contains a Button component. It checks Code Connect, finds that the Button in Figma maps to `src/components/Button.tsx`, and uses that component — with the right props — instead of inventing a new one. This is the whole point of Code Connect.

If Claude generates new components that should have already existed, that's a signal one of two things is wrong: the component isn't mapped in Code Connect yet, or the Figma frame is using a one-off style rather than a proper component instance. Both are worth fixing.

### 4. Review the generated code

In Cursor's Source Control panel, review every file Claude touched. The live preview in the editor window lets you see the rendered result. If Claude's generated HTML or CSS is off, you can click elements in the preview to see the specific code that produced them.

### 5. Commit, push, PR

Commit with a clear message. Push. Open a PR. The project lead reviews and merges — same as any other work.

---

## Workflow B: Code → Design

You've iterated in code and want to send the result back to Figma as editable layers — either for design review or to fold back into the design system. This direction is newer but worth using deliberately.

### 1. Have the code running

The rendered UI needs to exist somewhere Claude can see it. Usually that's the live preview in Cursor's built-in browser (running a local dev server on a prototype app) or a deployed preview.

### 2. Prompt Claude to send to Figma

In Cursor, something like: "Send the current browser preview to Figma as editable frames in [Figma file URL]." Claude uses the MCP server's write tools to create native Figma layers — frames, components, auto-layout, variables — rather than just a flat screenshot.

### 3. Refine in Figma

The result is editable. A designer can rearrange, restyle, polish, and treat it like any other Figma content. This is useful when:

- A prototype was built fast in code for a client feedback session, and now needs to be brought back into the design system properly.
- A designer wants to run visual variations on something Claude coded, using Figma's canvas (which is better for exploring variations side-by-side than prompting).

### 4. Close the loop

Once refined in Figma, you can regenerate the code from the updated Figma frame via Workflow A. This is the iterate-in-Figma, re-pull-into-code loop identified as the ideal feedback loop.

---

## Workflow C: Updating a component across both

You need to change a component that already exists in Figma and in code. Usually this is a design system update — "the button padding needs to change," "add a new variant."

### 1. Update Figma first

Make the change in the design system library file. This is the source of truth for the visual.

### 2. Pull into code via Claude

On a branch, ask Claude to update the component in code to match the updated Figma component. Reference the Figma URL. Claude reads the new spec and updates the TSX.

### 3. Update the Code Connect mapping if needed

If you added new props or variants, the `*.figma.tsx` mapping file may need updating so the new Figma variants are correctly mapped to the new code props. Claude can do this — ask it to "update the Code Connect mapping for this component to reflect the new variants."

### 4. Commit, push, PR

The PR will include the component code change and the Code Connect file change together. That's the right grouping — the two are inseparable.

---

## What lives where: the full picture

```
client-repo/
├── README.md
├── CLAUDE.md                       ← Project-specific Claude instructions
├── figma.config.json               ← Code Connect configuration
├── .agent/
│   └── skills/                     ← Project-level skills
├── research/                       ← Synthesis, interview guides
├── strategy/                       ← Roadmaps, recommendations
├── design/
│   ├── figma-references.md         ← URLs to every Figma file
│   └── system-notes.md             ← Design system decisions, rationale
├── src/
│   ├── components/
│   │   ├── Button.tsx              ← Actual component code
│   │   └── Button.figma.tsx        ← Code Connect mapping
│   └── pages/
└── prototype/                      ← Generated prototype code
```

Figma files themselves do not live here. They're referenced in `design/figma-references.md` and mapped to code via the `*.figma.tsx` files in `src/components/`.

---

## First-time Code Connect setup (for new client repos)

If this is a new client project and Code Connect hasn't been wired up yet, here's the short version. The person setting it up should be comfortable with Git and willing to read Figma's docs — it's not a five-minute job the first time.

### Decide: UI or CLI?

Figma offers two setup routes.

**Code Connect UI** runs inside Figma. It connects to GitHub, lets designers map components visually, and is the right default for our team. Most projects should use this.

**Code Connect CLI** runs in the repo. It supports dynamic property mappings and deeper integration, but requires every mapping to be authored as code. Reserve this for components that need logic beyond what the UI can express.

### Setting up Code Connect UI

1. In the client's Figma library file, open the Code Connect UI panel, click Settings, choose "Connect to GitHub."
2. Authorize Figma to access the client's repo (only that repo — don't grant access to the whole org).
3. For each component in the library, use the UI to map it to the component path in the repo. Autocomplete will help.
4. Confirm the mapping by checking Dev Mode — the code snippet shown for each component should reference the right file path and props.

### Setting up Code Connect CLI (if needed)

1. From the repo root, `npm install --save-dev @figma/code-connect`.
2. Create `figma.config.json` (the template includes this).
3. For a given component, run the CLI's create command pointed at the Figma component URL. It generates a `ComponentName.figma.tsx` file.
4. Edit that file to map Figma props to code props.
5. Run the CLI's publish command to register the mapping with Figma.
6. Commit everything.

---

## Plan and seat requirements

- Figma Organization or Enterprise plan (we're on Organization — ✓)
- Dev or Full seat for anyone doing Code Connect work or using the MCP server's write features
- View or Collab seats are limited to 6 MCP tool calls per month — fine for reviewers, not enough for designers doing active work

If someone on the team can't use the write tools, check their seat type first.

---

## Troubleshooting

**"Claude is generating new components instead of using our existing ones."**
Code Connect isn't mapped for that component, or the Figma frame is using a detached instance rather than a proper component. Check the Code Connect mapping for the component in Figma's Dev Mode — if the code snippet field is empty, that's the issue.

**"Claude wrote to the wrong Figma file."**
Always reference Figma URLs explicitly in prompts. Don't assume Claude has context about which file to write to based on the repo alone. If a Claude write goes to the wrong file, Figma's version history in the correct file is unaffected — the mistake is localized.

**"Claude's edit to Figma broke something, and I can't revert."**
This has happened before. Figma's version history will show the Claude write as an auto-save, but reverting to a pre-write version doesn't always stick cleanly. If you're about to let Claude write to a critical file, duplicate it first and point Claude at the duplicate. Don't rely on version history as your undo.

**"Two MCP servers are configured and the agent is confused."**
Pick one. Disable the other at the Cursor settings level or in the repo's MCP config — whichever applies.

**"The `figma.config.json` has no matches."**
Your `include` glob probably isn't finding the `*.figma.tsx` files. Check that the glob pattern matches where your mapping files actually live. Default template is `src/**/*.figma.tsx`.

---

## What's next

- If you haven't already, skim Figma's own guide to the MCP server — it's genuinely good and worth 15 minutes.
- Ask in `#ai-workflow` before doing something you're not sure about, especially anything that writes to a client's Figma file.
- When you figure something out that isn't in this doc, add it. This doc is meant to grow with the team's experience.
