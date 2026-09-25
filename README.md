# Kleos skills

Skills and the Claude Code plugin for [Kleos](https://ugc.zurc.app): generate, schedule and measure UGC-style TikTok and Instagram posts for your app from any AI agent.

- **MCP server:** `https://mcp.zurc.app/mcp` (one-click login, or an API key from Settings › API keys)
- **CLI:** `npx kleos`
- **Docs for agents:** https://ugc.zurc.app/docs/agentes · https://ugc.zurc.app/llms.txt

| Skill | Use when |
|---|---|
| [`kleos`](skills/kleos/SKILL.md) | your agent has the Kleos MCP server connected |
| [`kleos-cli`](skills/kleos-cli/SKILL.md) | your agent runs shell commands (OpenClaw, CI, servers) |

## Install

**Claude Code** (plugin: MCP + skill + `/kleos-semana`)
```
/plugin marketplace add RhuanCruz/kleos-skills
/plugin install kleos@kleos
```

**Any agent with Agent Skills** (Claude Code, Codex, Cursor, Gemini CLI…)
```
npx skills add RhuanCruz/kleos-skills
```

**Hermes**
```
hermes skills install RhuanCruz/kleos-skills/skills/kleos
# or: hermes skills tap add RhuanCruz/kleos-skills
```

**OpenClaw**
```
openclaw skills install kleos-cli
# or: openclaw skills install git:RhuanCruz/kleos-skills
```

**The CLI installs them too:** `npx kleos skill install`
