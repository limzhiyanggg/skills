---
name: spec-component
version: 1.1
description: Write a component spec for a UI component. Greps codebase usage, drafts the spec in chat, waits for approval, then saves.
---

## Config

Set these once. If any value is `UNSET`, ask the user before proceeding, then write their answers back here.

- `component_paths`: `src/components/ui/[component].tsx, src/components/[component].tsx`
- `grep_scope`: `src/`
- `output_path`: `docs/design-system/components/`

---

## Input

The component name to spec. Example: `/spec-component Badge`

---

## Workflow

1. **Check config** — read the Config section. If any value is `UNSET`, ask the user for each missing value, write the answers back into the Config section, then continue.

2. **Read the component file** — check each path in `component_paths` in order. Read the full file to understand variants, props, and structure.

3. **Grep codebase usage** — search `grep_scope` for all usages. Use findings to infer anatomy purposes, variant contexts, and best practices. Do not present a use cases table.

4. **Draft the spec in chat** — do not save yet. If any claim could not be confirmed in the component file or codebase, append an **Unverified** section at the end — a numbered checklist of items for the user to verify before approving.

5. **Wait for approval** — stop and ask the user to approve or request changes. Unverified items must be resolved before approval.

6. **Save** — once approved, write to `output_path/[component-name].md`.

---

## Writing Rules

- **Convention filter** — include a rule only if a designer or coding agent would make a wrong decision without it. Drop anything TypeScript, CSS, or the component itself enforces.
- **Purpose level only** — guide how to apply the component. Never describe implementation: no colors, dimensions, CSS classes, layout specs, or prop internals. Those are owned by tokens and code.
- **MECE, no repetition** — one distinct idea per point; no overlap between sections. If the same point fits two sections, pick one home and drop the other.
- **Short and plain** — every sentence should be as short as it can be without losing precision. If a word doesn't earn its place, cut it. Applies everywhere in the spec.
- **Plain language before code** — state any rule in plain language first. Code references (prop names, variant values) go in parentheses after, if needed at all. Applies everywhere in the spec.
- **One idea per bullet** — if a sentence contains multiple ideas, split it into separate bullets.
- **Never infer** — every claim in the spec must be confirmed in the component file or codebase grep. If it can't be confirmed, drop it. If it might be worth keeping but can't be verified, move it to the Unverified checklist (see Workflow step 3).

---

## Section Guide

### Overview *(required)*

Three `###` subtitles: **What it is** / **Use when** / **Don't use when**.
One sentence each. What it is: what the component is for — the user need it serves. Not a list of what it renders.
Use when / Don't use when: single sentence if one condition; bullets if 2+.

**Don't use when**: each bullet names a scenario where a different component or pattern is the better fit — name that alternative. Covers structural alternatives too, not just other named primitives. Nothing else belongs here.

---

### Anatomy *(required)*

Table only — no backtick structure line.

| Part | Optional | Purpose |
|---|---|---|

- **Part column**: plain language labels ("cancel button", "icon") — not prop or component names.
- **Purpose column**: user-facing — why the part exists or when to use it. Never implementation details.
- Only include a row if a designer or coding agent would misuse the part without it. Drop self-explanatory rows.
- **Order rows** to match the component's visual reading order: top-to-bottom, left-to-right, row by row as it lays out on screen.

---

### Size *(optional)*

Include only when the component has named sizes with distinct behavioral or layout differences.
Each size: when to use it in context — not dimensions or visual specs.

---

### Type *(optional)*

Include when the component has distinct visual treatments that affect weight or style.
Each type: when to use it.

---

### Variants *(optional)*

Include when the component has named semantic variants.
Each variant: when to use it — no visual descriptions. Visual appearance is owned by tokens. If you find yourself referencing fills, colours, or token names to explain a variant, stop — that content belongs in Implementation or nowhere.

---

### States *(optional)*

Omit entirely if no states have a visible or behavioral difference. Don't list "default" as a placeholder.
List only states with meaningful differences.

---

### Behavior *(optional)*

Use sub-headings as needed. Each sub-heading is optional — include only if there is something non-obvious to document. Purpose level only: describe what the user experiences, not implementation. If a behavior point requires naming a prop or explaining conditional logic, move it to Implementation instead.

**Sub-headings:**
- **Responsive** — how the component adapts at breakpoints
- **Motion** — transition and animation behavior
- **Keyboard** — keyboard navigation and focus management

---

### Best Practices *(required)*

Application rules that prevent misuse. Each rule must answer one of two questions — "how do I assemble this correctly?" or "how do I use this correctly in the UI?" If a rule doesn't answer either, drop it.

- **Composition rules** — correct assembly ("don't nest cards", "action belongs in header, not content")
- **UI usage rules** — correct placement on a screen ("one primary button per page", "don't use as a decorative wrapper")

No design opinions. No implementation instructions. If a rule tells a coding agent what to import or avoid importing, it belongs in Implementation, not here.

---

### Implementation *(optional)*

Coding-agent-specific wiring patterns: state management, import rules, render prop usage, and `className`/token conventions the component itself doesn't enforce. Include when a coding agent would make a wrong decision without it. Don't drop these as inline notes inside other sections.

---

### Props *(optional)*

Include when variant-to-prop mapping isn't obvious from the variants section alone, or when the component is likely applied by a coding agent.

| Prop | Variants | Type | Notes |
|---|---|---|---|

Notes: describe what the prop does for the user or the UI — not just its type.
