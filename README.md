# Claude Skills

A public repository of Claude Skills I've built to turn repeating tasks into reusable workflows.
 
Each skill is a self-contained `SKILL.md` (plus, where needed, a `reference.md` or scripts) that Claude reads and follows when a task matches its description.

## Repository Structure

Skills are grouped into plugins by domain, cataloged by a single marketplace manifest at the repo root:
 
```
.
├── .claude-plugin/
│   └── marketplace.json        # catalog listing every plugin below
├── product-skills/
│   ├── .claude-plugin/
│   │   └── plugin.json
│   └── skills/
│       ├── idea-brief/
│       │   └── SKILL.md
│       └── prd/
│           └── SKILL.md
└── helper-skills/
    ├── .claude-plugin/
    │   └── plugin.json
    └── skills/
        ├── conference-to-calendar/
            ├── SKILL.md
            └── reference.md
```
 
New groups get added as new top-level plugin folders and you only need to install the group you need.

## Available skills
 
| Plugin | Skill | What it does |
|---|---|---|
| `product-skills` | `idea-brief` | Structures a product/feature idea into a brief for stakeholder review. |
| `product-skills` | `prd` | Writes a full PRD / feature spec from a description of the work. |
| `helper-skills` | `conference-to-calendar` | Extracts a conference program (webpage, PDF, or pasted text) into an importable `.ics` calendar file. |

## Installation
 
**Claude Code** — add the marketplace once, then install whichever plugin group you want:
 
```bash
claude plugin marketplace add sichvoge/skills
claude plugin install product-skills@skills
claude plugin install helper-skills@skills
```
 
**Any SKILL.md-compatible tool** (Codex/ChatGPT, Cursor, Gemini CLI, Copilot, etc.) via the cross-tool installer:
 
```bash
npx skills add sichvoge/skills
```
 
**Claude.ai (web/desktop)** — download the individual skill's `SKILL.md` (or the packaged `.skill` file) and upload it under the skill/project settings UI.
 
**Manual** — clone the repo and copy whichever skill folder you want into your tool's skills directory, e.g.:
 
```bash
git clone https://github.com/sichvoge/skills.git
cp -r skills/helper-skills/skills/conference-to-calendar ~/.claude/skills/
```
 
## Adding a new skill
 
1. Pick (or create) the plugin group it belongs to.
2. Add a new folder under `<plugin>/skills/<skill-name>/` with a `SKILL.md` (YAML frontmatter: `name`, `description`; the rest is instructions). Add a `reference.md` alongside it if the skill needs detailed format/API notes that shouldn't load into every conversation.
3. Update the plugin's `plugin.json` version if you're bumping it, and add a row to the table above.
