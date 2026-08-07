<p align="center">
  <img src="assets/skills-hive-logo.svg" alt="skills-hive — portable agent skills, one hive, every harness" width="620">
</p>

<p align="center">
  <em>Each cell in the hive is a harness; the dots inside are its skills. The bee at the core is the hive itself.</em>
</p>

# skills-hive

> **Harness-agnostic by design.** Skills here are written as portable
> `SKILL.md` files with no lock-in to any one agent — the same skill runs across
> Claude Code, Codex, Cursor, GitHub Copilot, Gemini, Goose, and Scout.

Personal agent skills for [werkrbee](https://github.com/werkrbee), packaged for **multiple AI coding harnesses** using the open [Agent Skills](https://agentskills.io) standard (`SKILL.md`).

One repo. Install into Cursor, Claude Code, Codex, GitHub Copilot, Gemini CLI, Goose, Scout, and more.

## Skills

| Skill | Description |
|-------|-------------|
| [**barry**](skills/barry/) | Chief of staff — decomposes goals, delegates to a fleet of agents, synthesizes executive summaries |

## Repository layout

```text
skills-hive/
├── skills/                  # Portable skills (install source of truth)
│   └── barry/
│       ├── SKILL.md
│       └── references/
│           └── fleet.md
├── adapters/                # Harness-specific notes and future overrides
│   └── README.md
├── scripts/
│   ├── install.sh           # macOS/Linux: copy skills into harness dirs
│   └── install.ps1          # Windows/Scout: junctions + m-settings.json
├── LICENSE
└── README.md
```

Portable skills live under `skills/`. Harness-specific adapters (when needed) go under `adapters/<harness>/`.

## Install

### Option 1 — skills CLI (recommended)

Requires [Node.js](https://nodejs.org/). Installs from GitHub once this repo is pushed:

```bash
# All skills → Cursor (global)
npx skills add werkrbee/skills-hive --global -a cursor -y

# Barry only → multiple harnesses
npx skills add werkrbee/skills-hive --global \
  -a cursor -a claude-code -a codex -a github-copilot -a goose -a gemini-cli \
  --skill barry -y

# List skills in repo without installing
npx skills add werkrbee/skills-hive --list
```

### Option 2 — install script (macOS / Linux)

```bash
git clone https://github.com/werkrbee/skills-hive.git
cd skills-hive
chmod +x scripts/install.sh

# Barry → global Cursor skills dir
./scripts/install.sh --global --harness cursor --skill barry

# All skills → Cursor + Claude Code (global)
./scripts/install.sh --global --all --harness cursor --harness claude-code
```

The script is written for macOS's default Bash 3.2 (no associative arrays).

### Option 3 — install script (Windows / Scout)

Windows harnesses use directory **junctions** instead of copies/symlinks — they
match how Scout and Copilot already link skills and need no Admin or Developer
Mode. For Scout, the script also enables `loadCopilotCliSkills` in
`m-settings.json`.

```powershell
.\scripts\install.ps1                 # link into every installed Windows harness
.\scripts\install.ps1 -DryRun         # preview only
.\scripts\install.ps1 -Only scout     # just Scout
.\scripts\install.ps1 -Force          # replace an existing target
```

### Option 4 — Cursor Remote Rule (Cursor 2.4+)

1. Cursor Settings → **Rules** → **Add Rule** → **Remote Rule (GitHub)**
2. Enter: `https://github.com/werkrbee/skills-hive`
3. Select skills to import

### Option 5 — manual copy

```bash
cp -R skills/barry ~/.cursor/skills/barry
```

## Harness paths

| Harness | Global path | Project path |
|---------|-------------|--------------|
| Cursor | `~/.cursor/skills/` | `.cursor/skills/` or `.agents/skills/` |
| Claude Code | `~/.claude/skills/` | `.claude/skills/` |
| OpenAI Codex | `~/.codex/skills/` | `.agents/skills/` |
| GitHub Copilot | `~/.copilot/skills/` | `.agents/skills/` |
| Gemini CLI | `~/.gemini/skills/` | `.agents/skills/` |
| Goose | `~/.config/goose/skills/` | `.goose/skills/` |
| OpenCode | `~/.config/opencode/skills/` | `.agents/skills/` |
| Scout (Windows) | `%USERPROFILE%\.scout\skills\` | — |

Many harnesses also read `~/.agents/skills/` as a shared fallback.

### Which "Copilot"? (and Microsoft's other AI surfaces)

The installer targets **GitHub Copilot** — the coding agent (VS Code / CLI /
cloud) that reads file-based skills from `~/.copilot/skills` and `.github/skills/`.
Microsoft's other AI surfaces are different products with different extension
models, so they're intentionally not in the install scripts:

- **Microsoft Foundry agents** and **Copilot Studio** (GitHub Copilot harness)
  can run these same portable `SKILL.md` files, but they load them from a
  project/deployment `skills/` folder at session start — you *publish* skills to
  them rather than linking into a home directory.
- **Microsoft 365 Copilot** extends via declarative agents / Graph connectors,
  not file-based skills, so it's out of scope here.

## Using Barry

Invoke by name in any harness where the skill is installed:

- *"Barry, map the auth system and fix the session timeout."*
- *"Chief of staff: get this PR merge-ready."*
- *"Orchestrate agents in parallel to review my changes."*

Barry is harness-agnostic: the intake → plan → delegate → synthesize loop applies
everywhere. The agent/skill/model names in Barry are logical roles — each harness
supplies its own equivalents (see [`skills/barry/references/fleet.md`](skills/barry/references/fleet.md)).

## Adding a skill

1. Create `skills/<skill-name>/SKILL.md` with YAML frontmatter (`name`, `description`).
2. Add optional `references/`, `scripts/` as needed.
3. Update the skills table in this README.
4. Run `./scripts/install.sh --global --harness cursor --skill <skill-name>` or use `npx skills add`.

## Sync with local development

While editing this repo, symlink for live reload:

```bash
ln -sf "$(pwd)/skills/barry" ~/.cursor/skills/barry
```

Or re-run install after changes.

## License

MIT — see [LICENSE](LICENSE).
