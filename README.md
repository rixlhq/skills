# Rixl Skills

An [Agent Skill](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)
and plugin for reading Rixl media, analytics, billing and content data through the Rixl MCP server.

## Installing

### Claude Code

```
/plugin marketplace add rixlhq/skills
/plugin install rixl@rixl
```

You will be asked for a Rixl API key. Create one in the dashboard under Project > API Keys with
the `mcp-readonly` policy attached.

### Codex

```sh
codex plugin marketplace add rixlhq/skills
codex plugin add rixl@rixl
```

Start a new Codex session after installation.

### Cursor

**Settings > Rules > Add Rule > Remote Rule (Github)** with `rixlhq/skills`.

### npx skills

```sh
npx skills add https://github.com/rixlhq/skills
```

### Local development

```
/plugin marketplace add /path/to/skills
/plugin install rixl@rixl
```

Set the MCP endpoint option to `http://localhost:8080/mcp` to point at a gateway started with
`docker compose up -d`.

## Skills

| Skill | Useful for |
|-------|------------|
| rixl  | Videos, images, feeds, posts, playback and engagement metrics, dashboards, invoices, usage, members, access policies, support tickets |

## MCP Server

| Server | Purpose |
|--------|---------|
| rixl   | Read-only access to the Rixl API via `search_endpoints` and `call` |

Read-only. Only GET operations are exposed, so nothing can create, change or delete.
