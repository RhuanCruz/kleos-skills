---
name: kleos-cli
description: Use the Kleos CLI (`kleos` / `npx -y kleos`) to generate, review, schedule and measure UGC-style TikTok and Instagram posts for an app, from any agent that can run shell commands (OpenClaw, Codex, Hermes, CI, cron). Triggers - "use Kleos", "generate posts for my app", "plan the week of posts", "schedule posts on TikTok/Instagram", "how are my posts doing", "launch posts", "gerar posts no Kleos", "gerar posts pro meu app", "planejar a semana de posts", "agendar no TikTok", "agendar no Instagram", "revisar a fila de posts", "relatório semanal de posts". Needs `kleos login` (browser) or KLEOS_API_KEY.
license: Proprietary
compatibility: Node 22+ and network access to https://mcp.zurc.app/mcp. `kleos login` once on a machine with a browser, or KLEOS_API_KEY in the environment (servers, CI, cron).
metadata: {"openclaw": {"requires": {"bins": ["npx"], "env": ["KLEOS_API_KEY"]}, "primaryEnv": "KLEOS_API_KEY"}}
---

# Kleos (via CLI)

Kleos writes, schedules and measures UGC-style posts (carousels, hook + app demo
videos, wall-of-text) for apps on TikTok and Instagram. The `kleos` CLI is a
client of the same MCP server: every command is one tool call. **The writing
method lives in Kleos**: you orchestrate, the person decides, Kleos writes.

Run it as `kleos …` if installed (`npm i -g kleos`), otherwise `npx -y kleos …`.
Sign in once with `kleos login` (opens the browser; the token renews by
itself). Where nobody can open a browser, use an API key: `KLEOS_API_KEY` in
the environment, or `kleos login --key kleos_sk_…`.
Always add `--json` and parse stdout; errors go to stderr as JSON.

Exit codes — act on them, don't retry blindly:

| Code | Meaning | What to do |
|---|---|---|
| 0 | ok | continue |
| 1 | error (network, invalid key or expired login, server, failed job) | read the message; check `KLEOS_API_KEY`, or ask the person to run `kleos login` again |
| 2 | wrong usage (command, flag, parameter) | fix the call (`kleos <cmd> --help`) |
| 3 | plan limit / quota | **stop**: tell the person what ran out and when it renews (`details.renovaEm`, `details.planos_url`) |
| 4 | needs human approval, key scope or plan layer | show `details` to the person; for big spends, rerun with `--yes` only after they agree |

Concepts: project (one app) → agent (a voice, e.g. "bia") → account
(TikTok/Instagram) → post → publication. Post states: `gerando` → `pronto`
(ready for review) → `aprovado` → `agendado` → `publicado`.

## The method (always)

1. **Resolve the project first**: `kleos projects list --json`; pick by
   name/slug, never guess. Then `kleos agents list --project <slug> --json`.
2. **Read the plan before volume**: `kleos usage --json`; plan with the
   `remaining` of the `posts` lever and propose what fits.
3. **Estimate, then spend**: add `--dry-run` first. Above 10 posts (or above
   what is left) the CLI exits **4** with the estimate and spends nothing;
   show it and rerun with `--yes` only after the person says yes.
4. **Generate → show 2 or 3 → then the rest**: `kleos posts show <id> --json`
   until they leave `gerando`; show slides, caption and `url`.
5. **Never publish without the person's go-ahead** (`posts approve`,
   `posts schedule`), unless the account is in automatic mode.
6. **Ask Kleos for changes, don't rewrite**:
   `kleos call revise_post --args '{"post":"<id>","instruction":"<what to change>"}' --json`.
7. **Don't wait in a loop you wrote**: `kleos jobs wait <job_id> --json` polls
   for you (exit 0 done, 1 failed).
8. **Always give the person the `url`** from the result.

## Recipes

### New week

```bash
kleos projects list --json
kleos agents list --project habi --json
kleos usage --json
kleos week plan --project habi --days 7 --per-day 2 --agent bia --dry-run --json
# after the person agrees:
kleos week plan --project habi --days 7 --per-day 2 --agent bia --yes --json
```

### Launch of a new version

```bash
kleos call get_project --args '{"project":"habi"}' --json
kleos call list_playbooks --args '{"project":"habi"}' --json
kleos posts generate --project habi --agent bia --count 3 --format carousel --playbook <id> --dry-run --json
kleos posts generate --project habi --agent bia --count 3 --format carousel --playbook <id> --json
kleos posts show <post_id> --json          # show 2–3 to the person
kleos posts schedule <post_id> --account tiktok --at 2026-09-30T18:00:00-03:00 --json
```

### Daily review of the queue

```bash
kleos posts list --project habi --status pronto --unscheduled --json
kleos posts show <post_id> --json
kleos posts approve <post_id> <post_id> --json    # only what the person approved
```

### Weekly report

```bash
kleos metrics --project habi --since 7d --json
kleos usage --json
```

Numbers the network didn't report come as `null`: say "—", never 0. Metrics
arrive 1 to 48 h late.

## Anything else

`kleos call <tool> --args '<json>' --json` calls any MCP tool by name. The list
of tools with parameters and examples: https://ugc.zurc.app/llms-full.txt
