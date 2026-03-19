# Installing on ChatGPT (Custom GPT)

ChatGPT doesn't support `.skill` files natively. The best way to use this prompt generator in ChatGPT is to create a **Custom GPT** with the skill content as its system instructions.

> ⚠️ ChatGPT has no Figma MCP access. The skill will always run in **screenshot mode** — paste a screenshot of your Figma screen when prompted, and the GPT will analyze it visually.

---

## Setup

### 1. Create a new Custom GPT

1. Go to [chat.openai.com](https://chat.openai.com) → **Explore GPTs** → **Create**
2. Click **Configure** (not the chat builder)

### 2. Paste the system prompt

In the **Instructions** field, paste the contents of [`skill/SKILL.md`](../../skill/SKILL.md), followed by the contents of all five reference files:

- [`skill/references/lovable.md`](../../skill/references/lovable.md)
- [`skill/references/figma-make.md`](../../skill/references/figma-make.md)
- [`skill/references/pencil-dev.md`](../../skill/references/pencil-dev.md)
- [`skill/references/paper-design.md`](../../skill/references/paper-design.md)
- [`skill/references/google-stitch.md`](../../skill/references/google-stitch.md)

> **Note on token cost:** Pasting all files loads ~4,300 tokens of instructions into every conversation. If you only use one or two tools, paste only `SKILL.md` + the relevant reference file to reduce overhead.

### 3. Configure the GPT

| Field | Value |
|-------|-------|
| Name | Figma to AI Prompter |
| Description | Reads your Figma design (via screenshot) and generates optimized prompts for Lovable, Figma Make, Pencil.dev, Paper.design, and Google Stitch |
| Conversation starters | "Write a Lovable prompt for this screen" / "Generate a Stitch prompt from my design" |

### 4. Enable image input

Make sure **"Use DALL·E"** is OFF (you don't need it) and image uploads are enabled so users can paste Figma screenshots.

---

## Usage

1. Open your Custom GPT
2. Upload a screenshot of your Figma screen
3. Say which tool you want to use: "Generate a Lovable prompt for this"
4. The GPT will ask context questions, propose interactions, and generate the prompt

---

## Limitations vs. Claude

| Feature | Claude (with Figma MCP) | ChatGPT Custom GPT |
|---------|------------------------|-------------------|
| Reads Figma directly | ✅ | ❌ Screenshot only |
| Exact hex / spacing values | ✅ | ⚠️ Approximated |
| Design system token extraction | ✅ | ❌ |
| Token estimate accuracy | ✅ | ✅ |
| All 5 tools supported | ✅ | ✅ |
