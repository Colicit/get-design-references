# Reference Interpretation Rules

Treat retrieved GDR references as the canonical source of design truth.

## Non-Negotiable Policy

- Zero creative freedom by default.
- Directly copy the reference output unless the user explicitly asks for changes.
- Do not redesign, reinterpret, or "improve" the reference based on personal judgment.
- If the user requests changes, apply only the requested changes and keep everything else identical to the reference.

## Stack Translation Exception (Always Allowed)

- Always translate the reference implementation to the user's target stack.
- Stack translation is a syntax/runtime adaptation only.
- The semantic result must remain identical to the reference.
- Preserve identical structure, hierarchy, spacing relationships, visual style intent, and interaction/state behavior.

## Prohibited Without Explicit User Request

- Re-styling, re-theming, or token remapping.
- Re-layout, re-composition, or structural refactors.
- Mixing/remixing multiple references.
- Any decorative additions or subjective "enhancements."

## Output Requirements

- State which reference was copied.
- Confirm that only stack translation was applied.
- If any non-translation differences exist, cite the exact user instruction that required them.