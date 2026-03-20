# Figma Make — Prompt Pattern

Claude-powered. Generates interactive HTML prototypes (not production code). Best for stakeholder
demos and interaction validation.

## Visual context — MCP text block (preferred, no frame attachment needed)

When the Figma MCP is available, build the visual context as structured text directly in the prompt.
This replaces frame attachment and saves ~300–500 hidden tokens while giving Make *more precise*
context than a visual, because it's explicit rather than interpreted.

Use the MCP data from `get_design_context`, `get_variable_defs`, and `get_screenshot` to fill this
block. Include it at the top of the prompt, before the interaction spec:

```
## Screen: [Frame name] — [width]×[height]px — [platform: web/mobile]
Purpose: [1 sentence — what the user does here]

Layout: [flex-col / flex-row / grid], gap [value], padding [value]
Background: [hex]

Components (in visual order, top → bottom):
- [layer-name] ([type: nav / card / form / table / modal / etc.])
    size: [w×h or full-width], position: [fixed/sticky/relative]
    bg: [hex], radius: [px], shadow: [value]
    contents: [child elements — labels, icons, inputs, values]

- [layer-name] ([type])
    [same pattern]

Typography:
- Headings: [font], [size/weight], [hex]
- Body: [font], [size/weight], [hex]
- Labels/captions: [font], [size/weight], [hex]

Design tokens (only those used on this screen):
[token/name → #hex]  [token/name → #hex]  ...
Radius: [card px] / [button px]  Shadow: [value]  Spacing base: 4px

[If no design system file: list raw hex values as before]
[If design system provided: use "token/name → #hex" format — semantic name + resolved value]
```

Only fall back to "paste the Figma frame" if the MCP connection is unavailable or the design is
too visually complex to describe accurately in text (e.g. heavy illustration, complex data viz).

## Full prompt structure

```
You are a senior [role] building [app type] for [user type].

[Visual context block above — generated from MCP]

Interactions:
- on click [element] → [action]
- on hover [element] → [CSS change]
- on submit [form] → validate [fields], show [success/error]
- clicking [tab] → replaces content, URL updates to [path]
- on scroll → [behavior]

States: Loading: [what shows]  Empty: [what shows]  Error: [what shows]

Responsive: on mobile [layout change — stack, collapse nav, etc.]

Do not: [guardrail 1] / [guardrail 2]
```

## Interaction action-words (use these — Figma Make understands them reliably)

`on click` · `on hover` · `on submit` · `on focus` · `navigates to [path]` · `opens a modal containing` · `slides in a drawer from the [left/right]` · `reveals a dropdown` · `shows a [success/error] toast at [position]` · `expands to show` · `clicking [Tab] replaces the content area`

## Progressive prompting (for complex screens)

Split into follow-ups: Prompt 1 = layout · Prompt 2 = interactions · Prompt 3 = states · Prompt 4 = responsive polish

## Refinement formula

Replace vague requests with exact values:
- ❌ "bigger button" → ✅ "button padding 12px 24px, font-size 16px"
- ❌ "more modern" → ✅ "8px radius, shadow 0 2px 8px rgba(0,0,0,0.08), 32px card padding"

## Token budget

Figma Make is bundled in the **Figma subscription** — no separate token billing. However, it runs
on Claude with a limited context window per session.

Using the MCP text block approach (no frame attachment) saves 300–500 hidden tokens vs. pasting a
frame, and shifts those saved tokens toward more precise interaction and state descriptions.

| Range (text prompt only) | Behaviour |
|--------------------------|-----------|
| < 500 tokens | Good for single-component or single-interaction refinements. |
| 500–1,000 | Ideal for one full screen with interactions and states. |
| 1,000–1,500 | Acceptable for dense screens. Use progressive prompting if over this. |
| > 1,500 | Split across the 4-prompt strategy: layout → interactions → states → polish. |

If the user must paste a Figma frame instead: treat it as +300–500 hidden tokens on top of the
text. Subtract that from the ranges above to stay within the same quality thresholds.

**Budget note for output:** `~[N] tokens (MCP text context — no frame attachment needed[, +~[X] tokens for [N] design system tokens]). [Safe / Use progressive prompting — split into layout first then interactions]`

## Output validation — correction prompt syntax

When reviewing a Make output URL against the Figma design, write correction prompts using these
patterns — one issue per prompt:

| Issue type | Correction prompt pattern |
|-----------|--------------------------|
| Wrong color | `Set the [element] background to #[hex]` |
| Missing component | `Add a [component name] above/below [reference element] — [brief description]` |
| Wrong spacing | `Set padding on [element] to [value]. Set gap between [a] and [b] to [value]` |
| Wrong typography | `Change [element] font-size to [px], font-weight to [value], color to #[hex]` |
| Layout shift | `Change [element] layout to [flex-row/grid/etc], align items [value]` |
| Non-functional interaction | `on click [element] → [action using Make action-word syntax]` |
| Wrong border radius | `Set border-radius on [element] to [value]` |

Always reference the element by its visible label or layer name — not by position ("the button"
is ambiguous; "the Save Changes button" is not).
