# Orange Slice agent kit

Give your coding agent a job: find the right people to sell to, and reach them.

[Orange Slice](https://orangeslice.ai) ships a CLI, `@orangeslice/cli`, built from the start to
be driven by an agent instead of a person — every command takes `--json` and `--dry-run`, reads
and writes NDJSON so it composes, and the package's own README has a section written directly
to an agent reading it. This repo is that section, pulled out into a drop-in `AGENTS.md`, plus
the couple of things every agent needs to know before it uses it on somebody's real LinkedIn
account.

Clone this, point your agent at it, and it can set up and run an outbound loop for you or a
customer — find companies, find the right people at them, draft the first note, send it, learn
from what gets accepted, and do it again tomorrow.

## Pick your agent

**Claude Code** — clone this repo (or copy `CLAUDE.md` and `AGENTS.md` into an existing one) and
run `claude` inside it. Claude Code reads `CLAUDE.md` on its own.

**Codex** — same clone, run `codex` inside it. Codex reads `AGENTS.md` on its own.

**Grok, ChatGPT, or anything else without a project file it reads automatically** — open
[`AGENTS.md`](AGENTS.md) and paste its contents into the agent's system prompt or first
message.

## What actually happens

1. The agent runs `orangeslice factory init --website <your site>` once. A model reads your
   site, works out what you sell and who buys it, and writes a first LinkedIn connection note.
2. It shows you the targeting and the note before anything goes out, and a dry run of who it
   would contact today.
3. Only once you say go does it start sending — a handful of connection requests a day, from
   your own LinkedIn account.
4. Every few weeks it looks at what got accepted, keeps the note or the targeting that's
   working, and tries one change to the other. `orangeslice factory status` shows the scorecard.

None of this costs anything until step 3. `--dry-run` is the whole point of this CLI: the price
is always shown before anything is called.

## Before you point this at a real account

- It sends real connection requests from a real LinkedIn profile. Run `factory run --dry-run`
  first, always, and only move past it when the person whose account it is has said go.
- It needs their LinkedIn connected to Orange Slice first — `factory init` will tell you the
  URL for that if it isn't.
- An agent following `AGENTS.md` is told to stop and ask at exactly those two points. If yours
  doesn't, something's wrong — don't let it send anything until it does.

## Setup

```bash
npm i -g @orangeslice/cli
orangeslice login          # opens a browser, stores a key
orangeslice auth status    # confirms it worked
```

No Orange Slice account yet: [orangeslice.ai](https://orangeslice.ai).

## Everything else the CLI can do

The factory loop is the fastest way to see it work, not the whole surface. 54 commands —
enrichment, company and people search, web search, a plain AI call — are all reachable the same
way:

```bash
orangeslice tools search email
orangeslice tools get work_email
```

Full CLI reference: https://www.npmjs.com/package/@orangeslice/cli

## License

MIT — copy whatever's useful into your own agent setup.
