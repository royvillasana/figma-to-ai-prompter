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

Figma-to-AI Prompter is a tool that converts Figma designs into structured prompts
that can be used with AI tools to generate working prototypes.

It bridges the gap between design → prompt → prototype, enabling faster iteration
and reducing manual translation effort.

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
	6.  Give to the AI the Figma Make published link to let the Ai validate the outcome
	7.  The Ai will give you an updated prompt if needed to make the prototype resemble more the Figma design

⸻

🎯 Goal

Enable designers and developers to:
	•	Reduce friction between design and development
	•	Prototype faster using AI
	•	Standardize prompt engineering for UI generation

⸻

## ⚡ Token Efficiency

Two compounding optimizations were implemented to reduce token usage across both **execution** and **output layers**.

---

### 🧠 1. Skill Execution Cost  
*(Cost of running the skill itself)*

- Reduced from **~40,782 → ~33,508 tokens per run**
- **↓ 18% reduction before prompt generation**

**How:**
- Compressed `SKILL.md`
- Lazy-loaded only **1 of 5 reference files per session**

---

### ✍️ 2. Generated Prompt Output  
*(Tokens used in the final prompt sent to AI tools)*

Initial assumption for a Figma Make prompt:
- **500 → 1,500 tokens**

Actual measured result (including validation loop):
- **~90 tokens total (both prompts combined)**
- **~45 tokens per prompt**

---

### 📊 Comparison

| Scenario | Baseline | Actual | Reduction |
|---|---|---|---|
| Low estimate | 500 tokens | ~45 tokens | **91% ↓** |
| Mid estimate | 1,000 tokens | ~45 tokens | **95.5% ↓** |
| High estimate | 1,500 tokens | ~45 tokens | **97% ↓** |

---

### 🔍 Key Insight

Using the **MCP text context block** instead of frame attachment:

- Encodes Figma designs as **structured text directly in the prompt**
- Eliminates **300–500 hidden tokens** from frame attachments
- Provides **more explicit and controllable context** vs. visual interpretation

---

### 🚀 Combined Impact

- **↓ 18%** → Skill execution cost  
- **↓ 91–97%** → Generated prompt size  

**Result:**  
A significantly more efficient pipeline from **design → prompt → prototype**, both in cost and performance.



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

Feel free to open issues or submit pull requests to improve prompts,
integrations, or workflows.

