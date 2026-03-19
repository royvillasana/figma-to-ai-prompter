# Figma Proto Prompt

```txt
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


⸻

🚀 Overview

Figma-to-AI Prompter is a tool that converts Figma designs into structured prompts that can be used with AI tools to generate working prototypes.

It bridges the gap between design → prompt → prototype, enabling faster iteration and reducing manual translation effort.

⸻

🧠 How it works
	1.	Extract design structure from Figma
	2.	Transform layout + components into structured prompts
	3.	Feed prompts into AI tools (Lovable, Figma Make, etc.)
	4.	Generate working prototypes automatically

⸻

🧰 Supported Tools

Tool	Model	Output	Token model
Lovable￼	Claude	Full-stack React app	Credit per message
Figma Make￼	Claude	Interactive HTML prototype	Figma subscription
Pencil.dev￼	AI	Production React components	Credit per generation
Paper.design￼	Agent	HTML/CSS canvas + code export	Agent tool calls
Google Stitch￼	Gemini 2.5	Responsive UI → Figma / HTML	Limited monthly generations


⸻

📂 Project Structure

figma-to-ai-prompter/
│
├── prompts/
│   ├── figma-to-lovable.md
│   ├── figma-to-figmamake.md
│   ├── figma-to-pencil.md
│   └── figma-to-paper.md
│
├── examples/
│   ├── input-figma.json
│   └── output-prompt.md
│
├── scripts/
│   └── transform.js
│
└── README.md


⸻

⚙️ Usage
	1.	Export your Figma design (JSON or structured data)
	2.	Select your target AI tool
	3.	Use the corresponding prompt template
	4.	Paste into your AI tool
	5.	Generate your prototype

⸻

🎯 Goal

Enable designers and developers to:
	•	Reduce friction between design and development
	•	Prototype faster using AI
	•	Standardize prompt engineering for UI generation

⸻

🔥 Future Improvements
	•	Automated Figma plugin
	•	Prompt optimization layer
	•	Multi-framework output (React, Vue, HTML)
	•	AI-assisted design validation

⸻

👨‍💻 Author

Roy Villasana
Senior UX Designer | AI Product Builder
royvillasana@gmail.com

⸻

⭐ Contribute

Feel free to open issues or submit pull requests to improve prompts, integrations, or workflows.

