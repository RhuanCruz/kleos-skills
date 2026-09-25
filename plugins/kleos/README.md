# Kleos plugin for Claude Code

One install with:

- the Kleos MCP server (`https://mcp.zurc.app/mcp`) with **browser login** (OAuth): no key to create or copy;
- the `kleos` skill (the method: resolve the project, read the plan, estimate, generate, show, only then schedule);
- the `/kleos-semana` command (plan the week with an estimate and confirmation).

## Install

```
/plugin marketplace add RhuanCruz/kleos-skills
/plugin install kleos@kleos
```

Then run `/mcp`, pick **kleos** and **Authenticate**. The browser opens on Kleos: sign in, choose the organization, the project and what Claude may do (read, generate, publish), and approve. Disconnect any time in Settings › API keys › Connected apps.

`KLEOS_MCP_URL` overrides the address.

### Without a browser (servers, CI)

The plugin sends no key on purpose (a fixed `Authorization` header would block the browser login). With an API key, add the server by hand instead:

```bash
claude mcp add --transport http --scope user kleos https://mcp.zurc.app/mcp --header "Authorization: Bearer $KLEOS_API_KEY"
```
