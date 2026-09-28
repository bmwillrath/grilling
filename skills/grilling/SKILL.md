---
name: grilling
description: Interview the user in short, ranked rounds until a plan, decision, or idea is clear enough that it cannot be misread or drift. Use when the user wants to stress-test their thinking, scope a new piece of work, or uses any 'grill' trigger phrases.
---

The goal is a plan the user and the assistant both read the same way, reached with as few questions as possible. Treat the plan as a **decision tree**: each answer settles one thing and opens the smaller decisions that depend on it. Most of the tree is never worth asking about; the job is to find the handful of decisions that shape the rest.

Before asking anything, read what is already settled: the conversation so far, the folder's CLAUDE.md and any decisions or brief on disk. A question answered there is a settled node, not a frontier question. Re-ask it only when the record is ambiguous, a later statement contradicts it, or the request needs something the recorded choice does poorly and another route does well. When you re-ask, say in one line why.

Questions go out in **rounds**. A decision is askable when everything it depends on is already answered; the set of askable decisions is the **frontier**. Never ask the whole frontier. Rank it by leverage (how much of the tree an answer reshapes) and ask only the top of it:

- **Round 1 is a shaping round: 3 to 4 questions, never more than 10.** You do not yet know the shape of the tree, so most of the early frontier is guesswork. Ask only the questions that decide what kind of project this is; everything else waits until those answers show which branches exist.
- **Later rounds: up to 5 questions**, highest leverage first. Park the rest.
- A question whose answer would not change what gets built is not asked.

Number each question and give your recommended answer. Then wait for the user's answers before the next round.

Format a round like so:

```
**Q1 · <question title>**
<one-sentence question>
  1️⃣ <option>
  2️⃣ <option>
  3️⃣ <option>
➡️ 1 + 3, <one clause why>

**Q2 · <question title>**
<one-sentence question>
  1️⃣ <option>
  2️⃣ <option>
➡️ 2, <one clause why>
```

One question is one decision in one sentence. Options are single lines, two to six of them; never fold several suggestions into one option. Options are combinable: the recommendation names the option or combination by number so the user can confirm with a number list or adjust it, rather than write a reply from scratch.

After each round, rebuild the tree from the answers, don't just extend it. An answer that closes a branch deletes every question under it, parked or not. An answer that changes the shape of the project can invalidate questions you were about to ask; check each parked question still matters before it goes out. Then re-rank what is left by leverage and take the next round from the top. If two questions in the same round depend on each other, the second one belongs to the next round.

The user decides; the assistant finds out. Anything the assistant can discover (files, tools, APIs, docs, a logged-in web app) is looked up, never asked. Access already available is used without asking, read-only: the interview makes no change to any system, file or account without the user's explicit permission. Where the platform supports sub-agents, send lookups to one on the cheapest model that can do the job: a small or mid-tier model for plain lookups and research, a strong model where judgement matters (design trade-offs, hard debugging, adversarial checks), a frontier model only where that judgement is safety-critical or the task is beyond the strong tier. (On Claude, for example: Haiku or Sonnet, then Opus, then Fable.) Without sub-agents, do the lookup inline. Start the lookup and carry on with the rest of the round: only the questions that hang off that fact wait for it.

When a lookup needs access the assistant does not have, it is asked as a question in the round, never as a bare request: ask for the access, not the answer, say what the access is for, and list as options only the routes that actually exist for that system (an API, a web login in a browser the assistant can drive, an export, a document or screenshot, a read-only account, a walkthrough on a call). Recommend the route that gives the most for the least effort. Prefer credentials in the OS credential store (macOS Keychain, Windows Credential Manager), referenced by name, and offer that format when asking; the user decides whether a key is worth storing.

Example only; the options are whatever fits:

```
**Q3 · Access to <system>**
I need to see how <system> is set up to evaluate <thing it decides>. What are you comfortable giving me?
  1️⃣ API access (paste the key, or add it to the OS credential store and give me the item name)
  2️⃣ Browser: log me in and I drive the web app
  3️⃣ Screenshots or an export you upload
➡️ 1, it gets the most out of the lookup and is repeatable
```

The interview ends when no remaining question would change what gets built. Before acting, state the settled decisions in a short list and wait for the user to confirm the shared understanding.

If the project has a folder, keep `grilling.md` in it: append every round's questions, options and the user's answers as you go, so later sessions can read what was decided and why instead of re-asking.
