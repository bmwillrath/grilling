# grilling

An [Agent Skill](https://agentskills.io) that scopes a project in a few short question rounds: ranked questions, numbered options, one recommendation, and a written record so nothing drifts.

## What it does

- Asks 3 to 4 shaping questions first, never a wall of ten. Later rounds are capped at five.
- Every question is one sentence with numbered, single-line options and a recommended pick. You answer with numbers, or adjust the recommendation, instead of writing a reply from scratch.
- Reads what you have already said and what is already on disk before asking, so it does not re-ask settled decisions.
- Prunes the question tree after every round. An answer that closes a branch removes every question under it.
- Looks up facts itself (files, APIs, a logged-in web app) rather than asking you for them, read-only, and asks for access as a normal numbered question when it needs it.
- Writes every round's questions and answers to `grilling.md` in the project folder, so the next session picks up the decisions instead of the interview.

It works in any coding agent that reads the Agent Skills format (a `SKILL.md` with `name` and `description` frontmatter). It was written in Claude Code; other platforms are listed below.

## Install

Clone the repo, then copy `skills/grilling` into the skills folder of the tool you use. Personal paths make the skill available everywhere; project paths make it available in that repo only and can be committed alongside the code.

```bash
git clone https://github.com/bmwillrath/grilling.git
```

| Tool | Personal | Project |
| --- | --- | --- |
| Claude Code | `~/.claude/skills/grilling` | `.claude/skills/grilling` |
| OpenAI Codex CLI | `~/.codex/skills/grilling` | `.codex/skills/grilling` |
| Cursor | `~/.cursor/skills/grilling` | `.cursor/skills/grilling` |
| Gemini CLI | `~/.gemini/skills/grilling` | `.gemini/skills/grilling` |
| GitHub Copilot | `~/.copilot/skills/grilling` | `.github/skills/grilling` |
| OpenCode | `~/.config/opencode/skill/grilling` | `.opencode/skill/grilling` |

For example, on Claude Code:

```bash
cp -r grilling/skills/grilling ~/.claude/skills/grilling
```

Several of these tools also read `.claude/skills/` in a project, so one copy there often covers more than one agent. Paths are current as of September 2026; if your tool moves them, check its skills documentation. Any other tool that implements the Agent Skills standard works the same way: put the `grilling` folder where it looks for skills.

## Use

Invoke it by name (`/grilling` in Claude Code, `$grilling` in Codex, or however your tool invokes skills), or just say it:

- "grill me on this"
- "stress-test this plan"
- "scope this before we build it"

The agent asks a round, you answer with numbers, it asks the next round. When no remaining question would change what gets built, it lists the settled decisions and waits for your confirmation before doing anything.

## What a round looks like

```
**Q1 · Hosting**
Where does this run?
  1️⃣ Existing cPanel host
  2️⃣ Docker on the apps VPS
  3️⃣ Serverless
➡️ 2, it needs a cron and a database and the VPS already has both

**Q2 · Users**
Who logs in?
  1️⃣ Staff only
  2️⃣ Staff and customers
➡️ 1, nothing in the brief needs a customer-facing login yet
```

Reply with `Q1: 2, Q2: 1` or `Q1: 1 + 3, Q2: 2` and it moves on.

## Licence

MIT. See [LICENSE](LICENSE).
