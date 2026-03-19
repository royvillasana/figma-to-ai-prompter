# Google Stitch — Prompt Pattern

Gemini 2.5-powered AI UI design tool from Google Labs. Generates responsive UI designs from text,
sketches, or screenshots — output pastes directly into Figma or exports as HTML/CSS. NOT
Claude-powered; uses Gemini, so prompting style differs from Figma Make or Lovable.

Two modes:
- **Standard** (Gemini 2.5 Flash): up to 350 generations/month — use for iteration and exploration
- **Experimental** (Gemini 2.5 Pro): up to 50 generations/month — use for final, high-fidelity screens

## Core prompting philosophy

Stitch responds best to **adjective-rich, layered descriptions** — mood and brand personality first,
then structure and content. Gemini infers visual decisions from intent, so "minimal, professional,
generous whitespace" does more work than a list of hex values. That said, specific values always
override inference — use both.

Stitch also supports:
- **Image input**: upload the Figma screenshot from the MCP to give visual grounding. Unlike Figma
  Make, attaching an image in Stitch does NOT cost hidden tokens — generations are flat-rate.
- **DESIGN.md**: a markdown file describing your design system that Stitch can ingest natively
- **URL import**: point Stitch at a live URL and it will extract the design system from the site

## Prompt structure

```
[Platform: Web / Mobile / Responsive] — [Screen name]
[1-sentence purpose of this screen]

Style: [3–5 adjectives describing mood/brand — e.g. minimal, editorial, warm, high-contrast]
Theme: [Light / Dark / System]

Design tokens:
Background: [hex]  Primary: [hex]  Accent: [hex]  Text: [hex]  Border: [hex]
Font: [family] — [heading weight/size], [body weight/size]
Radius: [value]  Spacing: [base unit]

Screen structure (top → bottom):
- [section name]: [what it contains and its purpose]
- [section name]: [what it contains]
- [section name]: [what it contains]

Components:
- [component name]: [description — labels, values, states, real content]

Interactions:
- [element]: [behavior on click/hover/tap]
- [element]: [behavior]

States: [loading / empty / error descriptions]

[If using design system file: "Match the design system defined in [DESIGN.md / URL]"]

Do not: [guardrails]
```

## Style adjectives — the highest-leverage part of a Stitch prompt

Stitch infers color temperature, spacing density, and type choices from adjectives. Use them
deliberately — they override Gemini's defaults:

Brand moods → `minimal` · `bold` · `editorial` · `playful` · `clinical` · `warm` · `luxurious`
Density → `spacious` · `compact` · `content-dense` · `generous whitespace`
Visual weight → `flat` · `elevated` · `glassmorphic` · `outlined` · `filled`
Platform feel → `native iOS` · `Material 3` · `web app` · `dashboard`

## Design system injection (Stitch-native approach)

Stitch has a unique DESIGN.md format — a short markdown file describing your system. If the user
has a design system Figma file, translate the MCP token extraction into a DESIGN.md block and
include it in the prompt:

```
## DESIGN.md
Primary: [hex] — used for CTAs, active states, links
Background: [hex] — page background
Surface: [hex] — cards, modals, sidebars
Text/primary: [hex] — headings and body
Text/secondary: [hex] — labels, captions, placeholders
Border: [hex] — dividers, input outlines
Font: [family] — [heading spec] / [body spec]
Radius: [value] (cards) / [value] (buttons) / [value] (inputs)
Spacing scale: [base]px base unit
```

Alternatively: if the brand has a live public site, tell Stitch `Extract design system from [URL]`
and it will infer tokens directly — zero prompt tokens spent on design system description.

## Using the Figma MCP screenshot as image input

Unlike Figma Make (where frame attachment costs hidden tokens), Stitch charges a flat 1 generation
per prompt regardless of whether an image is attached. So always include the screenshot from
`get_screenshot` as a visual reference — it gives Stitch layout grounding at no extra cost.

Tell Stitch: `Use the attached screenshot as the layout reference. Match the component structure
and visual hierarchy exactly. Apply the design tokens above for all colors, fonts, and spacing.`

## Incremental prompting strategy

Stitch works best screen-by-screen, component-by-component — never the full app at once:

1. **First prompt**: layout + style + key components (1 generation)
2. **Edit mode**: targeted refinements — "Make the CTA button full-width on mobile" (1 generation each)
3. **Component focus**: zoom into a single card or form for detail work (1 generation)

In Experimental mode (50 gen/month), treat each generation as high-value — front-load detail in
the first prompt to minimize follow-up generations.

## Token budget

Stitch billing is **generation-based**, not token-based. Each submitted prompt = 1 generation,
regardless of prompt length or image attachments. There is no per-token cost to the user.

| Mode | Budget | Strategy |
|------|--------|----------|
| Standard (Flash) | 350 gen/month | Iterate freely. Use shorter prompts and refine. |
| Experimental (Pro) | 50 gen/month | Front-load all detail in the first prompt. Minimize follow-ups. |

Because cost is per-generation (not per-token), **longer, more detailed prompts are free** — a
500-token prompt costs exactly as much as a 50-token one. Invest the saved budget in richer style
adjectives, more precise component descriptions, and explicit state definitions.

**Budget note for output:** `1 generation ([mode: Standard / Experimental]). [Standard — iterate freely, this prompt length is fine / Experimental — front-loaded well, minimal follow-up generations needed / Experimental — consider adding more detail here to avoid a follow-up generation]`
