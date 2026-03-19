███████╗██╗ ██████╗ ███╗   ███╗ █████╗ 
██╔════╝██║██╔════╝ ████╗ ████║██╔══██╗
█████╗  ██║██║  ███╗██╔████╔██║███████║
██╔══╝  ██║██║   ██║██║╚██╔╝██║██╔══██║
██║     ██║╚██████╔╝██║ ╚═╝ ██║██║  ██║
╚═╝     ╚═╝ ╚═════╝ ╚═╝     ╚═╝╚═╝  ╚═╝

██████╗ ██████╗  ██████╗ ████████╗ ██████╗ 
██╔══██╗██╔══██╗██╔═══██╗╚══██╔══╝██╔═══██╗
██████╔╝██████╔╝██║   ██║   ██║   ██║   ██║
██╔═══╝ ██╔══██╗██║   ██║   ██║   ██║   ██║
██║     ██║  ██║╚██████╔╝   ██║   ╚██████╔╝
╚═╝     ╚═╝  ╚═╝ ╚═════╝    ╚═╝    ╚═════╝

██████╗ ██████╗  ██████╗ ███╗   ███╗██████╗ ████████╗
██╔══██╗██╔══██╗██╔═══██╗████╗ ████║██╔══██╗╚══██╔══╝
██████╔╝██████╔╝██║   ██║██╔████╔██║██████╔╝   ██║   
██╔═══╝ ██╔══██╗██║   ██║██║╚██╔╝██║██╔═══╝    ██║   
██║     ██║  ██║╚██████╔╝██║ ╚═╝ ██║██║        ██║   
╚═╝     ╚═╝  ╚═╝ ╚═════╝ ╚═╝     ╚═╝╚═╝        ╚═╝   


# figma-to-ai-prompter

A Claude skill that reads your Figma design through the Figma MCP and generates optimized, ready-to-paste prompts for AI prototyping tools — calibrated to each tool's prompting model, token budget, and interaction syntax.

## Supported tools

| Tool | Model | Output | Token model |
|------|-------|--------|-------------|
| [Lovable](https://lovable.dev) | Claude | Full-stack React app | Credit per message |
| [Figma Make](https://figma.com) | Claude | Interactive HTML prototype | Figma subscription |
| [Pencil.dev](https://pencil.dev) | AI | Production React component code | Credit per generation |
| [Paper.design](https://paper.design) | Agent | HTML/CSS canvas + code export | Agent tool calls |
| [Google Stitch](https://stitch.withgoogle.com) | Gemini 2.5 | Responsive UI → Figma / HTML | 350 or 50 gen/month |

## What it does

1. **Checks Figma MCP** — verifies the connection is live before proceeding; if not, asks for a screenshot as fallback
2. **Reads your Figma design** — uses `get_design_context`, `get_variable_defs`, and `get_screenshot` to extract components, layout, and exact design tokens
3. **Optionally reads your design system** — if you provide a separate Figma design system file URL, it pulls only the tokens your screen actually uses (not the whole system)
4. **Proposes interactions** — lists visible interactions from the design, suggests implied ones, waits for your approval before generating
5. **Generates a tool-specific prompt** — structured, copy-ready, with exact hex values, layer names, action words, states, guardrails, and follow-up prompts
6. **Reports token cost** — shows an estimated token count for the generated prompt and explains what it means for that specific tool's budget

## Quick install (Claude — Cowork or Claude Code)

Download [`ai-prototype-prompter.skill`](./ai-prototype-prompter.skill) and double-click it, or drag it into Claude.

See [`docs/platforms/claude.md`](./docs/platforms/claude.md) for full setup including Figma MCP connection.

## Install on other platforms

- [Claude (Cowork & Claude Code)](./docs/platforms/claude.md)
- [ChatGPT (Custom GPT)](./docs/platforms/chatgpt.md)
- [Gemini (Google AI Studio)](./docs/platforms/gemini.md)
- [Cursor / Windsurf / Copilot](./docs/platforms/cursor.md)

## Repository structure

```
figma-to-ai-prompter/
├── ai-prototype-prompter.skill   # installable skill bundle for Claude
├── skill/
│   ├── SKILL.md                  # main skill logic and workflow
│   └── references/
│       ├── lovable.md            # Lovable prompt pattern + token budget
│       ├── figma-make.md         # Figma Make prompt pattern + MCP text block
│       ├── pencil-dev.md         # Pencil.dev prompt pattern + layer inspection
│       ├── paper-design.md       # Paper.design agent-forward prompt pattern
│       └── google-stitch.md      # Google Stitch adjective-driven prompt pattern
└── docs/
    └── platforms/
        ├── claude.md             # Claude install + Figma MCP setup
        ├── chatgpt.md            # ChatGPT Custom GPT setup
        ├── gemini.md             # Gemini system prompt setup
        └── cursor.md             # Cursor / Windsurf / Copilot rules setup
```

## Requirements

- **Figma MCP** (strongly recommended) — gives the skill direct access to your design file. Without it the skill falls back to screenshot mode. See [Figma MCP setup](./docs/platforms/claude.md#figma-mcp-setup).
- A Figma file with the screen you want to build

## License

MIT — free to use, fork, and share.
# figma-to-ai-prompter
