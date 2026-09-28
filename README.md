# grilling

A Claude Code skill that scopes a project in a few short question rounds: ranked questions, numbered options, one recommendation, and a written record so nothing drifts.

## What it does

- Asks 3 to 4 shaping questions first, never a wall of ten. Later rounds are capped at five.
- Every question is one sentence with numbered, single-line options and a recommended pick. You answer with numbers, or adjust the recommendation, instead of writing a reply from scratch.
- Reads what you have already said and what is already on disk before asking, so it does not re-ask settled decisions.
- Prunes the question tree after every round. An answer that closes a branch removes every question under it.
- Looks up facts itself (files, APIs, a logged-in web app) rather than asking you for them, read-only, and asks for access as a normal numbered question when it needs it.
- Writes every round's questions and answers to `grilling.md` in the project folder, so the next session picks up the decisions instead of the interview.

## Install

Copy `skills/grilling` into your Claude Code skills folder:

```bash
git clone https://github.com/<you>/grilling.git && cp -r grilling/skills/grilling ~/.claude/skills/grilling
```

For a single project instead of every project, copy it into that project's `.claude/skills/` folder.

## Use

Run `/grilling`, or just say it:

- "grill me on this"
- "stress-test this plan"
- "scope this before we build it"

Claude asks a round, you answer with numbers, it asks the next round. When no remaining question would change what gets built, it lists the settled decisions and waits for your confirmation before doing anything.

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
