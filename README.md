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
            ├── SKILL.md
            └── reference.md
```

## Available skills

| Category | Skill | What it does |
|---|---|---|
| Product | `idea-brief` | Structures a product/feature idea into a brief for stakeholder review. |
| Product | `prd` | Writes a full PRD / feature spec from a description of the work. |
| Helper | `conference-to-calendar` | Extracts a conference program (webpage, PDF, or pasted text) into an importable `.ics` calendar file. |

`.claude-plugin/marketplace.json` declares one plugin per skill. Each entry's `skills` path points directly at the nested skill folder (which contains `SKILL.md` itself), so Claude Code installs exactly that one skill rather than scanning the whole category:

```json
{
  "name": "claude-skills",
  "owner": { "name": "<your-github-user>" },
  "metadata": { "description": "Personal Claude Skills", "version": "1.0.0" },
  "plugins": [
    { "name": "idea-brief", "source": "./", "skills": ["./skills/product/idea-brief"], "category": "product" },
    { "name": "prd", "source": "./", "skills": ["./skills/product/prd"], "category": "product" },
    { "name": "conference-to-calendar", "source": "./", "skills": ["./skills/helper/conference-to-calendar"], "category": "helper" }
  ]
}
```

If you'd rather install a whole category in one step, point a plugin's `skills` at the category folder itself instead of a leaf skill — e.g. `"skills": ["./skills/product"]` — and Claude Code auto-discovers every skill folder one level inside it (`idea-brief`, `prd`) as part of that single plugin. That trades per-skill install granularity for a coarser "install by category" install; the per-skill entries above are the finer-grained default.

## Installation

```bash
claude plugin marketplace add sichvoge/skills
claude plugin install idea-brief@skills
claude plugin install conference-to-calendar@skills
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