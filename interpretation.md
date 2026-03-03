# Reference Interpretation Rules

Treat retrieved GDR references as the canonical source of design truth.

## Non-Negotiable Policy

- Zero creative freedom by default.
- Directly copy the reference output unless a policy-defined exception applies (stack translation or asset handling).
- Do not redesign, reinterpret, or "improve" the reference based on personal judgment.
- If the user requests changes, apply only the requested changes and keep everything else identical to the reference.

## Stack Translation Exception (Always Allowed)

- Always translate the reference implementation to the user's target stack.
- Stack translation is a syntax/runtime adaptation only.
- The semantic result must remain identical to the reference.
- Preserve identical structure, hierarchy, spacing relationships, visual style intent, and interaction/state behavior.

## Asset Handling Policy

This logic applies after the reference has already been fetched and extracted.

- Asset URLs embedded in reference code comments are source references for visual direction.
- Prefer custom assets over direct reuse: use a user-provided asset, an existing workspace analog, or generate a custom asset using the referenced asset as input.
- Keep the asset's semantic role, placement, dimensions, and surrounding structure identical unless the user explicitly asks to change them.
- Directly reuse a referenced asset only if all are true: custom generation is not possible, no good workspace/prompt analog exists, and the asset is not reference-specific.
- Never directly reuse reference-specific assets such as product screenshots, logos, or brand-tied imagery.

### Comment URL location

Reference code may include asset source URLs in comments near where the asset appears in HTML/JSX:

```jsx
<img src="/placeholder.svg" alt="..." />
{/*fetch from https://cdn.example.com/path/to/reference-image.png */}
```

### Post-fetch decision order

1. For each asset slot, first use a user-provided asset or a strong analog already present in the user's workspace/prompt.
2. Otherwise, generate a custom asset using the referenced asset URL as image-generation input (if image generation is available).
3. Use the referenced asset directly only as strict fallback when all required conditions are met.

### Strict direct-use fallback conditions

Use the referenced asset directly only when all conditions are true:

1. A custom replacement cannot be generated.
2. No good analog exists in the user's workspace or prompt.
3. The asset is not specific to the original reference.

Examples:

- Usually acceptable for direct use: generic gradient backgrounds, abstract textures, non-specific decorative shapes.
- Not acceptable for direct use: product screenshots, brand-specific imagery, logos, or content clearly tied to the reference product.

### Post-fetch asset preparation

Keep all downloaded and generated assets inside `.references/assets/` so task cleanup removes them.

```bash
mkdir -p .references/assets

# Extract unique image URLs from reference snippets.
rg -h -o "https?://[^[:space:]\"')>]+\\.(png|jpe?g|webp|gif|svg|avif)(\\?[^[:space:]\"')>]*)?" .references/ref-*.txt \
  | sort -u > .references/asset-urls.txt

# Download each asset for reference analysis and optional image-generation input.
while IFS= read -r url; do
  file="$(basename "${url%%\?*}")"
  curl -L "$url" -o ".references/assets/$file"
done < .references/asset-urls.txt
```

## Prohibited Without Explicit User Request

- Re-styling, re-theming, or token remapping.
- Re-layout, re-composition, or structural refactors.
- Mixing/remixing multiple references.
- Any decorative additions or subjective "enhancements."

## Output Requirements

- State which reference was copied.
- Confirm whether only stack translation was applied.
- If an asset-handling exception was used, explain which rule triggered it.
- If any other non-translation differences exist, cite the exact user instruction that required them.