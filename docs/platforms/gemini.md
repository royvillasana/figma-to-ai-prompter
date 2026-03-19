# Installing on Gemini (Google AI Studio)

Gemini doesn't support `.skill` files natively. Use the skill as a **system instruction** in Google AI Studio or Gemini Advanced.

> ⚠️ Gemini has no Figma MCP access. The skill runs in **screenshot mode** — upload a screenshot of your Figma screen when prompted.

---

## Option A — Google AI Studio (free, recommended for developers)

Google AI Studio lets you set a system prompt and save it as a reusable "tuned model" or prompt template.

### Setup

1. Go to [aistudio.google.com](https://aistudio.google.com)
2. Click **Create new prompt** → **System instructions**
3. Paste the contents of [`skill/SKILL.md`](../../skill/SKILL.md)
4. Add a separator and paste the reference file for the tool(s) you use most:
   - [`skill/references/lovable.md`](../../skill/references/lovable.md)
   - [`skill/references/figma-make.md`](../../skill/references/figma-make.md)
   - [`skill/references/pencil-dev.md`](../../skill/references/pencil-dev.md)
   - [`skill/references/paper-design.md`](../../skill/references/paper-design.md)
   - [`skill/references/google-stitch.md`](../../skill/references/google-stitch.md)
5. Set model to **Gemini 2.5 Pro** for best results
6. Click **Save** → name it "Figma to AI Prompter"

### Usage

1. Upload a screenshot of your Figma screen using the attachment button
2. Type: "Generate a [Lovable / Stitch / Figma Make] prompt for this screen"
3. The model will ask context questions and generate the prompt

---

## Option B — Gemini Advanced (gemini.google.com)

Gemini Advanced supports **Gems** — custom AI assistants with a system prompt.

### Setup

1. Go to [gemini.google.com](https://gemini.google.com) → **Gems** → **Create a Gem**
2. Name: **Figma to AI Prompter**
3. In the instructions field, paste `SKILL.md` + your preferred reference file(s)
4. Save the Gem

### Usage

Open the Gem, upload a Figma screenshot, and ask for a prompt.

---

## Model recommendation

| Use case | Recommended model |
|----------|-------------------|
| Daily use, fast | Gemini 2.5 Flash |
| High-quality prompts, complex screens | Gemini 2.5 Pro |
| Google Stitch prompts specifically | Gemini 2.5 Pro (same model Stitch uses — better alignment) |

---

## Limitations vs. Claude

| Feature | Claude (with Figma MCP) | Gemini |
|---------|------------------------|--------|
| Reads Figma directly | ✅ | ❌ Screenshot only |
| Exact hex / spacing values | ✅ | ⚠️ Approximated |
| Design system token extraction | ✅ | ❌ |
| Google Stitch prompt alignment | ✅ | ✅ (same underlying model) |
| All 5 tools supported | ✅ | ✅ |
