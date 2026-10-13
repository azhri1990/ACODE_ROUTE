# GitHub search: "agent agency" in repos (2026-10-13)

Search: `agent agency in:name` on GitHub → **746 matches**.
Checked the top by stars + content. Rule followed: never judge by name, check the real content.

## Saved to catalog (2)

### msitarzewski/agency-agents (159,666 stars) — https://github.com/msitarzewski/agency-agents
- "The Agency" — a library of **200+ ready-made AI agent personas** (frontend wizards, Reddit community ninjas, reality checkers, and more).
- Each persona is a plain `.md` file with: identity/personality, mission, workflows, deliverable examples, success metrics.
- Organized in divisions: `engineering/`, `academic/`, `design/`, marketing, sales, security, etc. (`divisions.json`).
- License: MIT.
- **Works with OpenCode**: `./scripts/install.sh --tool opencode`, env var `OPENCODE_AGENTS_DIR`. Also Claude Code, Cursor, Codex, Gemini CLI, Qwen, and more (20 tools).
- ⚠️ The script notes OpenCode only loads ~119 agents without silently dropping some (upstream OpenCode #27988) — so pick personas, don't install all 200+.
- **On this phone**: text-only (.md + bash scripts), no GPU needed → usable. Best use: read personas as ideas / copy the few that fit the existing scout-technician-dispatcher-inspector-auditor set, and adapt them to OpenCode V2 frontmatter (`description` + `mode`) in `~/.config/opencode/agents/`.

### msitarzewski/agency-agents-app (712 stars) — https://github.com/msitarzewski/agency-agents-app
- Desktop app (macOS / Linux / Windows) to browse the roster and one-click install into tools.
- **NOT for this phone** (no Android build). The scripts in the main repo are the phone-friendly path.

## Skipped (dedupe / not useful)
- Translations/fork families of the same content: `jnMetaCode/agency-agents-zh` (21k stars), pt-BR, ko, ru, id, cn variants → same content, not unique.
- `Anas-Khan93/ai-agency-agents` (537 stars) — "Open Agency" AI workforce; different project → could add later if wanted.
- `keeply-cn/agency-agents-zh`, `Aura-huang/agency-agents-cn`, `leeknowsai/paperclip-agency-agents` (a Minecraft plugin!), etc. — niche/derivative.

## Next step idea (needs user y/n)
- Use agency-agents as a **persona idea source for the phone's agent set**: technician could convert a small pick (`--tool opencode`) into a temp dir, verify the output format fits OpenCode V2, then manually add 1–2 hand-picked personas (e.g. a "design" or "marketing" persona) as new agents — with user approval.