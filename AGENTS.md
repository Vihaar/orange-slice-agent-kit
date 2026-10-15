# AGENTS.md

Instructions for any coding agent reading this file — Claude Code, Codex, Grok, or anything
else. One file, so every agent gets the same instructions and never drifts apart.

This project uses [Orange Slice](https://orangeslice.ai) for outbound: finding the right
people to sell to, and reaching them. The `orangeslice` CLI is how an agent drives it.

## Setup

```bash
npm i -g @orangeslice/cli
orangeslice auth status
```

If that exits 3, the human running this must run `orangeslice login` (it opens a browser) or
set `ORANGESLICE_API_KEY`. Do not go looking for a key anywhere else.

## If you've been asked to set up outbound for a company

Do this, in order, and stop where it says to.

1. `orangeslice factory init --website <their site> --json`. Read the JSON back to the human
   in plain words: what it thinks they sell, who it will contact, the note it will send, and
   what a day costs in credits. Ask whether the targeting and the note are right.
2. If they want a change, edit `factory.json` — `variants[0].icp` and `variants[0].note` — and
   say so.
3. If the JSON carries a non-null `blocker`, give the human its URL and stop until they say
   LinkedIn is connected. Nothing sends without it.
4. `orangeslice factory run --dry-run`. Show the human the names it would contact today. This
   costs only the database reads, nothing else.
5. Only when they say go: `orangeslice factory run`. Then set up the cron line it printed, for
   every weekday morning.
6. Each week: `orangeslice factory status --json`. Report the acceptance rate per variant and
   what changed.

**Never run `factory run` for real without `--dry-run` first on a fresh state file.** **Never
run it on somebody's behalf without their go-ahead** — it sends real connection requests from
their own LinkedIn profile.

## Beyond the daily loop

`orangeslice tools search <word>` finds any of the CLI's other commands — enrichment, company
search, web search, a plain AI call — by what they do, instead of reading the full `--help`.
Every command takes `--dry-run` to show its price before it spends anything, and `--json` for
one machine-readable result instead of a stream.

Full reference: https://www.npmjs.com/package/@orangeslice/cli
