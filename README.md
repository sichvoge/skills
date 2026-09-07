# Claude Skills

A public repository of Claude Skills I've built to turn repeating tasks into reusable workflows.

Each skill is a self-contained `SKILL.md` (plus, where needed, a `reference.md` or scripts) that Claude reads and follows when a task matches its description — no slash command needed, it just triggers from context.

## Structure

Skills are physically grouped by category under `skills/`. `.claude-plugin/marketplace.json` declares each skill as its own installable plugin with an explicit path, so nesting doesn't limit you to installing a whole category at once:

```
.
├── .claude-plugin/
│   └── marketplace.json      # one plugin entry per skill, explicit nested paths
└── skills/
    ├── product/
    │   ├── idea-brief/
    │   │   └── SKILL.md
    │   └── prd/
    │       └── SKILL.md
    └── helper/
        ├── conference-to-calendar/
        │   ├── SKILL.md
        │   └── reference.md
        └── company-research-profile/
            └── SKILL.md
```

## Available skills

| Category | Skill | What it does |
|---|---|---|
| Product | `idea-brief` | Structures a product/feature idea into a brief for stakeholder review. |
| Product | `prd` | Writes a full PRD / feature spec from a description of the work. |
| Helper | `conference-to-calendar` | Extracts a conference program (webpage, PDF, or pasted text) into an importable `.ics` calendar file. |
| Helper | `company-research-profile` | Builds a structured, skeptical company research profile (styled HTML) ahead of a job application, interview, or meeting. |

`.claude-plugin/marketplace.json` declares one plugin per category. Each entry's `skills` array lists all skill folders in that category, so installing a plugin (e.g. `helper`) gives you every skill inside it:

```json
{
  "name": "skills",
  "owner": { "name": "Christian Heidenreich" },
  "metadata": { "description": "Personal Claude Skills", "version": "1.0.0" },
  "plugins": [
    {
      "name": "product",
      "source": "./",
      "skills": ["./skills/product/idea-brief", "./skills/product/prd"],
      "category": "product",
      "description": "Product management skills."
    },
    {
      "name": "helper",
      "source": "./",
      "skills": ["./skills/helper/conference-to-calendar", "./skills/helper/company-research-profile"],
      "category": "helper",
      "description": "Skills that help with day-to-day tasks."
    }
  ]
}
```

If you'd rather install skills individually, change each plugin entry so its `skills` array contains only one path — e.g. `"skills": ["./skills/product/idea-brief"]` — and give each entry a unique `name`. That trades category-level convenience for per-skill install granularity.

## Installation

```bash
claude plugin marketplace add sichvoge/skills
claude plugin install product@skills
claude plugin install helper@skills
```

**Any SKILL.md-compatible tool** (Codex/ChatGPT, Cursor, Gemini CLI, Copilot, etc.) via the cross-tool installer:

```bash
npx skills add sichvoge/skills
```

**Claude.ai (web/desktop)** — download the individual skill's `SKILL.md` (or a zipped skill folder) and upload it under the skill/project settings UI.

**Manual** — clone the repo and copy whichever skill's leaf folder you want into your tool's skills directory (copy the skill folder itself, not its parent category folder — a copied category folder won't be auto-discovered):

```bash
git clone https://github.com/sichvoge/skills.git
cp -r skills/skills/helper/conference-to-calendar ~/.claude/skills/
```

## Adding a new skill

1. Add a new folder under `skills/<category>/<skill-name>/` with a `SKILL.md` (YAML frontmatter: `name`, `description`; the rest is instructions). Add a `reference.md` alongside it if the skill needs detailed format/API notes that shouldn't load into every conversation.
2. Add a matching entry to `.claude-plugin/marketplace.json`, with `skills` pointing at the new nested path and a `category` tag, so it's installable on its own.
3. Add a row to the table above.

## License

MIT