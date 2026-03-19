# Pencil.dev — Prompt Pattern

Design-to-code fidelity tool. Native MCP integration reads exact pixel values. Built-in support
for shadcn/ui, Tailwind, Lunaris, Halo, Nitro. Rename Figma layers semantically before prompting
(not "Frame 42" — use "pricing-card-container") — Pencil uses layer names as CSS anchors.

## Structure

```
Inspect the selected frame: [Frame Name].

Generate a [React/HTML] component: [ComponentName].tsx
Library: [Tailwind CSS / shadcn/ui + Tailwind / CSS Modules]

Read exact values from these layers:
- [layer-name]: padding [?], margin [?], radius [?], shadow [?]
- [layer-name]: font-size [?], font-weight [?], line-height [?], color [?]
(Read all; do not approximate)

Token mapping:
- [hex] → [Tailwind class, e.g. bg-indigo-500]
- [px] → [Tailwind class, e.g. rounded-xl, p-6, gap-4]

Layout:
- [flex-row / flex-col / grid], justify: [value], align: [value]
- Mobile (<[bp]px): [change]  Desktop (>[bp]px): [change]

Semantic CSS classes (from layer names):
- [layer-name] → .[css-class]

Interactions (code-level):
- [layer: "name"]: onClick → [handler / state setter]
- [layer: "name"]: onChange → [state update]
- [layer: "name"]: hover → [CSS transition: property value duration]

[If shadcn: "Map to shadcn: search → <Input>, dropdown → <Select>, modal → <Dialog>, buttons → <Button variant=[...]>"]

Output:
- File: components/[Name]/index.tsx
- TypeScript interface for all props at top
- Semantic HTML (button, nav, form, section — not just div)
- Named + default export
```

## MCP inspection prompt (when using Claude + Pencil MCP)

`Inspect the selected frame. Read exact padding, margin, font-weight, line-height, border-radius, shadow, and color from every layer. Generate a React + Tailwind component that matches the layout precisely — do not approximate any value.`

## Token budget

Pencil.dev uses its own **credit system** per generation. Your text prompt is lean — the real token
cost comes from the MCP layer reading design data on Pencil's backend. Every layer you reference by
name triggers an inspection call; more layers = more backend tokens consumed.

| Range | Behaviour |
|-------|-----------|
| < 400 tokens | Fast, low-cost. Best for single components with few layers. |
| 400–800 | Ideal for a full screen component. Layer names do the heavy lifting. |
| 800–1,500 | Complex components with many variants or interactions. Expect slower generation. |
| > 1,500 | Split by component: generate the card separately from the table, etc. |

Design tip: lean on semantic layer names in your prompt rather than describing every value in text.
"Read exact values from `pricing-card-pro`" is cheaper than manually listing every CSS property.

**Budget note for output:** `~[N] tokens. [Fast/cheap — layer names carry the context / Medium — layer inspection will add backend overhead / High — split into [X] component prompts]`
