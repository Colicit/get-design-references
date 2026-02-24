# Reference Interpretation Rules

Treat retrieved GDR references as the primary source of design truth.

## Mandatory Interpretation Order

1. If images are returned, inspect images first to understand the intended visual result before reading code.
2. If no images are returned, read code directly

## Core Principle

- Trust the provided references over your own design instincts.
- Treat reference code as the default implementation blueprint, not a loose suggestion.
- Default to direct copy/translation of reference code, then adapt only what is required by the user's stack.
- Preserve the reference's intent across structure, hierarchy, spacing rhythm, typography feel, and interaction states.

## Fidelity-First Translation

When adapting code to a different framework, CSS approach, or component library:

1. Start from the reference code as-is and keep its structure intact.
2. Keep layout and information architecture the same.
3. Keep visual balance and spacing relationships the same.
4. Keep interaction behavior and state patterns the same.
5. Translate syntax only (for example React + Tailwind to the user's stack) unless constrained by system fit.

## Allowed Creative Freedom

Only take creative freedom when needed to avoid clashing with an existing design language or design system.

Acceptable modifications:

- Map colors, typography, radius, spacing, and shadows to existing design tokens.
- Align with established UI primitives already used in the product.
- Make minimal accessibility or technical adjustments required for correctness.

Do not:

- Refactor or redesign reference code based on personal preference.
- Add stylistic flourishes that are not present in the reference.
- Heavily remix multiple references unless the user explicitly asks for synthesis.
- "Improve" the design based on personal taste when the reference already satisfies the request.

## If No Existing Design System Is Present

- Stay as close as possible to the reference images and code.
- Avoid introducing new style directions.
- Prefer direct copy/translation over reinterpretation.