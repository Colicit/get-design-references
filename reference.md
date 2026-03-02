# GDR CLI Reference

Default API Base URL: `https://gdr.colicit.com`

The CLI uses this base URL by default. Override with `--base-url <url>` when needed.

## Setup

```bash
# Install once (fastest repeated usage)
npm install -g @colicit/gdr-cli@latest

# Or run ad-hoc without global install
npx -y -p @colicit/gdr-cli@latest gdr --help
```

You can use this helper so examples work whether `gdr` is globally installed or not:

```bash
run_gdr() {
  if command -v gdr >/dev/null 2>&1; then
    gdr "$@"
  else
    npx -y -p @colicit/gdr-cli@latest gdr "$@"
  fi
}
```

## Authentication

CLI key resolution order:

1. `GET_DESIGN_REFERENCES_API_KEY` in environment
2. `.env.local` in the project directory
3. `.env` in the project directory

Typical local setup:

```
GET_DESIGN_REFERENCES_API_KEY=<your-key>
```

Run from the project root (or pass `--project-dir <path>`), then verify:

```bash
run_gdr auth doctor --json
run_gdr auth status --json
```

All examples below use `--json` for deterministic machine-readable output.

## Ephemeral `.references` workspace

For each agent run, create a temporary project-root workspace for reusable references and delete it when done.
Use the same pattern for any `gdr` command by redirecting output to `.references/<name>.json`.

```bash
# Always start clean
rm -rf .references
mkdir -p .references

# Auto-cleanup on shell exit/interruption
trap 'rm -rf .references' EXIT INT TERM HUP
```

### Save and reuse reference code as `.txt`

```bash
run_gdr views search \
  --functional "analytics dashboard with charts and KPI cards" \
  --style "dark theme, neon accents, data-dense" \
  --n 2 \
  --json > .references/results.json

# Extract each returned `code` field into `.txt` files.
node -e 'const fs=require("fs");const data=JSON.parse(fs.readFileSync(".references/results.json","utf8"));data.forEach((item,i)=>{if(item&&typeof item.code==="string"&&item.code.trim())fs.writeFileSync(`.references/ref-${i}.txt`,item.code);});'

# Reuse during task:
# .references/ref-0.txt
# .references/ref-1.txt

# Optional explicit cleanup at the end (trap also handles this):
rm -rf .references
```

Use `.txt` for extracted snippets to avoid TypeScript/ESLint/import-resolution noise from `.tsx` temporary files.

---

## primitives

Search for atomic UI components (buttons, inputs, toggles, etc.).

### Semantic search

```bash
run_gdr primitives search \
  --functional "date picker" \
  --style "minimal, monochrome, sharp corners" \
  --n 2 \
  --json
```

**Input options:**

| Option         | Required | Notes                                          |
|----------------|----------|------------------------------------------------|
| `--functional` | yes      | What the component is. Max 1000 chars.         |
| `--style`      | yes      | Visual style keywords. Max 1000 chars.         |
| `--n`          | no       | Results to return, clamped to 1-3 (default 1)  |

### ID lookup

```bash
run_gdr primitives get --id "<uuid>" --json
```

### Response

```json
[
  {
    "images": [
      { "image_url": "https://...", "caption": "Default state" }
    ],
    "code": "<React + Tailwind source>",
    "collection_id": "<uuid>"
  }
]
```

---

## patterns

Search for compositional UI patterns (cards, sidebars, navbars, etc.).

Request and response structure is identical to `primitives`.

```bash
run_gdr patterns search \
  --functional "pricing card with feature list" \
  --style "glassmorphism, soft shadows, rounded" \
  --n 1 \
  --json
```

---

## views

Search for complete layouts (hero sections, dashboards, settings pages, etc.).

Request and response structure is identical to `primitives`.

```bash
run_gdr views search \
  --functional "analytics dashboard with charts and KPI cards" \
  --style "dark theme, neon accents, data-dense" \
  --n 1 \
  --json
```

---

## design-systems

Search for design system tokens (color palettes, font pairings, spacing scales).

### Semantic search

```bash
run_gdr design-systems search \
  --style "warm, earthy tones with serif typography" \
  --n 1 \
  --json
```

**Input options:**

| Option    | Required | Notes                                          |
|-----------|----------|------------------------------------------------|
| `--style` | yes      | Visual style keywords. Max 1000 chars.         |
| `--n`     | no       | Results to return, clamped to 1-3 (default 1)  |

Note: design systems have no `functional_description` search field.

### ID lookup

```bash
run_gdr design-systems get --id "<uuid>" --json
```

### Response

```json
[
  {
    "tokens": "<design system values in markdown>",
    "collection_id": "<uuid>"
  }
]
```

---

## collections

List all artifacts that belong to a collection. Useful after finding a result and exploring everything in the same source collection.

```bash
run_gdr collections get --collection-id "<uuid>" --json
```

### Response

```json
[
  {
    "type": "primitive",
    "id": "<uuid>",
    "functional_description": "toggle switch",
    "style_description": "minimal, rounded"
  },
  {
    "type": "design_system",
    "id": "<uuid>",
    "functional_description": "",
    "style_description": "neutral palette, geometric sans-serif"
  }
]
```

---

## Raw API fallback

If needed, you can still call the HTTP API directly at:

- `/v1/primitives`
- `/v1/patterns`
- `/v1/views`
- `/v1/design_systems`
- `/v1/collections`

Use `Authorization: Bearer <GET_DESIGN_REFERENCES_API_KEY>` and `Content-Type: application/json`.
