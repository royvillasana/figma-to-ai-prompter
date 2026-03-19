# Lovable — Prompt Pattern

Stack: React + Vite + Tailwind + TypeScript. No Vue/Next/Angular/native.
Back-end needs Supabase. Backend code (Python/Node) must go in Supabase Edge Functions.

## First prompt = Blueprint

Open every new Lovable session with this structure (fill in from Figma data):

```
## App: [name]
## Users: [who and their goal]
## Screen: [name — one screen per prompt]
## Stack: React + Vite + TypeScript + Tailwind CSS [+ shadcn/ui if applicable]

## Design tokens
Primary: [hex]  Background: [hex]  Surface: [hex]
Text: [hex]     Border: [hex]      Error: [hex]
Font: [name], body [px], heading [weight]
Radius: [card px] / [button px] / [input px]
Shadow: [value]

## Components
[List each by name, exactly as in Figma layer — real labels, no placeholders]

## Interactions
- [element] → [action with action-word: on click / on submit / on blur / navigates to / opens]

## States
- Loading: [skeleton / spinner location]
- Empty: [message + CTA if any]
- Error: [inline / toast, message style]

## Guardrails
- Do not [X]
- Keep layout as described; do not add unrequested components

## Before building: review this and ask any clarifying questions
```

## Follow-up formula

`On [route/component], [change]. It should [behavior]. Don't touch [X].`
Max 3 changes per follow-up. One follow-up per concern.

## Mobile-first line (always include)

`Responsive, mobile-first. On mobile: [describe layout change — stack, collapse nav, etc.]`

## Token budget

Lovable billing is **credit-based** (1 credit per message, regardless of prompt length). But token
count still affects *quality* — the longer the prompt, the more likely Lovable will lose track of
components, skip states, or hallucinate unrequested elements.

| Range | Behaviour |
|-------|-----------|
| < 600 tokens | Fast, reliable. Good for follow-ups and focused component edits. |
| 600–1,200 | Ideal for a first Blueprint prompt covering one full screen. |
| 1,200–2,000 | Acceptable for complex screens with many components and states. Review output carefully. |
| > 2,000 | Risky — split into 2 prompts: one for layout/tokens, one for interactions/states. |

**Budget note for output:** `~[N] tokens. [Safe / Borderline — consider splitting interactions into a follow-up / Over limit — split this into 2 prompts]`
