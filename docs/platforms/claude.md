# Installing on Claude (Cowork & Claude Code)

## 1. Install the skill

**Option A — One-click install (Cowork desktop app)**

1. Download [`ai-prototype-prompter.skill`](../../ai-prototype-prompter.skill)
2. Double-click the file — Claude will prompt you to install it
3. Confirm the install

**Option B — Claude Code (terminal)**

```bash
claude skills install ai-prototype-prompter.skill
```

Or install directly from this repo:

```bash
claude skills install https://github.com/royvillasana/figma-to-ai-prompter/raw/main/ai-prototype-prompter.skill
```

---

## 2. Figma MCP setup

The skill uses the Figma MCP to read your design files directly. Without it, the skill falls back to screenshot mode (approximate values only).

**In Claude Cowork:**
1. Open Claude → Settings → Integrations
2. Find **Figma** and click Connect
3. Sign in with your Figma account
4. Grant read access to your files

**In Claude Code:**
Add the Figma MCP to your `claude_desktop_config.json` (macOS: `~/Library/Application Support/Claude/`):

```json
{
  "mcpServers": {
    "figma": {
      "command": "npx",
      "args": ["-y", "@figma/mcp-server"],
      "env": {
        "FIGMA_ACCESS_TOKEN": "your-figma-personal-access-token"
      }
    }
  }
}
```

Get your Figma Personal Access Token at: **Figma → Account Settings → Security → Personal access tokens**

---

## 3. Using the skill

Once installed, just describe what you want in plain language:

> "Write me a Lovable prompt for this Figma screen: [paste Figma URL]"

> "Generate a Google Stitch prompt for my dashboard design"

> "Help me prompt this UI in Figma Make"

The skill will:
1. Verify the Figma MCP is connected (or ask for a screenshot if not)
2. Read your design
3. Ask a few quick questions (tool, platform, stack, goal)
4. Propose interactions for your approval
5. Generate a copy-ready prompt with token estimate

---

## 4. Optional — Design system file

If you have a separate Figma design system / component library file, paste its URL when the skill asks. It will cross-reference and pull only the tokens your screen actually uses, adding semantic token names (like `color/primary → #6366F1`) to the generated prompt.
