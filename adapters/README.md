# Harness adapters

Portable skills live in [`skills/`](../skills/). Most skills install unchanged into any harness that reads `SKILL.md`.

Add a subdirectory here when a skill needs harness-specific overrides:

```text
adapters/
  cursor/barry/       # Cursor-only extensions (Task tool, subagent names)
  claude-code/barry/  # Claude Code subagent / skill hooks
  codex/barry/        # Codex plugin packaging notes
```

**Barry** currently ships as a single portable, harness-agnostic skill under `skills/barry/`; its delegation roster in `references/fleet.md` maps logical agent roles to whatever each harness exposes. Split into adapter folders only if a harness needs materially different instructions.
