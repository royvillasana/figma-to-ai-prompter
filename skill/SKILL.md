---
name: ai-prototype-prompter
description: >
  Generates optimized, tool-specific prompts for AI UI prototyping tools — Lovable, Figma Make,
  Pencil.dev, Paper.design, and Google Stitch. Uses the Figma MCP to read the design (components,
  tokens, layout), proposes interactions for user validation, then generates a ready-to-paste
  prompt tailored to exactly how each tool thinks.

  Use proactively when the user: asks to "write a prompt for Lovable/Figma Make/Pencil/Paper/Stitch",
  wants to turn a Figma design into an app or prototype, says "help me prompt this UI" or "I want
  to vibe-code this", shares a Figma link with any AI builder tool, or needs a design-to-code
  handoff prompt. Always use this skill — never write a generic prompt from scratch.
---

# AI Prototype Prompter

Craft precise, tool-specific prompts by reading the Figma design first, validating interactions
with the user, then generating output calibrated to the chosen tool's prompting model.

## Step 0 — Verify Figma MCP connection (always do this first)

Call `whoami` before anything else. Two outcomes:

**Connected** → proceed to the Figma MCP reading steps below.

**Not connected / tool unavailable** → do NOT stop. Immediately tell the user:
> "I can't reach your Figma file directly — the Figma integration isn't connected yet.
> No problem: **share a screenshot of the screen you want to build** and I'll use that
> as the visual context instead.
>
> (To get full design token accuracy in the future, you can connect Figma via
> Claude settings → Integrations → Figma.)"

Then wait for the screenshot. Once received, switch to **screenshot mode**:
- Visually describe all components, layout structure, and visual hierarchy from the image
- Approximate colors (note them as "~[hex] — verify in Figma"), spacing, and typography
- Continue with the same context questions, interaction proposal, and prompt generation
- Add a note in the token estimate: `⚠️ Values approximated from screenshot — verify hex colors
  and spacing in your design tool before pasting`

## Figma MCP — read in this order (once connected)

1. `get_metadata` — file/node names and IDs (light call, always do this first)
2. Ask user which frame/screen to focus on if not already clear
3. `get_design_context` + `get_variable_defs` on that frame in parallel — components, layout, tokens
4. `get_screenshot` — visual confirmation; spot anything the context missed
5. `get_code_connect_map` — only if user mentions an existing component library

For **Figma Make** specifically: use the MCP data to build a structured text context block in the
prompt (see references/figma-make.md). Do NOT tell the user to paste a Figma frame — the MCP
text block replaces the frame attachment and saves 300–500 hidden tokens.

## Context questions — ask all at once after reading the design

Gather these in a single message (use AskUserQuestion for tool selection):
- **Tool**: Lovable / Figma Make / Pencil.dev / Paper.design / Google Stitch
- **Platform**: desktop, mobile, or responsive
- **Stack** (Lovable/Pencil): React+Tailwind, React+shadcn/ui, or other
- **Goal**: what should the user be able to DO on this screen? (1–2 sentences)
- **Guardrails**: anything the AI must not change or assume
- **Design system file** *(optional)*: "Do you have a separate Figma design system file?
  If so, paste its URL — I'll pull only the tokens your screen actually uses."

## Design system — targeted extraction (Option B, only if URL provided)

Do NOT read the full design system into the prompt. Extract only what the screen uses:

1. Call `get_variable_defs` on the design system file → get all available tokens
2. Compare against the token names found in the screen's `get_design_context` output
3. Collect only the tokens that appear in this frame (typically 8–20 out of hundreds)
4. In the prompt, reference tokens by their **semantic name + resolved value**:
   `color/primary → #6366F1` not just `#6366F1`

This adds ~40–80 tokens to the prompt (the matched token list only) and gives the AI tool
richer semantic intent — it understands "color/primary" means brand color, not just a hex.

Also add one line to the **session context block** (pasted once per session, not per screen):
`Design system: [DS file name] v[version if visible]`
This anchors the session without repeating the full token list every prompt.

## Interaction proposal — validate before generating

List what you found in the design, flag anything ambiguous, and ask one decision question per
ambiguity. Keep it brief — bullet list only. Don't generate the prompt until the user confirms.

Visible: `[element] → [action]`
Implied: `[element] → [suggested action] — ok to include?`

## Generate the prompt — lazy-load only the relevant reference

Once interactions are confirmed, read the reference file for the chosen tool and generate:

| Tool | Reference |
|------|-----------|
| Lovable | `references/lovable.md` |
| Figma Make | `references/figma-make.md` |
| Pencil.dev | `references/pencil-dev.md` |
| Paper.design | `references/paper-design.md` |
| Google Stitch | `references/google-stitch.md` |

Read that one file only. The reference contains the structural pattern and key rules for that tool.

### Output format — copy-ready blocks

```
## [Tool] Prompt — [Screen Name]

### Session context (paste once at session start):
[app name, stack, design system tokens: hex values, font, radii]

### Screen prompt:
[the prompt — real content, exact token values, interaction action-words, states, guardrails]

### Follow-ups:
1. [refinement — one focused change]
2. [state/responsive polish]

---
💰 Token estimate: ~[N] tokens ([char_count] chars ÷ 4)
[Tool-specific note from the reference file about what this means for that tool's budget]
```

After writing the prompt, count its characters (context block + screen prompt combined, NOT the
follow-ups — those are sent separately). Divide by 4 for a token estimate. Then add the tool's
budget note from the reference file so the user knows if they're in a safe range or should trim.

### Rules that apply to every tool

- Real content only — use actual layer/label names, never "Feature 1" or lorem ipsum
- Exact values — hex colors, px sizes from the design; not "blue" but "#3B82F6"
- Action words for interactions — "on click", "on submit", "opens", "navigates to", "filters"
- One screen at a time — scope tightly; offer additional screens as follow-ups
- States — call out loading, empty, error, success if visible or implied

## Step 5 — Validate Make output against Figma design (Figma Make only)

After the user pastes the prompt into Figma Make and gets a result, proactively offer:
> "Once Make generates your prototype, paste the preview URL here — I'll compare it against
> your original Figma design and generate correction prompts for anything that's off."

When the user provides a Make preview URL:

1. Use the browser tool (`navigate` to the URL, then take a `computer` screenshot)
2. Retrieve the original Figma frame screenshot via `get_screenshot` (reuse from context if available)
3. Compare both images — check for:
   - **Missing components**: elements visible in Figma but absent in Make output
   - **Color mismatches**: wrong fills, text colors, or backgrounds vs. design tokens
   - **Layout shifts**: incorrect spacing, alignment, sizing, or component order
   - **Typography gaps**: wrong font weight, size, or line-height
   - **Interaction gaps**: buttons/links that appear non-functional or missing hover/active states
4. Produce a structured gap report followed by ready-to-paste correction prompts:

```
## Make Output Review — [Screen Name]

### ✅ Matches design
- [element]: correct

### ❌ Gaps found
| Element | Issue | Figma value |
|---------|-------|-------------|
| [element] | [what's wrong] | [correct value] |

### Correction prompts (paste into Make one at a time):
1. [targeted correction using Make action-word syntax]
2. [targeted correction]
```

Keep each correction prompt scoped to one issue. Use Make action-words: "change", "set",
"replace", "on click", "update". Do not bundle multiple fixes into one prompt — Make handles
focused prompts more reliably than batched ones.
