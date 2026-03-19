# Paper.design — Prompt Pattern

Agent-first, bidirectional design tool. Canvas is HTML/CSS natively — what you design is code.
MCP has 24 tools for read+write access. Best for design-token ↔ code sync workflows and
design-system component generation. Think agent instructions, not just design briefs.

## Structure

```
Task: [what the agent should do — read/build/sync/generate variants]
Canvas layer: [Dashboard / Main Content / Component Name]

Design tokens (sync from codebase or define here):
--color-primary: [hex]        --color-background: [hex]
--color-surface: [hex]        --color-text: [hex]
--color-border: [hex]         --color-error: [hex]
--font-family: [value]        --font-size-body: [value]
--spacing-sm/md/lg: [values]  --radius: [value]

Structure: [describe using semantic HTML: section, article, nav, header, main, aside]

Interactions (CSS states + JS events):
- :hover [element] → [CSS change, transition duration]
- click [element] → [add class / toggle state / navigate]
- [form] submit → [action]

[If real data: "Populate [layer] from [source]. Fields: [f1], [f2]. Include 1 edge case: [long text / empty field]."]

Agent actions:
- [Sync tokens to /src/styles/tokens.css]
- [Generate N variants: default, hover, active, disabled]
- [Export as standalone HTML/CSS to /src/components/[name]/]

Guardrails: [what not to change]
```

## Token sync prompts

Pull: `Read tokens from /src/styles/tokens.css and apply to all layers in [Frame].`
Push: `Export color + type values from [Frame] as CSS custom properties → /src/styles/[name].css`

## Variant generation

`From [ComponentName], generate 4 variants: default / hover / active / disabled.
Organize as [ComponentName]/[variant]. Keep dimensions and token refs in sync — only state differs.`

## Token budget

Paper.design is **agent-driven** — your text prompt triggers a chain of MCP tool calls (read canvas,
write layers, sync tokens, export code). Each tool call has its own token overhead on their backend.
A vague 200-token prompt can cost more than a precise 800-token one, because vagueness forces the
agent to make extra clarification calls.

| Prompt range | Agent behaviour | Actual cost |
|-------------|----------------|-------------|
| < 300 tokens | Agent makes exploratory calls to understand intent | Higher total cost due to extra tool calls |
| 300–700 | Precise enough for the agent to act directly — sweet spot | Lowest total cost |
| 700–1,200 | Detailed builds/syncs; agent reads fewer layers because you specified them | Medium cost |
| > 1,200 | Agent may exceed session context — split into task + agent actions | Risk of incomplete execution |

Key insight: **every vague word = extra agent tool calls = more tokens**. "Add spacing" triggers the
agent to check existing spacing values before acting. "Set padding to 24px on `card-container`" is
one direct tool call. Be precise to save tokens.

**Budget note for output:** `~[N] tokens. [Precise — agent will act directly, low tool-call overhead / Some ambiguity — agent may probe canvas first, medium overhead / Vague sections detected — resolving them before pasting will save significant token cost]`
