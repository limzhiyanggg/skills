# spec-component

A Claude Code skill that documents a UI component as a design spec, readable by both designers and AI agents.

---

## What it does

Reads your component file, searches how it's used across the codebase, drafts a spec in chat, waits for your approval, then saves it to your docs folder.

The spec covers what the component is for, when to use it, its anatomy, variants, states, behavior, and best practices.

---

## Requirements

- A codebase with component files at a known path
- A docs folder to save specs into (will be created if it doesn't exist)

---

## How to use

```
/spec-component Badge
```

Pass the component name as an argument.

---

## How it works

1. **Checks config:** Asks for the component file path, search scope, and output folder if not already set (`component_paths`, `grep_scope`, `output_path`).
2. **Reads the component file:** Finds and reads the component to understand its variants, props, and structure.
3. **Searches codebase usage:** Scans the source folder to find every place the component is used.
4. **Drafts the spec in chat:** Writes the full spec and flags anything it couldn't confirm from the code.
5. **Waits for approval:** You review, request changes, and sign off before anything is saved.
6. **Saves:** Writes the final spec to your docs folder (`output_path/[component-name].md`).

---

## What you get

A markdown file saved to `docs/design-system/components/[component-name].md`.

```markdown
# Badge

## Overview
### What it is
### Use when
### Don't use when

## Anatomy
| Part | Optional | Purpose |

## Variants
## States
## Behavior
## Best Practices
## Implementation
## Props
```

Sections are included only when there is something non-obvious to document. Empty sections are dropped.

---

## Config

Set these once per project inside `SKILL.md`:

| Key | Default | What it is |
|-----|---------|------------|
| `component_paths` | `src/components/ui/[component].tsx` | Where to find the component file |
| `grep_scope` | `src/` | Where to search for usage |
| `output_path` | `docs/design-system/components/` | Where to save the spec |

Not sure what to put? Ask Claude to look at your project structure and suggest the right values.

---

## Created by

Lim Zhiyang
