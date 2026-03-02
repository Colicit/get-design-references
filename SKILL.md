---
name: get-design-references
description: Search and retrieve highly curated frontend design references. Use this skill when creating or refactoring components, pages, or design systems.
---

# Get Design References (GDR)

Semantically search Colicit's curated database of frontend design references. Returns React + Tailwind code snippets and labelled images.

## Artifact Types

- **Primitives** — Atomic components (buttons, inputs, toggles, date pickers).
- **Patterns** — Compositions of primitives (card, sidebar, navbar).
- **Views** — Complete layouts (hero section, pricing page, analytics dashboard).
- **Design Systems** — Cohesive token sets (color palettes, font pairings, spacing scales).

Artifacts from the same source share a `collection_id`.

## Search Modes

1. **Semantic search** — Describe what you need by function and style. Returns the closest matches ranked by similarity.
2. **ID lookup** — Fetch a specific artifact by its UUID.

## Setup

Use the GDR CLI:

```bash
# Install once (fastest repeated usage)
npm install -g @colicit/gdr-cli@latest

# Or run ad-hoc without global install
npx -y -p @colicit/gdr-cli@latest gdr --help
```

## Authentication

The CLI reads API keys in this order:

1. `GET_DESIGN_REFERENCES_API_KEY` in environment
2. `.env.local` in the project directory
3. `.env` in the project directory

For local projects, users typically store the key in `.env.local`:

```
GET_DESIGN_REFERENCES_API_KEY=<your-key>
```

Before any search/get command, verify auth:

```bash
gdr auth doctor --json
gdr auth status --json
```

If auth is unresolved, stop and ask the user to set `GET_DESIGN_REFERENCES_API_KEY` in environment or add it to `.env.local`/`.env`. They can go to `https://www.colicit.com` to get their API key. Do not extract secrets with `grep`/`cut` or print them to logs.

## Workflow

1. Ensure `gdr` CLI is available (`gdr --help` or use `npx -y -p @colicit/gdr-cli@latest gdr ...`).
2. Run from the project root (or pass `--project-dir <path>`), then run `gdr auth status --json`.
3. Read [reference.md](reference.md) for command examples.
4. Identify the artifact type that matches the need (primitive, pattern, view, or design system).
5. Craft a concise `functional_description` (what it is / what it does) and `style_description` (how it looks).
6. Run the relevant `gdr ... --json` command.
7. Read [interpretation.md](interpretation.md) and apply its fidelity-first approach to reference usage.
8. Optionally, use `gdr collections get --collection-id <uuid> --json` to discover related artifacts from the same collection.