# Installing on Cursor, Windsurf, GitHub Copilot & VS Code

These tools use AI for code generation — the skill works as a **rules file** or **system prompt** that gives the AI the context it needs to generate tool-specific UI prompts whenever you ask.

> ⚠️ None of these editors have Figma MCP access by default. The skill runs in screenshot mode — paste a Figma screenshot into the chat when prompted. Some editors (Cursor) support MCP servers, so Figma MCP can be connected manually (see below).

---

## Cursor

### Option A — Project rules (recommended)

1. In your project root, create `.cursor/rules/figma-prompter.mdc`:

```
---
description: Generate optimized prompts for AI prototyping tools from Figma designs
globs: []
alwaysApply: false
---

[paste the contents of skill/SKILL.md here]

[paste the contents of skill/references/lovable.md here]
[paste the contents of skill/references/figma-make.md here]
[paste the contents of skill/references/pencil-dev.md here]
[paste the contents of skill/references/paper-design.md here]
[paste the contents of skill/references/google-stitch.md here]
```

2. In Cursor chat, type `@figma-prompter` to activate the rule, then paste your screenshot and ask for a prompt.

### Option B — Figma MCP in Cursor (full design token access)

Cursor supports MCP servers. To connect Figma:

1. Open **Cursor Settings** → **Features** → **MCP**
2. Add a new MCP server:

```json
{
  "name": "figma",
  "command": "npx",
  "args": ["-y", "@figma/mcp-server"],
  "env": {
    "FIGMA_ACCESS_TOKEN": "your-figma-personal-access-token"
  }
}
```

3. Get your token at: **Figma → Account Settings → Security → Personal access tokens**
4. Once connected, paste your Figma file URL in Cursor chat — the skill will read it directly without needing a screenshot.

---

## Windsurf (by Codeium)

1. Open **Windsurf Settings** → **AI Rules** or create `.windsurfrules` in your project root
2. Paste the contents of `skill/SKILL.md` followed by your preferred reference files
3. In the Cascade chat, paste a Figma screenshot and ask: "Generate a Lovable prompt for this screen"

---

## GitHub Copilot (VS Code)

### Using custom instructions

1. Open VS Code → **Settings** → search "Copilot instructions"
2. Set **GitHub Copilot: Custom Instructions File** to a file in your repo
3. Create `.github/copilot-instructions.md` and paste the skill content:

```markdown
# Figma to AI Prompter

[paste skill/SKILL.md contents]

[paste your preferred reference file contents]
```

4. In Copilot Chat, upload a Figma screenshot (VS Code 1.90+) and ask for a prompt

### Using a workspace `.instructions.md`

VS Code also supports per-workspace instructions for Copilot:

1. Create `.github/instructions/figma-prompter.instructions.md`
2. Add frontmatter: `applyTo: '**'`
3. Paste the skill content below the frontmatter

---

## Which files to include

To keep instructions lean, only include the reference file(s) for the tool(s) you actually use:

| If you use... | Include |
|--------------|---------|
| Lovable only | `SKILL.md` + `lovable.md` |
| Figma Make only | `SKILL.md` + `figma-make.md` |
| All tools | `SKILL.md` + all 5 reference files |

---

## Limitations vs. Claude

| Feature | Claude (Cowork) | Cursor + Figma MCP | Cursor / Windsurf / Copilot |
|---------|----------------|-------------------|----------------------------|
| Reads Figma directly | ✅ | ✅ (with MCP) | ❌ Screenshot only |
| Exact design tokens | ✅ | ✅ | ⚠️ Approximated |
| Design system extraction | ✅ | ✅ | ❌ |
| `.skill` one-click install | ✅ | ❌ | ❌ |
| All 5 tools supported | ✅ | ✅ | ✅ |
